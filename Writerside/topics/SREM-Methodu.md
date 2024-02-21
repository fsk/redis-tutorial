# SREM Methodu

* Set'ten değer siler ve değeri geriye döner.
* Eğer key değeri yoksa empty set gibi davranır ve geriye 0 döner.
* Set'in üyesi olmayan bir değer verildiğinde bunu gözardı (ignore) eder.
* Bir veya daha fazla eleman aynı anda silinebilir.
* Bir eleman silinecekse O(1)'de çalışılır. N eleman çıkarılacaksa O(N)'de çalışır.
> <b>Node.js Syntax</b>
> ````javascript
> const deleteFromSet = await client.sRem('programminglanguages', 'C#');
> console.log(`delete set ==> ${deleteFromSet}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> srem programminglanguages 'Python'
> ````
> <b>Not:</b> Bu kodlar intellij'de çalıştırıldığında geriye integer 1 döner ama redis-cli'da çalıştırıldığı zaman
> geriye ilgili değerin kendisi döner.