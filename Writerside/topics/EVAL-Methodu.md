# EVAL Methodu

Lua scriptlerini çalıştırmak için iki komut vardır: 

**_EVAL_** ve **_EVALSHA_**. 

_EVAL Syntax_ şu şekildedir:
```
EVAL script numkeys key [key ...] arg [arg ...]
```
Parametreler şu şekildedir:

1. **script:** Lua scriptinin kendisi, bir string olarak
2. **numkeys:** Scripte parametre olarak geçirilen Redis key'lerinin sayısı
3. **key:** Script içinde KEYS değişkeni aracılığıyla erişilebilecek key adı
4. **arg:** Script içinde ARGV değişkeni aracılığıyla erişilebilecek ek bir argüman

> **BİLGİ**
> 
> Redis'te yazılan LUA Scriptleri Redis'te gömülü Lua 5.1 interpreter'ı tarafından çalıştırılır.
> 
{style="note"}


```SQL
EVAL "return 'Hello, Scripting.!'" 0
```

```
--Output
Hello, Scripting.!
```

Bu yapıyı incelediğimizde,

* **_EVAL:_** Redis'in Lua scriptlerini çalıştırmasını sağlayan komuttur.

* **_"return 'Hello, scripting!'":_** Çalıştırılacak olan Lua scriptidir. Bu script, yalnızca bir string döndürür: 
'Hello, scripting!'.

* **_0:_** Bu komutun sonunda yer alan sayı, Lua scriptine geçirilecek anahtarların (KEYS) sayısını belirtir. Bu örnekte
script, herhangi bir Redis anahtarı kullanmadığı için 0 değeri verilmiştir.

````SQL
SET book:1 'Docker in Action'
SET book:2 'Redis Essentials'
SET book:3 'Effective Java'
-- Bu şekilde 3 tane kitabı key-value olarak SET ettik.

EVAL "return redis.call('GET', KEYS[1])" 1 book:1
-- bu komuttan sonra çıktı olarak Docker in Action görürüz.
````

> **SAKIN UNUTMA.!**
> 
> Yukarıdaki Lua scripti ile redis komutlarının çalıştırılması biraz kafa karıştırıcı olabilir.
> 
> ```SQL
> EVAL "return redis.call('GET', KEYS[1])" 1 book:1
> ```
> bu satıra ilk bakıldığı zaman biz call methodu içerisinde 2. parametre olarak `KEYS[1]` diyerek zaten Docker in Action
> çıktısını almaya çalıştık. Neden bir daha book:1 diye ikinci bir defa bunu istedik diye kendi kendinize sorabilirsiniz.
> 
> Öncelikle Lua'da array'ler genellikle 1. indisten başlar. Bu yüzden KEYS[1] diye ifade ettik. 
> Bu KEYS array'inde sadece bir tane eleman vardır. 
> Ayrıca bu array'de hangi key'in olduğunu Lua bilemez. 
> 
> Eğer biz aşağıdaki gibi bir command yazsaydık;
> 
> ```SQL
> EVAL "return redis.call('GET', KEYS[2])" 1 book:1
> ```
> Bu command hata verirdi çünkü KEYS arrayinde şu an 2 tane eleman yok.
> 
> Ama aşağıdaki kod sorunsuz bir şekilde çalışırdı.
> 
> ```SQL
> EVAL "return redis.call('GET', KEYS[1])" 1 book:3
> ```
> Bu kod da çıktı olarak ``Effective Java`` String'ini verirdi.
{style="warning"}