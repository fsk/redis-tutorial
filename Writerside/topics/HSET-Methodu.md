# HSET Methodu

* Bir key değerine `Key-Value Pair` olacak şekilde veri ekler. Eğer key değeri varsa value'sinin üzerine yazar.
  Eğer key değeri yoksa yeniden oluşturur. Eğer değer başarılı bir şekilde eklenirse 1 döner. Eğer ilgili key değeri varsa
  (ki bu durumda key değeri sabit kalarak value değeri değişir) o zaman da 0 değeri döner.

> <b>Node.js Syntax</b>
> ````javascript
> const setHash = await client.hSet("movie", {title: "The Godfather"});
> console.log(`Create HSET ==>  ${setHash}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> hset movie "title" "The Godfather"
> ````