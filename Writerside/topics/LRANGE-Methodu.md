# LRANGE Methodu

* Bu method key'in sahip olduğu liste'nin elemanlarını, parametre olarak verilen başlangıç indexi ve bitiş indeksine
  göre bir array olarak döndürür.

> **SAKIN UNUTMA.!**
> 
> Redis'te listelerde ilk eleman 0. indeksten başlar. Son eleman da -1. indeksten başlar.
> İkinci eleman 1. index'tir. Sondan bir önceki eleman ise -2. indekstir.
>
{style="warning"}

> <b>Node.js Syntax</b>
> ````javascript
> const lRangeArr = await client.lRange('languages', 1, -1);
> console.log(`Elements of Redis List wit lRange Method ==> ${lRangeArr}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> lrange bookList 0 -1
> ````

* Bu method hata dönmez. yani range'ın dışında bir indis girildiği zaman hata alınmaz. Eğer methoda verilen
  başlangıç indeksi list'in size'ından büyükse boş bir array döner. Eğer stop parametresi daha büyükse redis son eleman gibi
  davranır.