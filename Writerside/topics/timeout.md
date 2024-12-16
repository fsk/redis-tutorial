# timeout

redis.conf dosyasındaki tanımı

> timeout 0

timeout 0 ayarı, client (yani Redis’e bağlanan program) hareketsiz (hiç komut göndermez, veri almaz) kaldığında ne kadar 
süre sonra bağlantının otomatik kesileceğini belirler.

`timeout 0 demek, zaman aşımı yoktur demek`. Yani eğer bir client bağlanıp hiçbir şey yapmadan beklerse bile Redis 
bağlantıyı kesmez.

Eğer buraya örneğin timeout 300 yazılsaydı, 300 sn (5 dk) boyunca client'tan hiçbir haber (komut) gelmezse bağlantıyı 
keserdi. Bu, kullanılmayan bağlantıların boşu boşuna açık kalmaması için iyi olabilir.

Bu ayar, sunucunun kullanılmayan bağlantılarla dolup kaynak tüketmesini engelleyebilir. Eğer bağlantıları açık tutmak 
isterseniz 0 değerini kullanabilirsiniz.