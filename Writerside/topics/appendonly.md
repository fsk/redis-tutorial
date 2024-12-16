# appendonly

Redis bellek tabanlı bir veritabanıdır, yani veriler öncelikle RAM’de tutulur. Ancak RAM geçici bir bellektir, sunucu kapanırsa veriler kaybolur. Bunu önlemek için Redis diske de yedek (kopya) alır. Bu sayede sunucu yeniden başladığında verileri disketen geri yükleyebilir. Burada iki temel yaklaşım vardır:

* **`RDB (Snapshot) Kaydetme:`** Zaman zaman (örneğin belli aralıklarla veya belli değişiklik sayısından sonra) veriler 
diske bir `“anlık görüntü” (snapshot)` olarak yazılır. Bu yöntem hızlıdır ama en kötü senaryoda bu snapshot’tan sonra 
yapılan son yazma işlemleri kaybolabilir (belki birkaç dakika).

* **`AOF (Append Only File) Kayıt Tutma:`** Yapılan her değişiklik sırasıyla bir log dosyasına (append only file) yazılır. 
Bu yöntem genelde RDB’ye göre daha fazla veri saklama garantisi sunar (daha az veri kaybı).

Redis bu iki yöntemi birlikte de kullanabilir.

redis.conf dosyasındaki tanımı

> appendonly no

Bu ayar, AOF modunu etkinleştirip etkinleştirmeyeceğinizi belirler.

**`no:`** Sadece RDB kullanılır (varsayılan). Belirli aralıklarla disk yedekleri alınır.

**`yes:`** AOF devreye girer. Veriler her değişiklikte bir log dosyasına yazılır, bu daha yüksek dayanıklılık sağlar.

`Varsayılan olarak no seçili, yani AOF devre dışı. Sadece RDB ile çalışır.`