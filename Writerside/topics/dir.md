# dir

redis.conf dosyasındaki tanımı

> dir ./

Bu ayar, Redis’in RDB dosyası veya ek dosyaları nereye yazacağını belirler. Burada ./ ana klasöre (genellikle Redis’in 
çalıştığı dizine) işaret eder.

Bu ayarı mutlak bir yol ile (/var/lib/redis/) değiştirirseniz, Redis daima bu klasöre yazar.

dbfilename ayarındaki dosya bu dizinin içinde oluşturulur.

Aynı şekilde ek bir özelliğiniz varsa Append Only File denen dosya da bu dizine yazılır.

`Dikkat: Buraya bir klasör ismi vermelisiniz, dosya ismi değil.`