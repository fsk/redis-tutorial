# ZRANGE Methodu

* Belirli bir <key>'de saklanan sıralı setteki belirtilen aralıktaki elemanları döndürür.
* ZRANGE, farklı türde aralık sorguları gerçekleştirebilir: indekse (sıralama) göre, puana göre veya alfabetik sıraya göre.
* ZRANGE methodu normal defaultta score puanlarına göre sıralı getirir. Aynı puana sahip olan değerler
  alfabetik sıraya göre düzelir.
* Opsiyonel **_REV_** argümanı, sıralamayı tersine çevirir; böylece elemanlar en yüksek
  puandan en düşük puana doğru sıralanır ve aynı puana sahip elemanlar arasındaki bağlar ters alfabetik
  sıralama ile çözülür.
* Opsiyonel **_LIMIT_** argümanı, eşleşen elemanlardan bir alt aralık elde etmek için kullanılabilir (SQL'deki SELECT LIMIT offset, count'a benzer).
* Opsiyonel **_WITHSCORES_** argümanı, komutun yanıtına döndürülen elemanların puanlarını ekler.

### ZRANGE METHODU ARGUMANLARI KULLANIMI
* **`REV ARGUMANI:`**
> <b>Node.js Syntax</b>
> ````javascript
> // Asagidaki json'un Redis'te 'ProgrammingLanguages' adlı bir key'de tutulduğunu düşünelim.
> const programmingLanguages = [
>    {
>        "score": 200,
>        "value": "Javascript"
>    },
>    {
>        "score": 250,
>        "value": "Java"
>    },
>    {
>        "score": 100,
>        "value": "CSharp"
>    },
>    {
>        "score": 120,
>        "value": "PHP"
>    }
> ];
> const keyName = 'ProgrammingLanguages';
> const zRangeMethod = await client.zRange(keyName, 0, -1);
> zRangeMethod.forEach(item => console.log(`${item}`));
> ````
>
> <b>Redis Syntax</b>
> ````SQL
> range ProgrammingLanguages 0 -1
> ````

* **LIMIT ARGUMANI:** Eşleşen elemanlardan bir alt aralık elde etmek için kullanışlıdır. 
Özellikle büyük veri setleriyle çalışırken, belirli bir aralıktaki elemanları sorgularken kullanışlıdır.  
Örneğin, bir oyuncu lider tablosunu temsil eden bir sıralı setiniz olduğunu varsayalım ve bu lider tablosunda en iyi 10 oyuncuyu göstermek
istediğimizde bu durumda, ZRANGE komutunu LIMIT argümanı ile kullanılabilir.

> <b>Node.js Syntax</b>
> ````javascript
> /**
> NOT: 0 - x değerini görmek istersek ZRANGE key 0 9 withsocre dememiz yeterli.
> Withscore: Değerleri score'ları ile birlikte getirir.
> Ama mesela X - Y aralığını görmek istiyorsak bu sefer LIMIT argumanını > 
> kullanırız. LIMIT 2 tane deger alır. Birisi Offset, digeri Count. Offset degeri
> baslangic noktasını temsil eder. Count degeri ise baslangictan sonra kac tane  > eleman geleceğini temsil eder.
> */
>
> ````
> <b>Redis Syntax</b>
> ````SQL
> ZRANGEBYSCORE ProgrammingLanguages -inf +inf  withscores limit 10 3
> ````