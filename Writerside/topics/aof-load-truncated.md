# aof load truncated

redis.conf dosyasındaki tanımı

> aof-load-truncated yes

Sunucu yeniden başlarken AOF dosyasını okuyup RAM’e yükler. Eğer dosya tam yazılamamış, sonunda eksik veri varsa bu 
`“truncated” (kesik)` bir dosya sayılır. Bu ayar, böyle bir durumda ne yapılacağını söyler.

**`yes:`** Dosya eksik ise bile elindeki veriyi yükler ve çalışmaya devam eder. Logda bu durum bildirilir. Veri eksik 
olabilir ama en azından bir kısmı kurtarılır.

**`no:`** Dosya eksikse Redis hata vererek başlatmayı reddeder. Bu durumda `redis-check-aof` gibi bir araçla dosyayı 
tamir etmek gerekebilir.

Varsayılan yes, kolaylık sağlar. 

no ise daha katıdır, tamir etmeden devam etmez.