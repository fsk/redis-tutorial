# SISMEMBER Methodu

* Bir Set içerisinde bir değer var mı yok mu onu kontrol eder.
* Eğer değer varsa geriye integer 1 döner, eğer değer yoksa integer 0 döner.
* O(1)'de çalışır.
> <b>Node.js Syntax</b>
> ````javascript
> const isExistInSet = await client.sIsMember('programminglanguages', 'Go');
> console.log(`Is Exist in Set ==> ${isExistInSet}`);
> //Not: Bu kod node.js tarafında true/false döner.
> ````
> <b>Redis Syntax</b>
> ````SQL
> sismember programminglanguages 'Go'
> //NOT: Bu kod Intellijn db konsolunda çalıştığında true/false döner ama Redis-Cli da çalıştığında 0/1 döner.
> ````