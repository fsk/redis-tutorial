# EVALSHA Methodu

EVALSHA komutu, Redis'in bir komutudur ve Lua scriptlerini çalıştırmak için kullanılır. Redis, veri depolama ve yönetimi 
işlemlerinde yüksek performanslı bir yapı sunmak için tasarlanmış bir in-memory veritabanıdır. Lua scriptleri, Redis 
üzerinde daha karmaşık işlemleri tek bir komutla gerçekleştirmek için kullanılabilir.

EVALSHA komutu, önceden yüklenmiş (Redis'e SCRIPT LOAD komutu ile yüklenmiş) bir Lua scriptini, scriptin SHA1 hash değeri 
kullanılarak çalıştırır. Bu yöntem, scripti her seferinde tekrar göndermek yerine, scriptin bir kez yüklenmesini ve 
daha sonra hash değeri ile hızlı bir şekilde tekrar kullanılmasını sağlar. Bu, ağ trafiğini azaltır ve script 
çalıştırma süresini kısaltır.


_Evalsha Syntax aşağıdaki gibidir._

```
EVALSHA sha1 numkeys key [key ...] arg [arg ...]
```

* **sha1:** Çalıştırılacak Lua scriptinin SHA1 hash değeri.

* **numkeys:** Scripte geçirilen key sayısı. Scriptin erişmesi gereken Redis keylerini belirtir.

* **key [key ...]:** Scripte geçirilen keyler. numkeys parametresiyle belirtilen sayıda anahtar burada listelenir.

* **arg [arg ...]:** Scripte geçirilen ek argümanlar. Betik içinde bu argümanlar kullanılabilir.


> **BİLGİ**
> 
> Eğer belirtilen SHA1 hash değeri ile bir script daha önce yüklenmemişse, Redis bir hata döner. Bu durumda, scripti
> `SCRIPT LOAD` komutu ile yeniden yüklemeniz veya EVAL komutunu kullanarak scripti doğrudan çalıştırmanız gerekebilir.
>
{style="note"}


> **BİLGİ**
>
> EVALSHA komutu, özellikle aynı scripti sık sık çalıştırmanız gerektiğinde çok yararlıdır, çünkü betiği her seferinde 
> yeniden göndermek yerine, scriptin hash değerini kullanarak çok daha hızlı bir şekilde çalıştırılabilir. Bu, özellikle 
> ağ gecikmesinin önemli olduğu durumlarda, performansı önemli ölçüde artırabilir.
>
{style="note"}