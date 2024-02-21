# HYPERLOGLOGS

* HyperLogLogs Redis'te tam bir data type değildir.

* Kavramsal olarak, bir HyperLogLog, bir Set'te var olan benzersiz elemanların sayısının çok iyi bir yaklaşımını sağlamak 
için rastgelelik kullanan bir algoritmadır. Daha basit bir ifadeyle büyük veri setlerinde benzersiz elemanların sayısını 
(kardinalite) tahmin etmek için kullanılan bir algoritmadır.

* HyperLogLog algoritması olasılıksaldır. Yani 100% doğruluk sağlamayı garanti etmez.

* HyperLogLog algoritması orijinal olarak Philippe Flajolet, Éric Fusy, Olivier Gandouet ve Frédéric Meunier tarafından 
yazılan _**"HyperLogLog: Neredeyse optimal kardinalite tahmin algoritmasının analizi"**_ adlı makalede tanımlanmıştır. 
HyperLogLog'lar Redis 2.8.9'da tanıtılmıştır.

* Genellikle, benzersiz sayım yapmak için, sayımını yaptığınız kümedeki öğe sayısına orantılı bir hafıza miktarına 
ihtiyacınız vardır. HyperLogLog'lar bu tür problemleri harika performans, düşük hesaplama maliyeti ve az miktarda hafıza 
ile çözer. Ancak, HyperLogLog'ların %100 doğru olmadığını hatırlamak önemlidir. Yine de, bazı durumlarda, %99.19 
yeterince iyidir.


İşte HyperLogLog'ların kullanılabileceği birkaç örnek:

1. Bir web sitesini ziyaret eden benzersiz kullanıcı sayısını saymak
2. Belirli bir tarih veya saatte web sitenizde aranan farklı terimlerin sayısını saymak
3. Bir kullanıcı tarafından kullanılan farklı hashtag'lerin sayısını saymak
4. Bir kitapta görünen farklı kelimelerin sayısını saymak

* Benzersiz öğeleri saymak genellikle, saymak istediğiniz öğe sayısına orantılı bir hafıza miktarı gerektirir, çünkü çoklu 
sayımlardan kaçınmak için geçmişte zaten gördüğünüz elemanları hatırlamanız gerekir. Ancak, hafızayı doğrulukla takas 
eden bir algoritma seti mevcuttur: Bunlar, Redis'in HyperLogLog için uygulamasında %1'den az olan bir standart hatayla 
tahmini bir ölçüm döndürürler. Bu algoritmanın sihri, artık sayılan öğe sayısına orantılı bir hafıza miktarı 
kullanmanıza gerek kalmaması ve bunun yerine sabit bir hafıza miktarı kullanabilmenizdir.

* Redis'teki HyperLogLog'lar, teknik olarak farklı bir veri yapısı olsalar da, bir Redis string'i olarak kodlanır, 
böylece bir HyperLogLog'u serileştirmek için _GET_ ve onu sunucuya geri serileştirmek için _SET_ çağrılabilir.

* Kavramsal olarak HyperLogLog API'si, aynı görevi yapmak için Set'leri kullanmaya benzer. Gözlemlenen her elemanı bir 
sete _SADD_ ile ekler ve setin içindeki benzersiz elemanların sayısını kontrol etmek için _SCARD_ kullanırdık, 
_çünkü SADD mevcut bir elemanı yeniden eklemeyecektir._ Gerçekte bir HyperLogLog'a ögeler eklemeseniz de, çünkü veri 
yapısı gerçek elemanları içermeyen bir durum içerir, API aynıdır: Yeni bir eleman gördüğünüz her seferinde, onu _**PFADD**_ 
ile sayıma ekleriz. _PFADD_ komutu kullanılarak eklenen benzersiz elemanların güncel yaklaşımını almak istediğimizde 
_**PFCOUNT**_ komutunu kullanabilirsiniz. İki farklı HLL'yi birleştirmek gerekiyorsa, **_PFMERGE_** komutu mevcuttur. 
HLL'ler benzersiz elemanların yaklaşık sayılarını sağladığından, birleştirme sonucu her iki kaynak HLL'deki benzersiz 
elemanların sayısının bir yaklaşımını size verecektir.

* HLL -> HyperLogLog