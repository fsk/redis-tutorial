# INCR Methodu

* O(1) Zamanda çalışır.
* Bir key değerini 1 arttırır. Arttırılmış değeri geriye döner. Eğer key değeri String'e dönüştürülemeyen
  ya da yanlış bir değer içeriyorsa hata döner.
* Redis sayıları int olarak ya da başka bir sayı formatında saklamaz. Sayılar redis'e eklenirken String
  formatında saklanır. İşlem yapılacağı zaman tekrar int'e çevrilir.
* INCR methodu ile kullanılan bir key değeri yoksa bu key değerini önce rediste oluşturur.
  Sonra bu key'in value değerini 0 olarak ayarlar. Akabinde, bu value değerini 1 arttır ve geriye bu arttırılmış
  değeri döner. Yani olmayan bir key değeri ile INCR methodunu kullandığımızda ekranda 1 değerini görürüz.
* INCR methodu yalnızca 64 bitlik işaretli tamsayılarla çalışır.

> <b>NodeJS Syntax</b>
> ````javascript
> const value1 = await client.set("redis-essentials", 292);
> console.log(`Value1 ==> ${value1}`);
> const value2 = await client.INCR("redis-essentials");
> console.log(`Value2 ==> ${value2}`);
> ````
> <b>Redis Syntax</b>
>````SQL
> SET counter "100"
> INCR counter
> ````