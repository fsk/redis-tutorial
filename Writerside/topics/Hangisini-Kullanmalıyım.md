# Hangisini Kullanmalıyım.?

* Büyük bir veri setini kurtarırken, RDB'den veri geri yüklemek AOF'ye göre daha hızlıdır. Bunun nedeni, RDB'nin tüm 
veritabanında yapılan her değişikliği yeniden çalıştırmasına gerek olmamasıdır; sadece önceden depolanan verileri 
yüklemesi gerekmektedir. Örneğin, "pageview" adında bir key'imiz olduğunu ve değerinin 1 ile başladığını hayal edelim. 
Bir gün geçtiğini ve şimdi "pageview" değerinin 100.000 olduğunu varsayalım (bu ziyaret sayısına sahip olduğumuz ve her 
ziyaret için INCR komutunun çalıştırıldığını düşünebiliriz). Veritabanımızı AOF kullanarak geri yüklememiz gerekiyorsa, 
Redis key'inin son değerini elde etmek için 100.000 INCR komutu çalıştıracaktır. Ancak RDB kullanarak geri yükleme 
yapılırken, Redis hemen 100.000 değerine sahip bir anahtar oluşturacak, bu çok daha hızlıdır.

* Redis, dosyalardan herhangi biri varsa başlangıçta RDB veya AOF'yi yükler. Her iki dosya da varsa, AOF dayanıklılık 
garantileri nedeniyle önceliklidir. Redis'te kalıcılık kullanırken dikkate alınması gereken bazı hususlar şunlardır:
1. Uygulamanız kalıcılığa ihtiyaç duymuyorsa, RDB ve AOF'yi devre dışı bırakın.
2. Uygulamanız veri kaybına tolerans gösterebiliyorsa, RDB kullanın.
3. Uygulamanız tamamen dayanıklı kalıcılık gerektiriyorsa, hem RDB hem de AOF kullanın.

