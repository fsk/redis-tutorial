# RTT

* Redis, client-server modelini kullanan ve Request/Response protokolü olarak adlandırılan bir TCP server'ıdır.
* Bu, genellikle bir isteğin aşağıdaki adımlarla tamamlandığı anlamına gelir:
Client, server'a bir sorgu gönderir ve genellikle engelleyici bir şekilde, server'ın yanıtını socket'ten okur.
komutu işler ve yanıtı client'a geri gönderir.
* Örneğin, dört komutluk bir dizi şöyle bir şeydir:

> ````
> Client: INCR X
> Server: 1
> Client: INCR X
> Server: 2
> Client: INCR X
> Server: 3
> Client: INCR X
> Server: 4
> ````

* Client'lar ve Server'lar bir ağ bağlantısı üzerinden bağlanır. Böyle bir bağlantı çok hızlı (bir loopback interface) 
veya çok yavaş (iki host arasında birçok hop olan İnternet üzerinden kurulan bir bağlantı) olabilir. 
Ağ gecikmesi ne olursa olsun, paketlerin client'tan server'a ve yanıtı taşımak için server'dan client'a geri dönmesi
zaman alır. Bu süreye RTT (Round Trip Time) denir. 

* Bir client'ın peş peşe birçok istekte bulunması gerektiğinde (örneğin, aynı listeye birçok eleman eklemek veya 
bir veritabanını birçok anahtarla doldurmak gibi) bunun performansı nasıl etkileyebileceği kolayca görülebilir. 
Örneğin, RTT süresi 250 milisaniye ise (İnternet üzerinden çok yavaş bir bağlantı durumunda), server saniyede 100k istek
işleyebilse bile, en fazla saniyede dört istek işleyebiliriz.

* Kullanılan arayüz bir loopback interface ise, RTT çok daha kısadır, tipik olarak milisaniyenin altındadır, 
ancak peş peşe birçok yazma işlemi yapmanız gerekiyorsa, bu bile çok fazla toplanacaktır.

* Bir Request/Response server, client eski response'ları henüz okumamış olsa bile yeni request'leri işleyebilecek 
şekilde uygulanabilir. Bu şekilde, response'ları hiç beklemeden server'a birden fazla komut göndermek ve sonunda 
response'ları tek bir adımda okumak mümkündür.

> **UNUTMA**
> 
> Client pipelining kullanarak komutlar gönderirken, server yanıtları sıraya koymak zorunda kalacak 
> ve bu da bellek kullanımına neden olacaktır. Bu nedenle, pipelining ile çok sayıda komut göndermeniz gerekiyorsa, 
> her biri makul bir sayıda komut içeren gruplar halinde göndermek daha iyidir, örneğin 10k komut, yanıtları okuyun ve 
> ardından başka bir 10k komut daha gönderin, ve böyle devam edin. Hız neredeyse aynı olacak, ancak ek bellek kullanımı 
> en fazla bu 10k komutun yanıtlarını sıraya koymak için gereken miktarda olacaktır.
> 
{style="warning"}

> **BİLGİ**
> 
> Pipelining, RTT ile ilişkili gecikme maliyetini azaltmanın sadece bir yolu değil, aslında belirli bir Redis server'ında
> saniyede gerçekleştirebileceğiniz işlem sayısını büyük ölçüde artırır. Bu, pipelining kullanılmadığında, her komutun 
> servis edilmesinin veri yapılarına erişme ve yanıt üretme açısından çok ucuz olduğu, ancak soket I/O'sunu yapma 
> açısından çok maliyetli olduğu için olur. Bu, read() ve write() syscall'larını çağırmayı, yani kullanıcı alanından 
> çekirdek alanına geçmeyi içerir. Bağlam değişimi büyük bir hız cezasıdır.
> Pipelining kullanıldığında, genellikle birçok komut tek bir read() sistem çağrısı ile okunur ve birden fazla yanıt 
> tek bir write() sistem çağrısı ile teslim edilir. Sonuç olarak, saniyede gerçekleştirilen toplam sorgu sayısı 
> başlangıçta daha uzun pipelinelarla neredeyse doğrusal olarak artar ve sonunda pipelining kullanılmadan elde edilen 
> temel çizginin 10 katına ulaşır, bu şekilde gösterilir.
>
{style="note"}