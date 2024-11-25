# Problem2 - Distributed Lock

## Distributed Lock nedir.?

Birden fazla bağımsız süreç (örneğin farklı sunucular, hizmetler veya uygulamalar) arasında paylaşılan bir kaynağa aynı 
anda erişimi engellemek için kullanılan bir mekanizmadır. Bu mekanizma, belirli bir kaynağı aynı anda sadece tek bir 
sürecin kullanabilmesini sağlar. Amaç, kaynak kullanımında çakışmaları ve veri tutarsızlıklarını önlemektir.

Bir sistemde birden fazla işlem aynı veri kaynağına (veritabanı, dosya sistemi, bellek içi veri deposu gibi) erişip 
işlem yapmaya çalıştığında veri tutarsızlıkları, çakışmalar veya hata durumları meydana gelebilir. Distributed locks, bu 
tür çakışmaları önleyerek aşağıdaki iki temel işlevi sağlar:

* **Mutual Exclusion:** Aynı anda yalnızca bir işlem veya süreç belirli bir kaynağı kullanabilir.
* **Consistency:** Bir işlem lock'u aldığında, o kaynak üzerinde güvenli bir şekilde işlem yapabilir ve kilit bırakılana 
kadar başka bir işlem bu kaynağa erişemez.

<b>-></b> Distributed Lock, dağıtık bir sistemde (birden fazla makine veya süreç içeren) tüm katılımcı sistemlerde aynı 
kaynağa erişim kontrolünü sağlar.

<b>-></b> Bir işlem kaynağa erişmeden önce kilit alır (lock acquire). Kaynağa erişimi bitirdiğinde kilidi serbest bırakır 
(lock release).

<b>-></b> Kilidi alan bir işlem başarısız olursa, diğer işlemler sonsuza kadar bekleyebilir. Bu nedenle, kilitlere 
genellikle bir zaman aşımı (timeout) verilir. Eğer kilit belirlenen süre içinde serbest bırakılmazsa, kilit otomatik 
olarak kaldırılabilir.

## Güvenlik ve Canlılık Garantileri
Distributed Lock'u sağlıklı bir şekilde sağlamak için aslında sadece 3 tane özellik kullanarak modellemek yeterli olacaktır.

* **Safety Property (Mutual Exclusion/Karşılıklı Dışlama):** Belirli bir anda, yalnızca bir istemci bir kilidi tutabilir.
Mutual Exclusion terimi, bir kaynağa aynı anda yalnızca bir istemcinin erişebilmesini garanti altına alır. Bu, özellikle 
dağıtık sistemlerde kritik öneme sahiptir. Dağıtık bir sistemde aynı kaynağa aynı anda birden fazla istemcinin erişmesi 
veri tutarsızlıklarına ve sistem hatalarına yol açabilir. Bu güvenlik özelliği, yalnızca bir istemcinin belirli bir anda 
kilidi tutabileceğini, diğer istemcilerin bu kilidi alabilmesi için beklemesi gerektiğini ifade eder. Böylece veri bütünlüğü 
sağlanmış olur.

* **Canlılık Özelliği A (Deadlock Free):** Bir istemci bir kaynağı kilitlemiş olsa bile, çökerse ya da ağda bir bölünme 
yaşanırsa, eninde sonunda bir kilidi edinmek her zaman mümkün olmalıdır. Bu özellik, bir istemcinin kaynağı kilitledikten 
sonra, eğer istemci çöker ya da bağlantısı koparsa diğer istemcilerin kilidi sonsuza kadar beklemesinin önüne geçer. Kilidi 
almış olan istemci kaynağa erişemediğinde, sistem `tıkanıklık (deadlock) durumuna düşebilir. Deadlock, bir sistemin 
işleyişinin durmasına neden olur çünkü hiçbir istemci kilidi alamaz. Ancak bu canlılık özelliği, sistemin bir şekilde kilidi 
serbest bırakabileceğini garanti eder, böylece diğer istemciler sonunda bu kilidi alabilir.

* **Canlılık Özelliği B (Fault Tolerance):** Redis node'larının çoğunluğu ayakta olduğu sürece, istemciler kilitleri alabilir 
ve bırakabilir. Bir Redis kümesinde (cluster) birçok düğüm (node) olabilir ve bu düğümlerin bazıları çeşitli nedenlerle 
devre dışı kalabilir (ağ sorunları, sunucu çökmesi vs.). Ancak, çoğunluk (majority) prensibine dayanan bir hata toleransı 
mekanizmasıyla, sistemdeki düğümlerin çoğunluğu ayakta olduğu sürece istemciler kilitleri alabilir ve serbest bırakabilir.
Bu özellik, sistemin kısmi hatalara rağmen çalışmaya devam etmesini sağlar ve dağıtık sistemlerde yüksek erişilebilirlik 
(high availability) sağlamanın önemli bir parçasıdır. Redis gibi dağıtık veri yapılarına sahip sistemlerde bu özelliğin 
sağlanması, veri kaybını önler ve sistemin sürekli işlevsel kalmasına yardımcı olur.

## Failover Tabanlı Uygulamalar neden güvenli değildir.?
Redis kullanarak bir kaynağı kilitlemenin en basit yolu, bir instance'ta bir anahtar (key) oluşturmaktır. Bu key genellikle 
sınırlı bir yaşam süresiyle (time to live) oluşturulur, böylece Redis'in zaman aşımı özelliğini (expires feature) kullanarak 
sonunda kilit serbest bırakılır. İstemci kaynağı serbest bırakmak istediğinde, anahtarı siler.
Redis kullanarak bir kaynak üzerinde kilit oluşturmak için, genellikle anahtar (key) bir Redis instance'ına yazılır. 
Ancak bu durumda Redis master düğümü, kilit işlemlerinin merkezinde yer aldığı için, master düğümün çökmesi tüm kilit 
mekanizmasını etkiler. Bu, karşılıklı dışlama (mutual exclusion) özelliğini tehdit eden bir durumdur çünkü sadece bir 
istemci kilidi elinde tutmalıdır ve master düğüm çökerse bu garanti sağlanamayabilir.

Yüzeysel olarak bu yöntem iyi çalışıyor gibi görünse de, bir sorun vardır: Mimarimizde single point of failure oluşmaktadır.
`Single Point of Failure` "bir bileşenin çökmesi durumunda tüm sistemin etkilenmesine yol açan zayıf bir noktadır." anlamına
gelmektedir. Peki ya Redis master düğümü devre dışı kalırsa ne olur? O halde bir replika ekleyelim! Ve master düğüm 
kullanılmaz hale geldiğinde replikayı kullanalım. Ne yazık ki bu, uygulanabilir değildir.  Bunu yaparak mutual exclusion 
güvenlik özelliğimizi sağlayamayız çünkü Redis replikasyonu asenkron çalışmaktadır.

Bu modelde bir race condition vardır:

* İstemci A, master düğümde kilidi alır.
* Master düğüm, anahtarı replikaya iletmeden önce çöker.
* Replika master olarak terfi eder.
* İstemci B, zaten İstemci A'nın kilitlediği aynı kaynağın kilidini alır. GÜVENLİK İHLALİ!

## Single Instance ile Doğru Uygulama