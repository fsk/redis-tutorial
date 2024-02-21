# EVAL Methodu Örneği

Lua programlama dilini kullanarak bütün key value değerlerini dolaşıp alabiliriz. Bunun için aşağıdaki Lua kodunu redis
cli'ya vermemiz yeterli olacaktır.

> ```JAVA
> local keys = redis.call('KEYS', '*'); 
> local result = {}; 
> for i=1, #keys do 
>   local key = keys[i]; 
>   local value = redis.call('GET', key); 
>   table.insert(result, {key, value}); 
> end; 
> return result;
> ```
> 

1) Veritabanındaki bütün anahtarları alır.
2) Key-Value çiftlerini saklamak için boş bir tablo oluşturur.
3) Bütün key'ler üzerinde döngü kurar. (**Hatırlatma:** Lua'da # ifadesi uzunluk alır.)
4) `local value = redis.call('GET', key);` satırı her anahtarın değerini alır.
5) insert işlemi ile key-value çiftini sonuç tablosuna ekler.


Bu kodu redis-cli'da aşağıdaki gibi command olarak çalıştırmamız lazım.

> ````SQL
> EVAL "local keys = redis.call('KEYS', '*'); local result = {}; for i=1, #keys do local key = keys[i]; 
> local value = redis.call('GET', key); table.insert(result, {key, value}); end; return result;" 0
> ````
> 

Dikkat edilirse en sonunda 0 değerini verdik. Çünkü bu script'in çalışması için herhangi bir key argümanının verilmesine
gerek yok.