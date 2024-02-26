# AOF(Append-only-File)

AOF etkinleştirildiğinde, Redis veri setini değiştiren herhangi bir komut aldığında, bu komutu AOF'ye (append-only-file) 
ekler. Bununla birlikte, AOF etkinleştirilmiş ve Redis yeniden başlatılmışsa, Redis tüm komutları AOF'te listelenen sırayla 
çalıştırarak verileri geri yükler ve veri setinin durumunu yeniden oluşturur. AOF, RDB anlık görüntüleme yöntemine bir 
alternatiftir. Redis 1.1'e kadar, Redis yalnızca snapshot stratejisini destekliyordu, bu da tamamen dayanıklı bir 
yaklaşım değildir. Redis 1.1 ile AOF, tamamen dayanıklı bir strateji olarak tanıtılmıştırr.

AOF, "insan tarafından okunabilir" bir "append-only" mantığına dayalı log dosyasıdır. Bu, herhangi bir arama işlemi olmadığı 
ve bozulma sorunlarının kolayca tespit edilebildiği anlamına gelir. Bu dosyayı bir metin düzenleyicide açabilir ve 
içindekileri ankayabiliriz (bu, ikili bir dosya olan RDB dosyası ile mümkün değildir). AOF hakkında dikkat çekici bir nokta, 
AOF ne sebeple olursa olsun eksik veya bozuk olduğu durumlarda bile, AOF dosyalarını kolayca kontrol edip düzelten 
_'redis-check-aof'_ adında bir aracın bulunmasıdır. Ancak, bu özellik performans ve ek disk alanı pahasına gelir.

AOF, birkaç seçenek aracılığıyla otomatik olarak kendisinin daha küçük bir versiyonuna optimize edilebilir veya 
**BGREWRITEAOF** komutu aracılığıyla manuel olarak optimize edilebilir. Yeniden yazma sırasında bir çökme durumunda, 
orijinal AOF değiştirilmez.

## AOF Command'leri

### appendonly
AOF'yi enable ya da disable eder. Sadece _yes_ ya da _no_ parametresi alır. Default olarak AOF disable'dır.

### appendfilename
AOF dosya adı belirtmemizi sağlar. Sadece filename içindir, path için değildir. Default olarak boştur.

### appendfsync
Redis, main process'te _fsync()_ işlemini gerçekleştirmek için bir background thread kullanır (**fsync(), işletim 
sistemine verileri diske boşaltmasını söyleyen bir sistem çağrısıdır**). 
Redis, fsync politikasını üç olası şekilde yapılandırmanıza izin verir:
* **no:** _fsync()_ çalıştırmayın; verilerin ne zaman boşaltılacağına işletim sistemine karar versin. Bu en hızlı seçenektir.
* **always:** Her yazım işleminden sonra _fsync()_ çalıştırın. Bu en yavaş seçenektir, ancak aynı zamanda en güvenlidir.
* **everysec**: _fsync()_ işlemini her saniye bir kez çalıştırın. Bu, hala iyi yazma performansı sağlar. Bu varsayılan 
değerdir.

### no-appendfsync-on-rewrite
Bu seçeneğin alabileceği değerler **yes** ve **no**dur. Eğer _appendfsync_ politikası _her saniye (everysec)_ veya _her 
zaman (always)_ olarak ayarlanmışsa ve bir arka plan kaydetme veya AOF log yeniden yazımı gerçekleşiyorsa, Redis çok fazla 
disk I/O'su nedeniyle bloke olabilir (_fsync() sistem çağrısı uzun sürebilir_). Bu seçeneği yalnızca gecikme sorunlarınız 
varsa etkinleştirmelisiniz. Varsayılan değer no'dur.

### auto-aof-rewrite-min-size
Bu, AOF'nin yeniden yazılması için minimum boyuttur. Belirtilen minimum boyut ulaşılmadığı sürece, belirtilen 
_auto-aof-rewrite-min-size_ değeri aşılsa bile AOF yeniden yazmaları engellenir. Varsayılan değer 67,108,864 byte'tır.

### aof-load-truncated
Geçerli değerler **yes** ve **no**dur. Bir çökme durumunda, AOF kısaltılabilir ve bu seçenek, Redis'in başlangıçta 
kısaltılmış AOF'yi yükleyip yüklemeyeceğini veya bir hata ile çıkıp çıkmayacağını belirtir. Değer _yes_ olduğunda, 
Redis kısaltılmış dosyayı yükleyecek ve bir hata mesajı yayınlayacaktır. _no_ olduğunda ise, Redis bir hata ile çıkacak 
ve kısaltılmış dosyayı yüklemeyecektir.

### dir
RDB ve AOF için dosya yolu belirtmemizi sağlar.

## Avantajlar
* AOF kullanarak Redis çok daha dayanıklı hale gelir: farklı fsync politikalarına sahip olabilirsiniz: hiç fsync yapmama, 
her saniye fsync yapma, her sorguda fsync yapma. Varsayılan her saniye fsync politikası ile yazma performansı hala harika. 
fsync, bir background thread kullanılarak gerçekleştirilir ve main thread, fsync işlemi devam etmediği zamanlarda yazma 
işlemlerini gerçekleştirmek için yoğun çaba gösterir, böylece yalnızca bir saniyelik yazıları kaybedebilirsiniz.

* AOF log, sadece ekleme mantığına dayalı bir log'tur, bu yüzden güç kesintisi durumunda arama veya bozulma sorunları 
yoktur. Günlük herhangi bir nedenle (disk doluluğu veya başka nedenler) yarım yazılmış bir komutla biterse bile, 
_redis-check-aof_ aracı bunu kolayca düzeltebilir.

* Redis, AOF çok büyük olduğunda otomatik olarak arka planda yeniden yazabilir. Yeniden yazma işlemi tamamen güvenlidir 
çünkü Redis eski dosyaya ekleme yapmaya devam ederken, mevcut veri setini oluşturmak için gereken minimum işlem seti ile 
tamamen yeni bir dosya üretilir ve bu ikinci dosya hazır olduğunda Redis ikisini değiştirir ve yeni olana ekleme yapmaya 
başlar.

* AOF, tüm işlemleri kolayca anlaşılır ve ayrıştırılabilir bir formatta birbirinin ardına kaydeder. Hatta bir AOF 
dosyasını kolayca dışa aktarabilirsiniz. Örneğin, _FLUSHALL_ komutunu kullanarak her şeyi kazara sildiyseniz bile, 
bu süre zarfında log'un yeniden yazılması gerçekleştirilmemişse, sunucuyu durdurarak, en son komutu kaldırarak ve 
Redis'i tekrar başlatarak veri setinizi hala kurtarabilirsiniz.

## Dezavantajlar
* Aynı veri seti için AOF dosyaları, genellikle karşılık gelen RDB dosyalarından daha büyüktür.

* AOF, belirli _fsync_ politikasına bağlı olarak RDB'den daha yavaş olabilir. Genel olarak, _fsync_ her saniye olarak 
ayarlandığında performans hala çok yüksektir ve fsync devre dışı bırakıldığında, yüksek yük altında bile RDB kadar hızlı 
olmalıdır. Yine de RDB, büyük bir yazma yükü durumunda bile maksimum gecikme süresi hakkında daha fazla garanti sağlayabilir. 