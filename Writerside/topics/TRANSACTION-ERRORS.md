# TRANSACTION Errors

* Bir transaction yürütülürken 2 farklı Command hatası ile karşılaşmak mümkündür.

* Bir komut sıraya alınmada başarısız olabilir, bu nedenle EXEC çağrılırken bir hata olabilir. 
Örneğin, komut sözdizimsel olarak yanlış olabilir (yanlış argüman sayısı, yanlış komut adı, vb.), veya maxmemory 
yönergesi kullanılarak sunucunun bir bellek limitine sahip olacak şekilde yapılandırılmışsa, bellek yetersizliği 
gibi kritik bir durum olabilir.

* EXEC çağrıldıktan sonra bir komut başarısız olabilir, örneğin bir key'e yanlış bir değerle 
(bir String değerine karşı bir liste işlemi çağırma gibi) bir işlem gerçekleştirdiğimiz için.

* Redis 2.6.5'ten itibaren sunucu, komutların toplanması sırasında bir hata algılayacaktır. 
Daha sonra EXEC sırasında hata veren operasyonu yürütmeyi reddederek transactionu iptal eder
.
* EXEC'den sonra meydana gelen hatalar özel bir şekilde ele alınmaz: transaction sırasında bazı komutlar başarısız olsa 
bile diğer tüm komutlar yürütülür.

* Bir komut başarısız olsa bile kuyruktaki diğer tüm komutların işlendiğini unutmamak önemlidir; Redis, komutların 
işlenmesini durdurmaz.