# REDIS DATABASE (RDB)

Bir .rdb dosyası, bir Redis instance'ında saklanan verileri temsil eden zamansal bir noktayı içeren bir binary dosyasıdır. 
RDB dosya formatı, hızlı okuma ve yazma işlemleri için optimize edilmiştir. Gerekli performansı sağlamak için, bir .rdb 
dosyasının disk üzerindeki iç temsili, Redis'in bellek içi temsiline çok benzerdir.

RDB'nin bir başka ilginç yönü ise, bir RDB dosyasını oldukça kompakt hale getirebilmek için LZF sıkıştırmasını 
kullanabilmesidir. LZF sıkıştırması, sıkıştırma sırasında çok küçük bir bellek gereksinimi olan hızlı bir sıkıştırma 
algoritmasıdır. Diğer sıkıştırma algoritmalarına kıyasla en iyi sıkıştırma oranlarına sahip olmamasına rağmen, Redis ile 
etkili bir şekilde çalışır. Ayrıca, tek bir RDB dosyası, bir Redis instance'ını tamamen geri yüklemek için yeterlidir.

RDB, ihtiyaçlarımıza göre her saat, gün, hafta veya ayda bir RDB dosyası kaydetmemize olanak tanıdığı için yedekleme ve 
felaket kurtarma için mükemmeldir. Bu yaklaşım, herhangi bir veri setini herhangi bir zamanda kolayca geri yüklemek için 
RDB dosyalarını kullanmamıza imkan tanır.

Daha detaylı bir açıklama olarak;
RDB, Redis veritabanı sisteminde kullanılan bir yedekleme yöntemidir. Bu yöntem, veritabanındaki tüm verilerin anlık 
görüntüsünü alır ve bu bilgileri bir dosyada saklar. RDB'nin güçlü yanı, yedeklemenin yapılandırılabilir olmasıdır; 
yani kullanıcılar, veri yedeklemelerinin ne sıklıkta yapılacağını (her saat, gün, hafta veya ayda bir gibi) 
özelleştirebilirler. Bu esneklik, farklı ihtiyaç ve kaynaklara sahip kuruluşlar için idealdir.

RDB dosyaları, veri kaybı yaşandığında hızlı ve etkili bir şekilde veri kurtarma sağlar. Örneğin, bir sistem çökmesi veya 
veri bozulması durumunda, son yedekleme noktasından itibaren verilerinizi geri yükleyebilirsiniz. Bu, iş sürekliliğini 
sağlamak ve olası veri kaybının etkilerini en aza indirmek için çok önemlidir.

RDB'nin avantajlarından biri de, yedekleme işleminin performans üzerinde minimal etkiye sahip olmasıdır. Yedekleme işlemi 
arka planda çalışır ve veritabanı performansını önemli ölçüde etkilemez. Bu, özellikle büyük veri setleriyle çalışan ve 
yüksek performans gereksinimleri olan sistemler için önemlidir.

**"SAVE"** komutu, Redis'te hemen bir RDB dosyası oluşturur, ancak anlık görüntü oluşturma işlemi sırasında Redis 
sunucusunu bloke ettiği için kullanılmaması önerilir. Bunun yerine, **"BGSAVE"** (arka plan kaydetme) komutu 
kullanılmalıdır; "SAVE" ile aynı etkiye sahiptir ancak Redis'i bloke etmemesi için bir alt süreçte çalışır.

Redis'in veri kalıcılığı stratejilerinden biri olan BGSAVE işlemi, veri kaybı riskini minimize ederken sunucu performansını 
korumak için tasarlanmıştır. Ana Redis süreci, BGSAVE komutu alındığında, tüm veritabanı durumunu diske yazmak üzere bir 
child process oluşturur. Bu child process, main processin bellek içeriğinin bir kopyasını alır ve bu kopyayı kullanarak RDB 
dosyasını oluşturur. Bu process sırasında, main process disk I/O işlemleri gerçekleştirmez, böylece istemci isteklerine 
yanıt vermeye devam edebilir. Ancak, child processin bellek kopyasını oluşturduğu sırada, main process halen yazma 
işlemleri alabilir. Bu durumda, Redis'in **"copy-on-write"** mekanizması devreye girer. Ana süreç tarafından değiştirilen 
herhangi bir bellek sayfası, child process'e yazılmadan önce kopyalanır. Bu, child process'in bellek içeriğinin 
tutarlılığını korur, ancak aynı zamanda ana sürecin yazma işlemleri sırasında değiştirilen bellek miktarına bağlı olarak 
toplam bellek kullanımının artmasına neden olabilir.

copy-on-write mekanizması, bellek verimliliği ve sistem performansı arasında bir denge sağlar; ancak, büyük veritabanları 
veya yoğun yazma trafiği olan durumlarda, BGSAVE işlemi sırasında sistem bellek kullanımının önemli ölçüde artabileceğinin 
farkında olmak önemlidir. Bu nedenle, Redis yapılandırması ve kaynak yönetimi, veritabanı boyutu, trafiğin yoğunluğu ve 
beklenen sistem yükü dikkate alınarak dikkatli bir şekilde planlanmalıdır.

