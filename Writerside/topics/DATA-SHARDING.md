# DATA SHARDING

* Redis Cluster, tutarlı hashing kullanmaz, ancak her key'in kavramsal olarak bizim _hash slot_ dediğimiz bir parçası 
olduğu farklı bir sharding formu kullanır.

* Redis Cluster, verileri birden çok node arasında dağıtmak için **_"consistent hashing"_** yerine, **_"hash slot"_** adı 
verilen bir yöntem kullanır.

* Burada her "key", belirli bir "hash slot"a atanır ve bu sayede verilerin hangi node'da saklanacağı belirlenir. 
Bu yöntem, verilerin dağıtımını ve erişimini optimize ederken, yüksek kullanılabilirlik ve ölçeklenebilirlik sağlar.

* "Sharding" ise genel olarak büyük bir veri kümesini daha küçük parçalara bölme ve bu parçaları farklı 
sunucularda (node) saklama işlemine denir, bu sayede sistem daha verimli çalışır.

* Redis Cluster'da toplam 16384 adet hash slot bulunur. Bir anahtarın hangi slot'a düşeceğini belirlemek için, öncelikle 
anahtarın CRC16 checksum'ı hesaplanır. CRC16, verinin doğruluğunu kontrol etmek için kullanılan bir tür hash fonksiyonudur. 
Bu işlem, verilen anahtarın bir sayısal değere dönüştürülmesini sağlar. Daha sonra bu sayısal değer, 16384 ile mod 
alınarak, anahtarın hangi hash slot'una ait olacağı belirlenir. Bu yöntem, Redis Cluster'ın verileri dağıtırken 
kullandığı temel mekanizmadır ve verilerin cluster içindeki node'lar arasında dengeli bir şekilde dağıtılmasını sağlar.

* Redis Cluster, verileri birden fazla node arasında dağıtmak için hash slotlarını kullanır. Toplamda 16384 hash slot 
bulunur ve her node, bu slotların belirli bir bölümünden sorumludur. Her bir node belirli bir hash slot aralığına 
sahiptir. Bu yapı, verilerin dengeli bir şekilde dağıtılmasını ve yüksek erişilebilirlik ile ölçeklenebilirliği sağlar. 
Bir anahtarın hangi node'da saklanacağı, anahtarın hangi hash slotuna düştüğüne bağlıdır ve bu da anahtarın CRC16 değeri 
ile belirlenir. Bu yöntem, her node'un cluster içindeki veri yükünün bir kısmını üstlenmesini ve böylece daha verimli 
bir veri yönetimi sağlar. 

Örneğin;


1. Node A, 0'dan 5500'a kadar olan hash slotlarını içerir.
2. Node B, 5501'den 11000'e kadar olan hash slotlarını içerir.
3. Node C, 11001'den 16383'e kadar olan hash slotlarını içerir.


Bu, cluster node'larını eklemeyi ve çıkarmayı kolaylaştırır. Örneğin, yeni bir node D eklemek istiyorsak, A, B, C 
node'larından bazı hash slotlarını D'ye taşımamız gerekiyor. Benzer şekilde, cluster'dan node A'yı çıkarmak istiyorsak, 
A tarafından hizmet verilen hash slotlarını B ve C'ye taşıyabiliriz. Node A boşaldığında, onu cluster'dan tamamen 
çıkarabiliriz.

Aslında bu durum Redis Cluster'ın esnekliğini ve ölçeklenebilirliğini vurgular. Node'ların eklenmesi veya çıkarılması 
durumunda, ilgili hash slotlarının diğer node'lara yeniden dağıtılması gerekmektedir. Yeni bir node eklemek istendiğinde, 
mevcut node'lardan seçilen hash slotları yeni node'a taşınır. Bu işlem, veri dağılımını yeniden dengeler ve yeni node'un da 
cluster'a katkıda bulunmasını sağlar. Benzer şekilde, bir node'un çıkarılması gerektiğinde, o node tarafından hizmet 
verilen hash slotları, cluster'daki diğer node'lara taşınır. Bu taşıma işlemi tamamlandığında ve node artık hiçbir 
hash slotuna hizmet vermiyorsa, node güvenli bir şekilde cluster'dan çıkarılabilir. 
Bu süreçler, Redis Cluster'ın dinamik bir yapıda çalışmasını ve gerektiğinde kolayca ölçeklenmesini sağlar.

* Redis Cluster, tek bir komutun yürütülmesinde (veya tüm işlemde veya Lua script yürütülmesinde) yer alan tüm key'ler
aynı hash slot'una ait olduğu sürece çoklu anahtar işlemlerini destekler. Kullanıcı, birden fazla anahtarı aynı 
hash slot'unun bir parçası yapmayı zorlamak için hash tag adı verilen bir özelliği kullanabilir.

* Hash tag özelliği, anahtar adlarında {} parantezleri içine alınan belirli bir alt dizinin hash işlemine tabi tutulması 
anlamına gelir. Bu yöntemle, parantezler içindeki string aynı olan anahtarlar aynı hash slot'una atanır. 
Örnekte verilen user:{123}:profile ve user:{123}:account anahtarları, {123} hash tag'ini paylaştıkları için aynı hash 
slot'una düşer. Bu, Redis Cluster'ın bu anahtarlar üzerinde atomik işlemler yapmasını sağlar, çünkü her iki anahtar da 
aynı node üzerinde bulunur. Bu özellik, ilişkili veri setleri üzerinde işlemler yapılırken veri tutarlılığını korumak ve 
işlemleri daha verimli hale getirmek için önemlidir. Bu, özellikle veritabanı işlemlerinde, script yürütülmesinde veya 
işlemlerde birden fazla anahtarın kullanılması gerektiğinde yararlıdır.