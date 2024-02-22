# REDIS CONFGIRUTAION PARAMETRELERİ

### cluster-enabled <yes/no>
Eğer `evet` seçilirse belirli bir Redis instance'ında Redis Cluster desteğini etkinleştirir. Aksi halde instance 
her zamanki gibi bağımsız bir instance olarak başlar.

### cluster-node-timeout <milliseconds>
Bu, bir Redis Cluster node'unun başarısız olarak kabul edilmeden önce ulaşılamaz olabileceği maksimum süredir. Eğer bir 
master node belirtilen süreden daha uzun süre ulaşılamaz durumda ise, replikaları tarafından _failover_ işlemine tabi 
tutulacaktır. Bu parametre, Redis Cluster'da diğer önemli işlevleri de kontrol eder. Özellikle, belirtilen süre boyunca 
çoğunlukta olan master node'lara ulaşamayan her node, sorguları kabul etmeyi durduracaktır.  Bu parametre, 
Redis Cluster'ın kullanıldığı ortama ve gereksinimlerine göre dikkatli bir şekilde ayarlanmalıdır, çünkü çok kısa bir 
timeout süresi gereksiz failoverlara, çok uzun bir süre ise gerçek hata durumlarında gereksiz gecikmelere neden olabilir.

### cluster-slave-validity-factor <factor>
Bu ayar, Redis Cluster'da replika node'ların master node'lara ne zaman ve nasıl failover yapacağını kontrol eder. 
Master ile replika arasındaki bağlantı kesintisi belirli bir süreyi aştığında, replikanın otomatik olarak failover 
işlemine başlamamasını sağlayarak hatalı failover işlemlerinin önlenmesine yardımcı olur. Bu, özellikle geçici ağ sorunları 
veya kısa süreli kesintiler sırasında gereksiz failover işlemlerinin ve bunun sonucunda oluşabilecek veri 
tutarsızlıklarının önlenmesi için önemlidir. Ancak, bu faktörün dikkatli bir şekilde ayarlanması gerekir, 
çünkü çok yüksek bir değer, gerçek bir master node başarısızlığında gerekli failover işleminin gecikmesine veya tamamen 
gerçekleşmemesine neden olabilir, bu da cluster'ın kullanılamaz hale gelmesine yol açabilir.

### cluster-migration-barrier <count>
Bu ayar, Redis Cluster'daki replika göçü mekanizmasını kontrol eder. _Replika göçü, bir master node'un artık replika 
tarafından kapsanmadığı durumlarda, yani tüm replikaları başarısız olmuş veya başka sebeplerle ulaşılamaz hale gelmişse, 
başka bir replikanın bu master'a göç etmesi sürecidir._ **cluster-migration-barrier** parametresi, bir replikanın başka 
bir master'a göç etmeye başlamadan önce, kaynak master'ın bağlı kalmış olması gereken minimum replika sayısını belirler.

Örneğin, cluster-migration-barrier değeri 1 olarak ayarlanmışsa, bir replikanın başka bir master node'a göç etmesi için, 
kaynak master'ın en az bir replikayla bağlantısının olması gerekir. Eğer bu sayı 0 olarak ayarlanırsa, herhangi bir 
replika bağlantısı olmadan da replika göçü gerçekleşebilir.

Bu mekanizma, Redis Cluster'da yüksek kullanılabilirliği ve veri bütünlüğünü sağlamaya yardımcı olur. Master node'ların 
replikasız kalmamasını sağlayarak, bir master başarısız olduğunda veya ağ bölünmesi yaşandığında veri kaybı riskini 
azaltır. Ancak, bu değerin belirlenmesi, cluster'ın boyutu, ağ yapısı ve kullanım senaryoları gibi faktörlere bağlı 
olarak dikkatli bir şekilde yapılmalıdır, çünkü çok yüksek bir engel, gereksiz yere replika göçünü engelleyebilir ve 
cluster'ın dayanıklılığını azaltabilir.

### cluster-require-full-coverage <yes/no>
Bu ayar, Redis Cluster'ın veri kapsamı ve kullanılabilirlik politikalarını yönetir. _cluster-require-full-coverage_ ayarı 
**"yes"** olarak ayarlandığında, cluster'ın tüm anahtar uzayının cluster'daki node'lar tarafından kapsanması gerektiğini 
belirtir. Eğer herhangi bir anahtar uzayı kapsanmazsa (örneğin, bir node başarısız olmuşsa ve o node'a atanmış hash 
slotlarına erişilemiyorsa), cluster yazma işlemlerini kabul etmeyi durdurur. Bu, veri tutarlılığını korumak için önemlidir, 
çünkü kapsanmayan anahtarlar üzerinde yapılan yazma işlemleri kaybolabilir veya tutarsız sonuçlara yol açabilir.

Ancak, "no" olarak ayarlandığında, cluster bazı anahtar uzaylarının kapsanmadığı durumlarda bile yazma işlemlerini kabul 
etmeye devam eder. Bu, özellikle yüksek kullanılabilirlik gereksinimleri olan senaryolarda kullanışlıdır çünkü cluster, 
bazı anahtarlar geçici olarak erişilemez olsa bile, kalan kısmı için hizmet vermeye devam edebilir. 
Bu ayar, kullanılabilirliği artırırken veri tutarlılığı risklerini de beraberinde getirebilir, bu yüzden kullanımı 
dikkatli bir şekilde değerlendirilmelidir.

### cluster-allow-reads-when-down <yes/no>
Bu ayar, Redis Cluster'ın kullanılabilirlik ve tutarlılık arasında nasıl bir denge kurduğunu gösterir. Varsayılan olarak, 
bir cluster başarısız olarak işaretlendiğinde, okuma işlemleri dahil olmak üzere tüm trafiği durdurarak potansiyel 
tutarsız veri okumalarını önler. Ancak, bu ayar **"evet"** olarak değiştirildiğinde, bir node arıza durumunda bile okuma 
işlemlerine hizmet vermeye devam edebilir. Bu, bazı uygulamalar için kullanışlı olabilir, özellikle de okuma işlemlerinin 
kullanılabilirliğinin, verilerin tam tutarlı olmasından daha önemli olduğu durumlarda. Ancak, bu esneklik, tutarsız 
verilere maruz kalma riski ile birlikte gelir ve bu nedenle, kullanımı belirli ihtiyaçlar ve risk toleransları göz önünde 
bulundurularak değerlendirilmelidir.