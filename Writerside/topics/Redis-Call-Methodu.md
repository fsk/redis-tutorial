# REDIS.CALL ve REDIS.PCALL

## Redis.Call Methodu
Bu method commandName ve gerekli argümanları parametre olarak bekler. Ve geriye script çalıştırıldıktan sonra oluşan
sonucu döner. 
Eğer hata olursa scripti cancel eder.

## Redis.PCall Methodu
Bu method call methoduyla çok benzerdir ama bir error durumunda geriye Lua Table döner ve script çalışmaya devam eder. 

Her script, return anahtar kelimesi aracılığıyla bir değer döndürebilir ve açık bir return ifadesi yoksa, _nil_ değeri 
döndürülür.


> **BİLGİ**
> 
> Hem redis.call hem de redis.pcall, bir Redis komutunun sonucunu otomatik olarak bir Lua türüne dönüştürür. Bu, Redis 
> komutu bir tamsayı döndürdüğünde, bu tamsayının bir Lua sayısına dönüştürüleceği anlamına gelir. Bir string veya array 
> döndüren komutlar için de aynı şey geçerlidir. Her script bir değer döndüreceğinden, bu değer bir Lua türünden bir 
> Redis türüne dönüştürülecektir.
> 
{style="note"}