# REDIS VE LUA

Redis, Redis 2.6 sürümünde, scripting özelliğini tanıttı ve Redis'i genişletmek için seçilen dil Lua oldu. 
Redis 2.6'dan önce, Redis'i genişletmenin sadece bir yolu vardı, kaynak kodunu değiştirmek, bu da C dilinde yazılmıştı. 
Lua, çok küçük ve basit olduğu için ve C API'si diğer kütüphanelerle entegre etmek için çok kolay olduğu için seçildi. 
Hafif olmasına rağmen, Lua çok güçlü bir dildir (sıklıkla oyun geliştirmede kullanılır).

Lua scriptleri atomik olarak çalıştırılır, bu da Redis sunucusunun betik yürütülürken engellendiği anlamına gelir. 
Bu yüzden, Redis'in herhangi bir scripti çalıştırmak için varsayılan olarak 5 saniyelik bir zaman aşımı vardır, 
ancak bu değer _**lua-time-limit**_ yapılandırması aracılığıyla değiştirilebilir. 

Redis, bir Lua scripti zaman aşımına uğradığında otomatik olarak scripti sonlandırmaz. Bunun yerine, bir scriptin 
çalıştığını belirten **_BUSY_** mesajı ile her komuta yanıt vermeye başlar. Sunucunun normal durumuna dönmesini 
sağlamanın tek yolu, **_SCRIPT KILL_** komutuyla veya **_SHUTDOWN NOSAVE_** ile script yürütmesini iptal etmektir. 
İdeal olarak, scriptler basit, tek bir sorumluluğa sahip olmalı ve hızlı çalışmalıdır.


* Bir Redis clientı, Lua scriptlerini Redis sunucusuna stringler olarak göndermelidir.

* Redis, geçerli herhangi bir Lua kodunu değerlendirebilir ve birkaç kütüphane mevcuttur (bitop, cjson, math ve string).

* Ayrıca, Redis komutlarını çalıştıran iki fonksiyon vardır: **_redis.call_** ve **_redis.pcall_**.

* _redis.call_ fonksiyonu komut adını ve tüm parametrelerini gerektirir ve çalıştırılan komutun sonucunu döndürür. 
Hatalar varsa, _redis.call_ betiği durdurur. _redis.pcall_ fonksiyonu *redis.call'*a benzerdir, ancak bir hata durumunda 
hatayı bir Lua table olarak döndürür ve scriptin yürütülmesine devam eder. Her script, return keywordü aracılığıyla bir 
değer döndürebilir ve açık bir return yoksa, _nil_ değeri döndürülür. Redis key isimlerini ve parametrelerini bir Lua 
scriptine geçirmek mümkündür ve bunlar Lua scriptine sırasıyla **_KEYS_** ve **_ARGV_** değişkenleri aracılığıyla 
kullanılabilir.

> **BİLGİ**
> 
> Hem _redis.call_ hem de _redis.pcall_, bir Redis komutunun sonucunu otomatik olarak bir Lua türüne dönüştürür, 
> Bu da Redis komutu bir tamsayı döndürürse, bu değerin bir Lua sayısına dönüştürüleceği anlamına gelir. 
> Bir string veya array döndüren komutlar için de aynı şey geçerlidir. Her script bir değer döndüreceğinden, 
> bu değer bir Lua türünden bir Redis türüne dönüştürülecektir.
>
{style="note"}