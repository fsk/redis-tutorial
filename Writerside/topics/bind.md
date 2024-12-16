# bind

Redis, ağ üzerinden ulaşılabilen bir service'tir. Yani, başka bilgisayarlar veya programlar ona bağlanıp veri alıp 
verebilirler. Bu bağlantılar, sunucunun çalıştığı bilgisayardaki ağ kartları ve IP adresleri üzerinden gerçekleşir. 
`“bind”` terimi, bir sunucu uygulamasının hangi ağ arayüzüne veya IP adresine "bağlanacağını" belirten bir ayardır. 
Yani, hangi adrese gelip Redis’e ulaşılabileceğini söyler.

`Eğer redis.conf dosyasında hiç “bind” ayarı yapmazsanız`, Redis bulunduğu makinedeki tüm kullanılabilir ağ 
arayüzlerinden (tüm IP adreslerinden) bağlantı kabul eder. Örneğin bilgisayarınızın hem “192.168.1.10” hem de “10.0.0.5” 
gibi iki IP adresi varsa, Redis iki adresten de gelen istekleri dinlemeye başlar. Böylece ağa bağlı olan başka bir 
bilgisayar veya uygulama, bu IP adreslerinin herhangi birine bağlanarak Redis ile iletişime geçebilir.

## Örnekler

> bind 127.0.0.1 192.168.1.10

Yukarıdaki gibi şekilde bir bind tanımlarsak Redis aynı anda hem 127.0.0.1 hem de 192.168.1.10 IP adresleri üzerinden 
bağlantı kabul edecektir. 

> bind 127.0.0.1

Bu şekilde tanımlama yaparsak redis sadece localhosttan gelen istekleri kabul edecektir.

### - simgesi

bind satırında belirtilen adreslerden önce `“-”` karakteri koyarak Redis’e `“Eğer bu adres mevcut değilse de sorun etme, 
yine de başla”` diyebiliriz. Bu ne anlama gelir?

Bazı durumlarda belirli bir IP adresi her zaman mevcut olmayabilir (örneğin sunucu yapılandırmanız dinamik veya belirli 
durumlara göre değişiyor diyelim). Eğer normalde bu IP adresi makinede yoksa, Redis hata verip durmayacak, yoluna devam edecek.

Buna karşılık eğer adresin kendisi bir ağ arayüzünde mevcut olsa da o port zaten başka bir uygulama tarafından kullanılıyorsa, 
Redis `hata verecektir`. Çünkü aynı port numarasını aynı IP adresi üzerinde iki farklı uygulamanın kullanması mümkün değildir.


## redis.conf dosyasındaki tanımı

> bind 127.0.0.1 -::1

Buradaki syntax'ta aslında hem IpV4'ten gelen hem de Ipv6'dan gelenleri kabul et şeklinde çevrilebilir.

