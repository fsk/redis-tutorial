# GETBIT

* Belirtilen indeksteki bitin değerini döndürür.

* Aralık dışı bitler (hedef anahtar içinde saklanan string'in uzunluğunun dışında bir biti adresleme) her zaman sıfır 
olarak kabul edilir.

Örneğin belirli bir durumun kontrolünün sağlanması için kullanılabilir.

> <b>Node.js Syntax</b>
> ````javascript
>const getBitOperation = await client.GETBIT(key, 10);
>console.log(`GETBIT operation ==> ${getBitOperation}`);
> ````
> <b>Redis Syntax</b>
>````SQL
> GETBIT key:2024-02-01 17
>````