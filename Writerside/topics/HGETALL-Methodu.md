# HGETALL Methodu

* Hash'deki bütün verileri getirir.
* O(N)' de çalışır.
> <b>Node.js Syntax</b>
> ````javascript
> const hGetAllFromBooks = await client.hGetAll('books');
> console.log(JSON.stringify(hGetAllFromBooks, null, 2));
> ````
> <b>Redis Syntax</b>
> ````SQL
> hgetall books
> ````
* HGETALL komutu, bir Hash çok sayıda alan içeriyorsa ve çok fazla bellek kullanıyorsa bir problem olabilir.
  Tüm bu verilerin ağ üzerinden aktarılması gerektiği için Redis'i yavaşlatabilir. Böyle bir senaryoda iyi bir alternatif
  `HSCAN` komutudur. HSCAN, tüm alanları bir seferde döndürmez. Bir cursor ve Hash alanlarını değerleriyle birlikte
  parçalar halinde döndürür. Bir Hash'teki tüm alanları almak için döndürülen cursor 0 olana kadar HSCAN komutunun tekrar
  tekrar çalıştırılması gerekir.