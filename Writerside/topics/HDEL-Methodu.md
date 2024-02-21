# HDEL Methodu

* Bir hash içerisindeki değeri siler.
* O(N)'de çalışır.
* Geriye Integer 1 döner. Eğer olmayan bir değer silinmeye çalışılırsa 0 döner.
> <b>Node.js Syntax</b>
> ````javascript
> const hDelFromPhone = await client.hDel('phones', 'phone1');
> console.log(`HDEL From Phones ==>  ${hDelFromPhone}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> hdel books writer1 book1
> ````