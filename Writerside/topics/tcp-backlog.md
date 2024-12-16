# tcp backlog

redis.conf dosyasındaki tanımı

> tcp-backlog 511

Redis bir sunucudur. Bu sunucu, ağ üzerinden gelen istekleri kabul ederek onlara yanıt verir. İstekler, `"TCP bağlantısı"` 
dediğimiz iletişim kanalları üzerinden gelir. Yani bilgisayarınızda çalışan Redis’e başka bilgisayarlar veya programlar 
bağlanmak istediğinde, bir `"istek kuyruğu"` oluşur. İşte `tcp-backlog, bu kuyrukla ilgilidir.`

tcp-backlog, basitçe ifade etmek gerekirse, `"Gelen bağlantı isteklerinin sıraya alınabileceği kuyruk uzunluğudur."`

* Düşünün ki Redis’e aynı anda birden fazla istemci (yani farklı bilgisayarlar veya programlar) bağlanmak istiyor. 
* Redis, her bağlantı isteği geldiğinde onu bir `bekleme sırasına (backlog) koyar. Eğer sunucu o an meşgulse 
(örneğin çok yoğun istek alıyor ve gelen isteklere anında bakamıyorsa), bu istekler geçici olarak bu kuyruğa alınır.
* tcp-backlog değeri, bu kuyruğun ne kadar uzun olabileceğini belirler. Örneğin tcp-backlog 511 demek, en fazla 511 
bağlantı isteğini sırada bekletebilirsiniz anlamına gelir.