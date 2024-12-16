# auto aof rewrite percentage ve auto aof rewrite min size 

redis.conf dosyalarındaki tanımı

> auto-aof-rewrite-percentage 100
> 
> auto-aof-rewrite-min-size 64mb

AOF dosyası zamanla büyür. Bu ayarlar, AOF dosyası çok büyüdüğünde otomatik olarak kendini yeniden yazarak (BGREWRITEAOF) 
optimize etmesi içindir.

**`auto-aof-rewrite-percentage 100:`** Eğer AOF dosyası, son yeniden yazma işleminden bu yana boyutunu %100 (yani 2 katına) 
çıkardıysa yeniden yazma işlemi tetiklenir.

**`auto-aof-rewrite-min-size 64mb:`** Ancak yeniden yazmanın tetiklenebilmesi için dosyanın en az 64 MB olması gerekir. 
Yani dosya küçükken boşa yazma işlemi yapmaz.

Bu sayede AOF dosyası gereksiz yere büyüyüp performans düşüşüne yol açmaz. Periyodik olarak kendini optimize eder.