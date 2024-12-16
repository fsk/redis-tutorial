# no appendfsync on rewrite 

redis.conf dosyasındaki tanımı

> no-appendfsync-on-rewrite no

Bu ayar, bir yandan diskte arka plan yazma işlemi `(BGSAVE veya BGREWRITEAOF)` yapılırken diğer yandan `fsync` çağrılarının 
nasıl davranacağını belirler.

**`no:`** Her zamanki gibi fsync yapılır.

**`yes:`** Arka plan yazma (BGSAVE/BGREWRITEAOF) devam ederken ana süreç fsync yapmaz. Bu, o anda veri kaybı riskini 
`“appendfsync no”` moduna çekebilir (yani 30 saniyeye kadar veri kaybı olabilir) ama bazen gecikmeyi azaltır.

Eğer gecikme sorunları yaşıyorsanız yes yapabilirsiniz, ama veri kaybı riski artar.