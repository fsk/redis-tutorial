# PFADD Methodu

* Redis'te PFADD komutu, HyperLogLog veri yapısına bir veya daha fazla eleman eklemek için kullanılır.
* HyperLogLog, benzersiz elemanların sayısını tahmin etmekte kullanılan bir algoritmadır ve PFADD komutu bu algoritmanın 
temel operasyonlarından biridir. Bu komut, verilen elemanları HyperLogLog'a ekler ve bu sayede veri yapısının benzersiz 
eleman sayısını tahmin etme kabiliyetini günceller.

* ``PFADD key element [element ...]``
* **_key:_** HyperLogLog veri yapısının adıdır. Eğer bu isimde bir veri yapısı yoksa, Redis otomatik olarak yeni bir 
HyperLogLog oluşturur.

* **_element:_** HyperLogLog'a eklenmek istenen eleman(lar). Birden fazla eleman eklemek için, elemanları aralarında 
boşluk bırakarak sıralayabiliriz.

* Eğer belirtilen key ile ilişkilendirilmiş bir HyperLogLog yoksa, Redis otomatik olarak yeni bir HyperLogLog oluşturur 
ve belirtilen eleman(lar)ı ekler.

* Eğer key zaten bir HyperLogLog'a işaret ediyorsa, Redis belirtilen eleman(lar)ı mevcut HyperLogLog'a ekler.

* _**PFADD**_ komutu, eklenen eleman(lar)ın HyperLogLog'un iç durumunu değiştirdiyse (yani en az bir eleman HyperLogLog'a 
daha önce eklenmemişse) 1 döner. Eğer eklenen tüm elemanlar zaten HyperLogLog'da varsa ve iç durum değişmezse 0 döner.

* PFADD komutunun sonucu, veri yapısının iç durumunun değişip değişmediğine bağlıdır, ancak bu sonuç HyperLogLog'un 
tahmini benzersiz eleman sayısının ne olduğu hakkında bir bilgi vermez

> <b>Node.js Syntax</b>
>````javascript
> const programmingLanguages = ["ElasticSearch", "Redis", "Docker", "Kubernetes"]
> const hyperLogLog = await client.PFADD('books', > programmingLanguages);
> console.log(`PFADD OPERATION ==> ${hyperLogLog}`);
>````
> <b>Redis Syntax</b>
> `````SQL
> PFADD visits:2015-01-01 "carl" "max" "hugo" "arthur"
>