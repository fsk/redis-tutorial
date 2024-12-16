# save

Redis için sadece `in-memory cache` demek redis'e çok ciddi bir haksızlıktır. Redis'in özelliklerinden
birisi de redis'i database olarak kullanmaktır.

Redis bir veritabanıdır ve verileri bellekte (RAM) tutar. Ancak sunucu kapandığında veya beklenmedik bir sorun yaşandığında 
verilerimizi kaybetmek istemeyiz. Bu nedenle Redis, belirli aralıklarla veya belirli sayıda değişiklikten sonra verileri 
diske yazar `(bu işleme "snapshot alma" veya "RDB kaydetme" denir)`. Diske yazılan bu dosya, sunucu yeniden başladığında 
verilerinizi geri getirebilir.

Zaten redis.conf dosyasını incelediğimizde de karşımıza `SNAPSHOT` başlığı altında bu configler gelir. Yani redis'te
bir şeyleri kaydedebilmek aslında snapshot amak demek.

redis.conf dosyasındaki tanımı  

> save ""

Bu yapı aslında aşağıdaki formülden türer.

**`save <seconds> <changes> [<seconds> <changes> ...]`**

Yukarıdaki config ile aslında redis'e şunu söylüyoruz.

`Belirtilen süre kadar zaman geçtiğinde ve bu süre içinde belirtilen sayıda veri değişikliği (yazma işlemi) gerçekleştiyse, 
diske bir kaydetme işlemi yap.`

Örn:

`save 3600 1`

Bu, eğer son kaydetme işleminden itibaren en az 3600 saniye (1 saat) geçmişse ve bu sürede en az 1 veri değişikliği 
yapılmışsa, Redis otomatik olarak diske bir snapshot alır demektir.

Tabi burada birden fazla kural da ekleyebiliriz.

`save 3600 1 300 100 60 10000`

Bu satır, aşağıdaki anlamları taşır:
* 3600 saniye (1 saat) geçip en az 1 değişiklik olduysa kaydet
* veya 300 saniye (5 dakika) geçip en az 100 değişiklik olduysa kaydet
* veya 60 saniye (1 dakika) geçip en az 10,000 değişiklik olduysa kaydet

Redis bu koşullardan herhangi biri sağlandığında diske kaydeder.

redis.conf dosyasındaki tanım ise (`save ""`) otomatik olarak snapshot almayı durdurur.

## Hata Durumu

Varsayılan olarak eğer RDB snapshot özelliği açıksa (yani en az bir save kuralı varsa) ve son "arka plan" kaydetme işlemi 
başarısız olmuşsa, Redis yeni yazma işlemlerini durdurur. Çünkü veriler diske düzgün kaydedilemiyorsa, sistemin sessizce 
bozuk bir duruma düşmesini istemeyiz. Bu bizi, bir sorun olduğunu fark edip müdahale etmeye zorlar. Örneğin disk dolmuş 
olabilir, izin sorunları olabilir. Ardından, eğer arka plan kaydetme işlemi tekrar düzelirse (sorun giderilir, sonraki 
kaydetme başarılı olur), Redis otomatik olarak yazma işlemlerini yeniden kabul etmeye başlar.