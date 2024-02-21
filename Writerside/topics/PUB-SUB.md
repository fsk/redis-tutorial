# PUB-SUB

* Pub/Sub, mesajların doğrudan belirli alıcılara gönderilmediği bir modeldir.

* Publisher'lar, mesajları kanallara gönderir ve subscriberlar, belirli bir kanalı dinliyorlarsa bu mesajları alırlar.

* Bir subscriber, bir publischer'a abone olduktan sonra mesajları gerçek zamanlı olarak alır.

* Bir subscriber, bir publischer'dan mesajları, mesajların yayınlanma sırasıyla alır.
````
* Pub/Sub messaging için 4 temel husustan bahsedilebilir.
    -> Messaging
    Mesajlar göndericiden alıcıya gönderilen bir iletişim verisidir. Mesaj veri türleri basit String'lerden , sensör
    verilerinden, seslerden, dijital içeriklerden ... yani her şeyden oluşabilir.
    
    -> Topic
    Her mesajın kendisiyle ilişkili bir topic'i vardır. Topicler, göndericiler ile alıcılar arasında aracı kanal vazifesi
    görür. İlgili konu başlığı hakkındaki mesajlarla ilgilenen alıcıların güncel bir listesini tutar.
    
    -> Subscriber
    Subscriber mesajın alıcısıdır. Subscriber, ilgilendiği konu başlıklarına kaydolmak(yani abone olmak) zorundadır. Farklı
    işlevleri yerine getirebilirler veya paralel mesaja farklı bir şey yapabilirler.
    
    -> Publischer
    Publischer mesajı gönderen bileşendir. Bir topic hakkında mesajlar oluşturur ve sadece ilgili topic'in subscriber'larına
    gönderir. Publischer ile subscriber arasındaki bu etkileşim, bire-çok ilişki olarak adlandırılır. Publischer, yayınladığı
    bilgilerin kimlerin kullandığını, subscriber da mesajın nereden geldiğini bilmek zorunda değildir.
    
* Avantajları
    -> Loosely Coupling
    Publischer'lar Subscriber'lar ile loosely coupling bir bağa sahiptir ve onların varlığından haberdar olması bile gerekmez.
    -> Scability
    Pub/Sub, paralel işletim, mesaj caching, three-basef veya ağ tabanlı yönlendirme gibi yollarla, geleneksel 
    client-server yapısına göre daha iyi ölçeklenebilirlik fırsatı sunar. Ancak, tightly-coupled, yüksek hacimli 
    kurumsal ortamların belirli türlerinde, sistemler binlerce sunucunun pub/sub altyapısını paylaştığı veri merkezlerine 
    ölçeklendirildikçe, mevcut satıcı sistemleri sıklıkla bu avantajı kaybeder; bu bağlamlarda yüksek yük altındaki 
    pub/sub ürünlerinin ölçeklenebilirliği bir araştırma zorluğudur.
* Dezavantajları: Mesaj iletim problemleri
```` 


![img.png](img.png)


````
Bir subscribe bir publischer'a SUBSCRIBE methodu ile bağlanır.
-> SUBSCRIBE "channelName"
Bu method var olan bir kanala bağlanır.

Bir publischer ise, şu şekilde kanal oluşturup mesaj yayınlar.
-> PUBLISH "channelName" "messageName"
Bu method "channelName" ' e subscribe olan bütün kanallara "messageName" ' i gönderir.

NOT: Ama burada bazı duurmlar var. message olmadan Publisher oluşturulamıyor. Ayrıca bir subscriber, publisher'a
abone olduktan sonra, önceki mesajları okuyamıyor. 
````

* Bir pub/sub mekanizmasında subscriber modda iken aşağıdaki methodlar çalışır.

> <b>PING</b>
> Hiç bir argument sağlanmazsa <i>PONG</i> değerini döndürür. Aksi halde argümanların
> bir kopyasını kopya olarak döndürür.
> Bir bağlantının hala ayakta olup olmadığını test etmek için kullanılabilir.
> Gecikme ölçümleri için kullanılabilir.
> 
> ````javascript
> const result = await client.PING(`Ayaktayim`);
> console.log(`${result}`);
> /**
> * NOT: Eğer PING methodu içerisine hiç bir şey parametre olarak verilmezse
> * PONG değerini döndürür. 
> Eğer içerisine bir parametre verilirse onu dönderir.
> **/
> ````

> **BİLGİ**
> 
> <b>PSUBSCRIBE</b> bu komut, belirli bir pattern'e uyan kurallardan yayınlanan mesajları dinlemek için kullanılır.
> Örnek olarak
> * h?ello; hello, hallo hxllo ' ya abone olabilir.
> * h*llo; hllo, heeeeeello ' ya abone olabilir.
> * h[ae]llo; hello, hallo ya abone olabilir. Ama hillo'ya abone olamaz.
> 
{style="note"}