Redis'in varsayılan yapılandırma dosyası, Redis kaynak kodu dizininde bulunur ve diskte veri kalıcılığını sağlamak için 
"save" direktifi aracılığıyla arka plan kaydetmeleri gerçekleştiren üç anlık görüntü kuralını etkinleştirir.
Bu teknik "snapshotting" (anlık görüntüleme) olarak adlandırılır.


Redis'teki birden fazla "save" direktifi, snapshot'ların ne sıklıkla ve ne zaman kaydedileceği konusunda büyük bir 
esneklik sağlar. Bununla birlikte, herhangi bir zaman aralığında ihtiyaç duyulan kadar çok "save" direktifi kullanılabilir. 
_Ancak, "save" direktiflerini birbirinden 30 saniyeden daha kısa aralıklarla kullanmanız önerilmez._ Snapshot işlemi, 
tüm "save" direktiflerini redis.conf dosyasından silerek veya yorumlayarak ve ardından Redis sunucusunu yeniden başlatarak 
devre dışı bırakılabilir. Ayrıca bir komut satırı seçeneği veya **"CONFIG SET"** komutu aracılığıyla da devre dışı bırakılabilir.

Ancak, RDB her ne kadar dakikada bir snapshot kaydedilse ve en az 100 değişiklikle bile, %100 garantili bir veri kurtarma 
yöntemi değildir. Ana Redis süreci herhangi bir nedenle durursa, veritabanımızdaki son yazımları kaybetmeye hazır olalım. 
RDB'nin bir diğer dezavantajı ise, her snapshot oluşturmanız gerektiğinde, Redis main processinin, verilerin diske kalıcı 
olarak yazılmasını sağlamak için bir child process oluşturmak üzere bir fork() işlemi gerçekleştirmesidir. Bu durum, Redis 
instance'ının, donanıma ve veri setinin boyutuna bağlı olarak, müşterilere hizmet vermeyi milisaniyeler, bazen de birkaç 
saniye boyunca durdurmasına neden olabilir.

Bu nedenle, Redis yapılandırmanızı planlarken, sisteminizin gereksinimlerini ve kısıtlamalarını dikkate almak önemlidir. 
Snapshot stratejimizi, veri kaybı riskini minimize ederken performansı optimize edecek şekilde dikkatli bir şekilde 
dengelememiz gerekir.

### Avantajları
* RDB, Redis verilerinizin tek dosyalık, zaman noktasındaki çok kompakt bir temsilidir. RDB dosyaları yedekleme için 
mükemmeldir. Örneğin, son 24 saat için her saat başı RDB dosyalarınızı arşivlemek ve 30 gün boyunca her gün bir RDB anlık 
görüntüsü kaydetmek isteyebiliriz. Bu, felaket durumlarında veri setinin farklı sürümlerini kolayca geri yüklememize 
olanak tanır.

* RDB (Redis Database) formatı, felaket kurtarma senaryoları için çok uygundur çünkü tek, kompakt bir dosya şeklinde olup, 
uzak veri merkezlerine aktarılabilir veya Amazon S3 gibi bulut depolama hizmetlerine (isteğe bağlı olarak şifrelenmiş 
bir şekilde) yüklenebilir.

* RDB (Redis Database), Redis performansını en üst düzeye çıkarmak için tasarlanmış bir kalıcılık mekanizmasıdır. 
Kalıcılığı sağlamak için Redis main process'inin yapması gereken tek iş, geri kalan her şeyi yapacak bir child process 
oluşturmaktır. Main Process, disk I/O işlemleri gibi işlemleri asla gerçekleştirmez.

* RDB, AOF'ye kıyasla büyük veri set'lerinde daha hızlı yeniden başlatmalara olanak tanır.


### Dezavantajları
* RDB, Redis'in çalışmayı durdurması durumunda (örneğin bir güç kesintisi sonrası) veri kaybı ihtimalini en aza indirmek 
istediğinizde iyi bir seçenek değildir. Farklı kaydetme noktaları yapılandırabilirsiniz ki bir RDB oluşturulsun 
(örneğin, en az beş dakika ve veri setine karşı 100 yazma işlemi sonrasında, birden fazla kaydetme noktasına sahip 
olabilirsiniz). Ancak genellikle her beş dakikada veya daha uzun sürelerde bir RDB snapshot oluşturacaksınız, bu nedenle 
herhangi bir nedenle doğru bir şekilde kapanmadan Redis'in çalışmayı durdurması durumunda, son dakikalardaki verileri 
kaybetmeye hazır olmalısınız.


* RDB, diskte kalıcılığı bir child process kullanarak sağlamak için sık sık _fork()_ işlemi yapması gerektirir. Veri seti 
büyükse, fork() işlemi zaman alıcı olabilir ve veri seti çok büyükse ve CPU performansı yeterince iyi değilse, Redis'in 
müşterilere hizmet vermeyi birkaç milisaniye veya hatta bir saniye boyunca durdurması sonucunu doğurabilir. 
AOF da fork() işlemi yapar ancak daha az sıklıkta ve güvenilirlikten ödün vermeden günlüklerinizi ne sıklıkta yeniden 
yazmak istediğinizi ayarlayabilirsiniz.