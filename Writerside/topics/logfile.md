# logfile

redis.conf dosyasındaki tanımı

> logfile ""

Bu ayar, Redis loglarının nereye yazılacağını belirler.

* Eğer buraya bir dosya adı `(örneğin logfile "/var/log/redis.log")` yazarsanız, Redis loglarını o dosyaya kaydeder.
* Eğer logfile "" (boş) bırakırsanız, Redis logları standart çıktı dediğimiz `terminal ekranına gönderir.` Terminali açık 
tutarsanız logları görebilirsiniz.


> **DİKKAT.!**
>
> Eğer `daemonize yes` yapıp `logfile ""` bırakırsanız, loglar bir yere yazılmayacaktır, yani kaybolacaktır. 
> (çünkü arka planda çalışan bir süreç terminalden ayrılmıştır). Bu durumda logları kaybetmemek için bir dosya yolu 
> belirtmelisiniz veya farklı bir yöntemle logları yakalamalısınız.
>
{style="warning"}