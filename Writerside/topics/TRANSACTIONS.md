# TRANSACTIONS

* Redis'te transaction sırasıyla ve otomatik olarak yürütülen bir dizi komuttur.

* Bu komutları MULTI, EXEC, DISCARD ve WATCH komutları etrafında yürütebiliriz.

* Bir transactiondaki tüm komutlar sıralı bir şekilde seri hale getirilir ve ardışık olarak yürütülür. 
Başka bir client tarafından gönderilen bir request, Redis Transactionunun yürütülmesinin ortasında asla işlenmez.
Bu, komutların tek bir izole işlem olarak yürütüldüğünü garanti eder.

* EXEC komutu, transactiondaki tüm komutların yürütülmesini tetikler, bu nedenle bir client, EXEC komutunu çağırmadan önce 
transaction bağlamında sunucuya olan bağlantısını kaybederse, hiçbir operasyon gerçekleştirilmez. Ancak EXEC komutu
çağrılırsa, tüm transactionlar gerçekleştirilir. Sadece ekleme modunda dosya kullanırken, Redis transactionu
diske yazmak için tek bir write(2) sistem çağrısını kullanmaya dikkat eder. Ancak, Redis sunucusu çökerse veya
sistem yöneticisi tarafından bazı sert yöntemlerle kapatılırsa, yalnızca kısmi sayıda operasyonun kaydedildiği mümkündür.
Redis bu durumu yeniden başlatıldığında algılayacak ve bir hata ile çıkış yapacaktır. Redis-check-aof aracını kullanarak,
yalnızca ekleme modundaki dosyayı düzeltebilir ve kısmi transactionu kaldırabilir, böylece sunucu tekrar başlayabilir.

> **TRANSACTION KULLANIMI**
>
> MULTI komutu kullanılarak bir Redis Trransaction'u girilir. Komut her zaman OK ile yanıt verir.
> Bu noktada kullanıcı birden fazla komut verebilir. Redis bu komutları yürütmek yerine bunları sıraya koyacaktır.
> EXEC çağrıldığında tüm komutlar yürütülür.
> Bunun yerine DISCARD çağrılırsa transaction kuyruğu temizlenir ve transactiondan çıkılır.
>
> ````Python
> MULTI
> INCR foo
> INCR bar
> EXEC
> ````
> Yukarıdaki işlemler **Redis** syntaxı ile bir transaction kuyruğunun oluşturulması ve çalıştırılması adımlarıdır.
> 1) Önce <i>MULTI</i> komutu ile transaction başlatılır.
> 2) Sonra bir tane komut eklenir. (NOT: Girilen her command QUEUED dizisiyle yanıt verir.)
> 3) Sonra bir tane daha komut eklenir.
> 4) En sonunda <i>EXEC</i> komutuyla transaction çalıştırılır.
> 
> 
> **Node.js** syntaxı ile ise aşağıdaki şekildeki gibi bir transaction çalıştırılabilir.
> 
> ````Javascript
>  const result = await client
>    .multi()
>    .hSet("movie", {title: "The Godfather", star: 5})
>    .hSet("movie", {title: "Shawshank Redemption", star: 5})
>    .mSet("java community", "Turkiye Java Community", "AWS Community", "ServerlessTR")
>    .incr("count")
>    .exec();
> ````
> 
> 
{style="tip"}



