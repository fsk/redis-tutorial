# SCARD Methodu

* Key değeri verilen Set'in eleman sayısını geriye dönderir.
* O(1)'de çalışır.
> <b>Node.js Syntax</b>
> ````javascript
> const setMemberCount = await client.sCard('programminglanguages');
> console.log(`Member Count ==> ${setMemberCount}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> scard programminglanguages
> ````