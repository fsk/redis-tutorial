# PFCOUNT

* PFCOUNT komutu, bir veya birden fazla anahtarı argüman olarak kabul eder. Tek bir argüman belirtildiğinde, yaklaşık 
kardinaliteyi döndürür. Birden fazla anahtar belirtildiğinde, tüm benzersiz elemanların birleşiminin yaklaşık 
kardinalitesini döndürür.

* PFCOUNT komutu tek bir key ile çağrıldığında, belirtilen key'de saklanan HyperLogLog veri yapısının hesapladığı 
yaklaşık kardinaliteyi döndürür. Eğer belirtilen anahtar mevcut değilse, 0 değerini döndürür.

* PFCOUNT komutu birden fazla keyle çağrıldığında, geçirilen HyperLogLog'ların birleşiminin yaklaşık kardinalitesini 
döndürür. Bu, içsel olarak belirtilen anahtarlarda saklanan HyperLogLog'ları geçici bir HyperLogLog'da birleştirerek yapılır.