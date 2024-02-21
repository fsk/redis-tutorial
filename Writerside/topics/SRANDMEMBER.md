# SRANDMEMBER Methodu

* Bir Set'ten rastgele bir üye döndürür. Set'ler unordered(sırasız) olduğu için belirli bir pozisyondan elemanları almak
  mümkün değildir.
* Pozitive ne Negative değerler alabilir.
* Eğer count argümanı pozitif bir sayı olarak verilirse, Set içinden benzersiz elemanlar içeren bir dizi döndürülür.
  Bu dizi, ya belirtilen count sayısı kadar eleman içerir ya da Set'in toplam eleman sayısına (kardinalitesine) eşit olur;
  hangisi daha azsa o kadar eleman içerir.
* Eğer count argümanı negatif bir sayı olarak verilirse, komutun davranışı değişir ve aynı elemanın birden fazla kez
  döndürülmesine izin verilir. Bu durumda, döndürülen eleman sayısı, count argümanının mutlak değeri kadardır.
> <b>Node.js Syntax</b>
> ````javascript
> const programmingLanguagesArr = [
>     'C', 'C#', 'C++', 'Java', 'Javascript', 'TypeScript', 'Go', 'Ruby', 'PHP', 'Python'
> ]
>
> await client.sAdd('programminglanguages', programmingLanguagesArr);
>
> const randomMember = await client.sRandMember('programminglanguages');
> console.log(`Random Member ==> ${randomMember}`);
> //Not: Yukarıdaki kod her çalıştığında console'da farklı bir değer göreceğiz.
> ````
> <b>Redis Syntax</b>
> ````SQL
> srandmember programminglanguages
> ````