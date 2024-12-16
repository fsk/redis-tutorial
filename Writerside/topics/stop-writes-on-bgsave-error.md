# stop writes on bgsave error

redis.conf dosyasındaki tanımı

> stop-writes-on-bgsave-error yes

Bu ayarın amacı, arka planda yapılan veri kaydetme (RDB dosyası oluşturma) işlemi eğer başarısız olursa (örneğin diske 
yazılamıyorsa, izinler kısıtlıysa, disk doluysa), Redis’in yeni yazma işlemlerini durdurup durdurmayacağıdır.

**`yes:`** Eğer arka plan kaydetme işlemi hata verirse, Redis yeni verileri yazmayı durdurur. Bu sayede bir sorun 
olduğunu hemen fark edebiliriz. Çünkü yeni veri yazılmadığından service'de aksama yaşanır ve bu soruna dikkat çekmiş oluruz.

**`no:`** Arka plan kaydetme işlemi başarısız olsa bile Redis verileri yazmaya devam eder. Yani sorun var ama veri de 
yazılıyor; belki geç fark edilebilir. Bu bazı ortamlarda istenebilir, mesela diskte sorun çıkmış ama siz yine de çalışmaya 
devam etmek istiyorsanız bu ayarı kullanabilirsiniz.
