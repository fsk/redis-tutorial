# HGET, HMGET Methodları

* HGET methodu Redis key'inin içindeki key-value pairlerinden key'i verdiğimizde value'yi getirir.
* O(1)'de çalışır.
> <b>Node.js Syntax</b>
> ````javascript
> const hGetBooks = await client.hGet('books', 'book1');
> console.log(`HGET From Books ==>  ${hGetBooks}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> hget books book1
> ````
* HMGET Methodu bir hash'deki birden fazla value değerini getirir.
* O(N)'de çalışır.
> <b>Node.js Syntax</b>
> ````javascript
> const hmGetFieldArr = ['book1', 'writer1'];
> const hmGetBooks = await client.hmGet('books', hmGetFieldArr);
> console.log(`HMGET From Books ==>  ${hmGetBooks}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> hmget books writer1 book1
> ````