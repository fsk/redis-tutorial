# redis.conf file

Redis konfigürasyonu, Redis sunucusunun davranışlarını yöneten çeşitli seçeneklerin ve parametrelerin ayarlanmasını içeren
bir konfigürasyonlar bütünüdür. onfigürasyon ayarları; ağ, bellek kullanımı, kalıcılık, güvenlik ve daha fazlası gibi 
yönleri etkiler. `Genellikle redis.conf olarak adlandırılan Redis konfigürasyon dosyası, bu ayarların tanımlandığı yerdir.`

Konfigürasyon dosyasını okumak için Redis'in ilk argüman olarak dosya yoluyla başlatılması gerektiğini unutmamak gerekir.
`./redis-server /path/to/redis.conf`
Burada bir veya daha fazla başka konfigürasyon dosyasını dahil edebiliriz. Bu, tüm Redis sunucularına giden standart bir 
şablonunuz varsa ve aynı zamanda sunucuya özel birkaç ayarı özelleştirmeniz gerekiyorsa kullanışlıdır. 

`"include"` seçeneğinin `yönetici veya Redis Sentinel'den "CONFIG REWRITE"` komutu ile yeniden yazılmayacağını unutmamak 
gerekir. Redis her zaman bir konfigürasyon yönergesi değeri olarak işlenen son satırı kullandığından, runtimeda yapılan 
konfigürasyon değişikliklerinin üzerine yazılmasını önlemek için include'ları bu dosyanın başına koymak daha iyidir.

Bunun yerine, konfigürasyon seçeneklerini geçersiz kılmak için include kullanmakla ilgileniyorsanız, include'ı son satır 
olarak kullanmak daha iyidir.

Dahil edilen yollar joker karakterler (wildcards) içerebilir. Joker karakterlerle eşleşen tüm dosyalar alfabetik sırayla 
dahil edilecektir. Bir include yolunun joker karakter içerdiğini ancak sunucu başlatıldığında hiçbir dosyanın 
eşleşmediğini unutmayın, include ifadesi göz ardı edilecek ve herhangi bir hata verilmeyecektir. Bu nedenle, 
boş dizinlerden joker karakterli dosyaları dahil etmek güvenlidir.

### Ben Anlamadım
Yukarıdaki cümleleri redis.conf dosyasından aldım. redis.conf'a redis'i kurduğunuz dosya yolundan ulaşabilirsiniz.
Burada anlatılmak istenen şeyler aslında redis.conf dosyasının kullanımıyla alakalı best practice'ler ve öneriler.

Bir redis'i başlatırken redis.conf dosyasını muhakkak parametre olarak vermemiz gerekir.

`**./redis-server /your/redisConfFilePath/redis.conf**`

> **`Soru:`** Neden peki redis-server başlatırken direkt olarak ./redis-server diye başlatıyoruz.?
> Çünkü redis.conf dosyası ile ilgili redis-server aynı dosya yolunda. O yüzden otomatik olarak görebiliyoruz.

Bazı durumlarda tek bir redis.conf dosyası yeterli olabilir. Fakat büyük bir sistem yönetiyorsak veya farklı sunucularımız 
varsa, her sunucunun farklı özellikleri olabilir. Buradaki best practice şu olacaktır.

* Tüm Redis sunucularında ortak olan configleri içeren bir ana konfigürasyon dosyamız olur.
* Her sunucuya özgü ayarları eklemek istersek de bu ana dosyanın içinde ek dosyalar `"include"` edilebilir. Böylece 
standart ayarlardan farklı olan kısımları ayrı bir dosyada tanımlayarak karmaşıklığı azaltabiliriz.

#### **_"include"_** Yapısı Nedir ve Nasıl Çalışır?
`include ifadesi`, redis.conf dosyasına başka bir dosyadan configleri eklememizi sağlar. Bu sayede bir ana şablonumuz olur 
(asıl redis.conf dosyası) ve bu şablonun sonuna veya başına başka dosyaları da ekleyerek yeni configler ekleyebiliriz.

