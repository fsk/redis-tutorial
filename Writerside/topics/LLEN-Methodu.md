# LLEN Methodu

* Key'in sahip olduğu listenin uzunluğunu dönderir. Eğer key değeri mevcut değilse boş bir liste olarak yorumlanır ve 0
  değerini geriye döner. Eğer key değerinin içerisindeki value bir liste değilse de hata döner.
* O(1) zamanda çalışır.
> <b>Node.js Syntax</b>
> ````javascript
> const lengthFromRedisList = await client.lLen('languages');
> console.log(`LENGTH FROM REDIS LIST LANGUAGES ==> ${lengthFromRedisList}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> llen languages
> ````