# SADD Methodu

* Bir veya daha fazla elemanı Set'e ekler.
* Eğer bir eleman ekleyeceksek O(1)'de çalışır. Eğer birden fazla eleman ekleyeceksek O(N)'de çalışır.
* Bir eleman eklenirse geriye integer 1 döner, N tane eleman ekleyeceksek geriye N döner.
* Eğer eklenen değer daha önceden Set içerisinde var ise geriye integer 0 döner.
> <b>Node.js Syntax</b>
> ````javascript
> const addToSet = await client.sAdd('soccers', 'Fernando Muslera');
> console.log(`Add To Set ==> ${addToSet}`);
>
> const addToSetMultipleVal = await client.sAdd('soccers', ['Wesley Sneijder', 'Didier Drogba']);
> console.log(`Add To Set Multiple Value ==> ${addToSetMultipleVal}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> sadd soccers 'Felipe Melo'
> sadd soccers 'Selcuk Inan' 'Torreira' 'Icardi'
> ````