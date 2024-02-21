# RPOP, LPOP Methodları

* RPOP methodu listenin en sonundaki elemanı siler ve geriye döner. Fakat optional olarak bir sayı verilirse de o kadar
  elemanı çıkarır ve geriye döner.
* Aynı şekilde LPOP methodu da listenin en başındaki elemanı siler ve geriye döner.
> <b>Node.js Syntax</b>
> ````javascript
> const rPop = await client.rPop('languages');
> console.log(`rPop Method ==> ${rPop}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> rpop bookList
> ````