Örneğin

> include /path/to/general_settings.conf
> 
> include /path/to/server_specific_settings.conf

#### include kullanırken dikkat edilmesi gerekenler
* Konfigürasyon dosyasında aynı ayarı birden fazla tanımlarsanız, Redis her zaman son satırda yazılan değeri esas alır. 
Bu, include dosyalarından faydalanırken önemli bir noktadır. Diyelim ki başta tanımladığınız bir ayarı, sonradan dahil 
edeceğiniz dosya ile override etmek istiyorsanız, include komutunu en sona koymalısınız. Böylece daha önce tanımlanmış 
ayarları yeni dosyayla değiştirebilirsiniz.
* Redis’in içinde çalışan bir yönetici ya da Redis Sentinel gibi aracılar bazen `CONFIG REWRITE` adı verilen bir komutla 
çalışan sunucunun konfigürasyonunu güncelleyebilir. Ancak include ile eklediğiniz dosyalar bu süreçte yeniden yazılmaz. 
Yani include edilen dosyalar Redis’in kendi kendine düzenlediği (rewrite) yapılandırmaların dışında kalır. Bu, 
manuel kontrolün sizde olması açısından önemli bir ayrıntıdır.
* Eğer include ile eklediğiniz dosyalar, ana ayarların `"üzerine yazmayacak"` şekilde kullanılacaksa, bunları konfigürasyon
dosyasının başına koymak daha iyidir. Böylece ileride CONFIG REWRITE gibi bir işlem yapıldığında bu ayarların etkilenmesi önlenir.
* Eğer include ile eklediğiniz dosyalar, mevcut ayarları geçersiz kılmayı amaçlıyorsa (yani sonradan gelen ayarlar öncekileri 
değiştirsin istiyorsanız), o zaman include komutunu redis.conf dosyasının en son satırlarında kullanmak daha uygun olur. 
Bu sayede önceki ayarlar en sonda gelen ayarlarca yenilenmiş olur.

#### wildcard
include ifadesiyle eklediğiniz dosya yolları joker karakterler içerebilir. Joker karakterler, belirli bir yolun içinde 
belirli bir dosya ya da dosya grubunu eşleştirmek için kullanılır. Örneğin:

> include /path/to/configs/*.conf

Buradaki *.conf ifadesi, /path/to/configs klasöründeki .conf uzantılı tüm dosyaları dahil eder. Redis bu dosyaları alfabetik sırayla okuyacaktır.

**`Soru:`** Eğer belirtilen joker karakterli bir yol hiç dosya ile eşleşmezse ne olur? <br/>
Bu durumda Redis bunu bir hata olarak görmez, sadece bu include satırını göz ardı eder ve sorunsuz çalışmaya devam eder.

## redis module
Redis'i direkt olarak yüklediğinizde aslında bir çok şeyi yapabiliyorsunuz fakat bütün temel özellikleri kullanamıyorsunuz.
redissearch ya da, redisjson gibi özellikleri kullanamıyorsunuz. Bunun için modüle tanımını yapmanız gerekmektedir.

> loadmodule /path/to/my_module.so
> 
> loadmodule /path/to/other_module.so

Yukarıdaki gibi modülleri redis.conf dosyanıza tanımlayarak redis'i gelişmiş seçenekleriyle kullanabilirsiniz.
Ama bence en temizi direkt redis yerine redis-stack kullanmak en mantıklısı çünkü normal redis'in özelliklerinin yanı sıra
gelişmiş redis seçenekleriyle de karşımıza çıkan bir yapı.



> **Ek Bir Bilgi**
>
> redis.conf dosyasında configler belirli başlıklar altında toplanmışlardır.
> `NETWORK`, `GENERAL`, `SNAPSHOTTING`, `REPLICATION` bunlardan bazılarıdır. Biz bu tutorial'da sadece belli başlı
> configlere bakacağız ve bu configleri alt başlıklar halinde ayırmayacağız.
>
{style="note"}



