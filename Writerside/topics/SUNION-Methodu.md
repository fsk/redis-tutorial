# SUNION Methodu

* Parametre olarak verilen Set'lerdeki verilerin tamamını birleştirir ve geriye bir array dönderir.
* Aynı değerlerden birden fazla olursa o değerleri tekler.
* O(N)'de çalışır.
> <b>Node.js Syntax</b>
> ````javascript
> const booksArray = ['technicalbooks', 'backendbooks', 'devopsbooks'];
> const sUnion = await client.sUnion(booksArray);
> sUnion.forEach(item => console.log(item));
> ````
> <b>Redis Syntax</b>
> ````SQL
> sunion technicalbooks backendbooks devopsbooks
> ````