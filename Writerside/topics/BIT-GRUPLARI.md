# BIT GRUPLARI

Bit grupları üzerinde işlem yapan üç komut vardır:

* **_BITOP_**, farklı string'ler arasında bit düzeyinde işlemler gerçekleştirir. 
Sağlanan işlemler _AND_, _OR_, _XOR_ ve _NOT_'tur.


* **_BITCOUNT_**, 1 ile işaretlenmiş sayıları döner. Örneğin belirli bir durum ile kaç kez karşılaşıldığını anlamak 
için kullanılır.

> **BİLGİ**
> 
> Bir String key'in de Bitcount değeri vardır. Bu yüzden de range verilebilir.
> 
{style="note"}


> <b>Node.js Syntax</b>
> ````javascript
> const bitCountOperation = await client.BITCOUNT(key);
> console.log(`BITCOUNT operation ==> ${bitCountOperation}`);
> ````
> <b>Redis Syntax</b>
>````SQL
> BITCOUNT mykey
>````

* **_BITPOS_**, belirtilen 0 veya 1 değerine sahip ilk biti bulur.

Hem BITPOS hem de BITCOUNT, string'in tüm uzunluğu için çalışmak yerine, string'in bayt aralıkları ile işlem yapabilir. 
Bir bitmap'de ayarlanmış bitlerin sayısını basitçe görebiliriz.