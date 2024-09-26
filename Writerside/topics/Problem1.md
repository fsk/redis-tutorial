# Problem1

<b>Örn:</b> Elimde bir redis instance'ı var. Bir tane program bu redis instance'ını okuyor ve okunan bu redis key'indeki
değerleri Queue'ya atıyor. Diyelimki bu key değerleri ise tarihe göre dinamik olarak okunuyor. Uygulama tekrar 
çalıştıktan sonra bu key değerlerini consistency gözeterek nasıl okurum.?

## Çözüm 1
### Consumer Offset Tracking Pattern
Bir queue veya stream'deki mesaj tüketme ilerlemesini yönetmek ve takip etmek için kullanılır. Bu pattern, mesajların 
doğru sırada işlendiğinden ve hiç bir mesajın atlanmadığından veya birden fazla kez işlenmediğinden emin olunmasını
sağlar. Özellikle consumer'ın bir queue veya stream'den mesaj işlediği ve hata toleransı ile tam olarak bir kez işleme 
yapılmasının gerektiği mesaj odaklı sistemlerde faydalıdır.

#### Nasıl Çalışır-1
* <b>Offsetlerin Saklanması:</b> Tüketilen her mesaj için, consumer'daki Redis'te başarıyla işlenmiş en yüksek offseti
veya mesaj kimliğini gösteren bir kayıt günceller. Bu kayıt genellikle tüketicinin benzersiz kimliğini ve offset değerini
içerir.

* <b>Offset Veri Yapısı:</b> Offset, Redis'te `Hash` ya da `SortedSet`gibi farklı veri yapıları kullanılarak saklanabilir.


-> `HSET consumer_offsets <tüketici_id> <offset>` -  Bir hash yapısı, burada anahtar tüketici kimliği ve değer ise son 
işlenen offset’tir. 

-> `ZADD consumer_offsets <offset> <tüketici_id>` - Bir SortedSet yapısı, burada skor offset, değer ise tüketici kimliğidir.

-> `Mesajların Okunması` - Bir tüketici başladığında, son işlenen offset bilgisini Redis’ten okur ve yalnızca yeni 
mesajları (saklanan offset’ten daha büyük offset’li mesajlar) kuyruk veya akıştan çeker.

-> `Hata Yönetimi` - Bir hata durumunda, tüketici yeniden başladığında, Redis'te saklanan son başarılı offset'ten 
itibaren işleme devam edebilir ve böylece daha önce işlenmiş mesajların yeniden işlenmesini önler.

## Çözüm 2
### Sorted Set ile Çözüm
Key'leri veya timestamp'leri bir SortedSet'te saklanabilir. Bu şekilde, hangi key'lerin okunduğunu takip edebilir ve 
işlenecek bir sonraki key grubunu verimli bir şekilde sorgulayabiliriz.

#### Nasıl Çalışır-2
* Redis'e eklenen her key için, aynı zamanda bir SortedSet'e (örneğin, keys_index) timestamp'i score olarak ekleriz.

>```
> ZADD keys_index <timestamp> <key>
> ```

Burada timestamp mesajın eklendiği zamanı, key ise ilgili mesaj key'ini temsil eder.

* redis_read işlemi başladığında, son okunan timestamp'ten bu yana eklenmiş tüm key'leri almak için 
`ZREVRANGEBYSCORE` komutunu kullanılabilir. Bu komut, belirtilen score aralığında, yani timestamp'ler arasında, 
eklenmiş olan keyleri almamızı sağlar.

>```
> ZREVRANGEBYSCORE keys_index +inf <last_read_timestamp>
> ```

Burada `+inf` sonsuzluğu, yani en son eklenen anahtarları ifade ederken, last_read_timestamp ise son okunan timestamp'i 
temsil eder. Bu komut, son okunan timestamp'ten sonra eklenmiş tüm key'leri döndürecektir.

## Çözüm 3
### Redis Queue Mekanizmasıyla Çözüm
Veri işleme sırası güvenilir bir kuyruk olarak Redis List kullanmak düşünülebilir.

#### Nasıl Çalışır-3
* List'e yeni key'ler eklemek için `LPUSH` methodu kullanılır.
* Key'ler işlenmek üzere `BRPOP` methoduyla list'ten çıkarılır.
* redis_read uygulaması kapanırsa key'ler listede kalacak ve hiç bir verinin kaybedilmemesi ya da yok olmaması sağlanacak.


