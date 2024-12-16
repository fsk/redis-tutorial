# aof timestamp enabled 

redis.conf dosyasındaki tanımı

> aof-timestamp-enabled no

AOF dosyalarına timestamp eklemeyi sağlayan bir ayardır. Bu, verileri belirli bir zamana kadar geri yüklemek gibi ileri 
seviye özellikler için kullanılabilir. Ancak bazen diğer araçlarla uyumluluğu bozabilir.

**`no:`** Timestamp eklenmez, uyumluluk sorunu olmaz.

**`yes:`** Timestamp eklenir, belirli bir noktaya geri dönme gibi özellikler kolaylaşır ama bazı eski AOF okuma 
araçlarıyla uyumsuzluk olabilir.