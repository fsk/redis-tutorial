# SETBIT

* İlk argüman olarak bit numarasını ve ikinci argüman olarak bitin ayarlanacağı değeri alır; bu değer 1 veya 0'dır. 
Komut, adreslenen bit mevcut string uzunluğunun dışındaysa string'i otomatik olarak genişletir.

* Örneğin bir kullanıcının bir uygulamayı belirli bir günde kullanıp kullanmadğını temsil eder.

* Redis'teki SETBIT komutu, belirli bir key'de saklanan string değerindeki belirli bir offset değerindeki biti ayarlar 
veya temizler. Bitin değeri value parametresine bağlı olarak 0 veya 1 olarak ayarlanır. 
Eğer belirtilen key mevcut değilse, yeni bir string değeri oluşturulur ve bu string, offset'teki bir biti 
barındırabilecek şekilde genişletilir.

* Offset argümanının 0 veya daha büyük ve 2^32'den küçük olması gerekmektedir; bu da Bitmap'lerin en fazla 512MB 
olabileceğini belirler. Key'in altındaki string genişletildiğinde, eklenen bitler 0 olarak ayarlanır.

><b>Node.js Syntax</b>
>````javascript
> const date = new Date();
> const day = date.getDay();
> const month = date.getMonth() + 1;
> const year = date.getFullYear();
> const key = year + '-' + (month < 10 ? '0' + month : month) + '-' + (day < 10 ? '0' + day > : day);
>const setBitOperation = await client.SETBIT(key, 10, 1);
>console.log(`SETBIT operation ==> ${setBitOperation}`);
>````
><b>Redis Syntax</b>
>````SQL
>SETBIT key:2024-02-01 17 0
>````