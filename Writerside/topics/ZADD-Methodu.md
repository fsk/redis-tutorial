# ZADD Methodu

* Bir sorted set'e eleman ekler.
* Belirtilen key'de saklanan sorted set'e, belirtilen score'larla tüm belirtilen üyeler eklenir.
* Birden fazla score/member çifti belirtmek mümkündür.
* Belirtilen bir üye zaten sorted set'in bir üyesiyse, score güncellenir ve eleman doğru sıralamayı sağlamak için doğru pozisyona tekrar yerleştirilir.
* Eğer key mevcut değilse, belirtilen üyelerle yeni bir sorted set oluşturulur, sanki sorted set boşmuş gibi. Eğer key mevcutsa ama bir sorted set içermiyorsa, bir hata döndürülür.
> <b>Node.js Syntax</b>
> ````javascript
>const cities = [
>    {
>        "score": 2020,
>        "value": "Sarajevo"
>    },
>    {
>        "score": 2020,
>        "value": "Canakkale"
>    }
>]
>
>const addToSortedSet = await client.zAdd('mySortedSet', cities);
> ````
> <b>Redis Syntax</b>
> ````SQL
> zadd mySortedSet 2024 "Ankara"
> ````

### ZADD METHODU FLAG'LERİ
* **`XX:`** Sadece zaten mevcut olan elemanların güncellenmesini sağlar. Yeni bir eleman eklemez.

> <b>Node.js Syntax</b>
> ````javascript
>const city = {
>    "score": 1923,
>    "value": "Ankara"
>}
>const zAddWithNXFlag = await client.zAdd('cities', city, {XX: true})
>console.log(`${zAddWithNXFlag}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> zadd cities XX 1905 "Ankara"
> ````

* **`NX:`** Sadece yeni bir eleman ekler. Var olan bir elemanı güncellemez.

> <b>Node.js Syntax</b>
>````javascript
>const newCity = {
>    "score": 1940,
>    "value": "Istanbul"
>}
>const zAddWithNXFlag = await client.zAdd('cities', newCity, {NX: true});
>console.log(`${zAddWithNXFlag}`);
>````
> <b>Redis Syntax</b>
> ````SQL
> zadd cities NX 1994 Hatay
> ````

* **`LT:`** Gelen yeni score eğer elemanın score'undan daha küçükse güncelleme yapar.
Yoksa güncelleme yapmaz. Yeni eleman eklenmesine engel değildir. Yani gelen değer, eğer ilgili sorted
set'te gelen eleman yoksa bu değeri ekler.
> <b>Node.js Syntax</b>
> ````javascript
> const update_LT_element = {
>     "score": 2000,
>     "value": "Sarajevo"
> }
>
> const zAddWithLTFlagUpdateElement = await client.zAdd('cities', update_LT_element, {LT: true});
> console.log(`LT FLAGI calistirildi. => ${zAddWithLTFlagUpdateElement}`);
>
> const add_LT_element = {
>     "score": 1994,
>     "value": "Hatay"
> }
>
> const zAddWithLTFlagAddElement = await client.zAdd('cities', add_LT_element, {LT: true});
> console.log(`LT FLAGI calistirildi. => ${zAddWithLTFlagAddElement}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> zadd cities LT 140 "Ankara"
> ````

* **`GT:`** Gelen yeni score eğer elemanın score'undan daha büyükse güncelleme yapar.
Yoksa güncelleme yapmaz. Yeni eleman eklenmesine engel değildir. Yani gelen değer, eğer ilgili sorted
set'te gelen eleman yoksa bu değeri ekler.
> <b>Node.js Syntax</b>
> ````javascript
> const update_GT_element = {
>     "score": 2050,
>     "value": "Ankara"
> }
>
> const zAddWithGtFlagUpdateElement = await client.zAdd('cities', update_GT_element, {GT: true});
> console.log(`GT FLAGI calistirildi. => ${zAddWithGtFlagUpdateElement}`);
>
> const add_GT_element = {
>     "score": 35,
>     "value": "Izmir"
> }
>
> const zAddWithGtFlagAddElement = await client.zAdd('cities', add_GT_element, {GT: true});
> console.log(`GT FLAGI calistirildi. => ${zAddWithGtFlagAddElement}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> zadd cities GT 140 "Ankara"
> ````

* **`CH:`** Redis'in ZADD komutunda CH (changed - değişen) bayrağı, komutun dönüş
değerinin davranışını değiştirir. Normalde ZADD komutunun dönüş değeri, sıralı sete eklenen yeni elemanların sayısını verir.
Ancak CH bayrağı eklenirse, dönüş değeri yalnızca yeni eklenen elemanların sayısını değil, aynı zamanda puanı
güncellenen mevcut elemanların sayısını da içerecek şekilde genişletilir.