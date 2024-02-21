# MGET, MSET Methodları

* MGET methodu aynı anda birden fazla key değerinin value'lerini bir <i>liste</i> olarak döndürür. Olmayan bir değer varsa
<i>nil</i> olarak döndürür.
* O(N)'de çalışır.
> <b>Node.js Syntax MGET Methodu</b>
> ````javascript
> await client.set("writer1", "Uncle Bob");
> await client.set("writer2", "Martin Fowler");
> await client.set("writer3", "Josh Long");
> const result1 = await client.mGet(["writer1", "writer2", "writer3"]);
> console.log(result1);
> ````
> <b>Redis Syntax MGET Methodu</b>
> ````SQL
> mget writer1 writer2 writer3
> ````
* MSET methodu aynı anda birden fazla key değeri set etmemizi sağlar.
* O(N)'de çalışır.
* Atomik bir işlemdir.
* Mevcut değerleri yeni değerleriyle değiştirir.
> <b>Node.js Syntax MSET Methodu</b>
> ````javascript
> const keyValueArray = ['writer1', 'Uncle Bob', 'writer2', 'Josh Long', 'writer3', 'Martin Fowler'];
> const result2 = await client.mSet(keyValueArray);
> console.log(`RESULT 2 ==> ${result2}`);
> ````
> <b>Redis Synrax MGET Methodu</b>
> ````SQL
> MSET key1 "Hello" key2 "World"
> ````

<b>NOT:</b> MSET komutu eğer redis'te bir key değeri varsa onun üzerine yeni value değerini yazar.
Eğer bu durum olsun istenmiyorsa <b><i>MSETNX</i></b> methodu kullanılabilir.