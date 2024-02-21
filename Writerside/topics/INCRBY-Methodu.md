# INCRBY Methodu

* O(1) Zamanda çalışır.
* Bir key değerini verilen sayı kadar arttırır ve yeni değeri geriye döner.
* Redis sayıları int olarak ya da başka bir sayı formatında saklamaz. Sayılar redis'e eklenirken String
  formatında saklanır. İşlem yapılacağı zaman tekrar int'e çevrilir.
* INCR methodu ile kullanılan bir key değeri yoksa bu key değerini önce rediste oluşturur.
  Sonra bu key'in value değerini 0 olarak ayarlar. Akabinde, bu value değerini 1 arttır ve geriye bu arttırılmış
  değeri döner. Yani olmayan bir key değeri ile INCR methodunu kullandığımızda ekranda 1 değerini görürüz.

> <b>NodeJS Syntax</b>
> ````javascript
> const incrByValue1  = await client.set("spring-boot-in-action", "300");
> console.log(`INCR_BY_VALUE==> ${incrByValue1}`);
> const incrByResult1 = await client.incrBy("spring-boot-in-action", "57");
> console.log(`INCR_BY_RESULT ==> ${incrByResult1}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> set number 2
> incrby number 7
> ````