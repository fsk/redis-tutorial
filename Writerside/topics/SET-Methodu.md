# SET Methodu

Bu komut ile Redis'e key value set edilebilir.
> <b>NodeJS Syntax</b>
> ````javascript
> await client.set('first_key', 'Redis Essentials');
> ````
> <b>Redis Syntax</b>
> ````SQL
> set keyName "valueName"
>````

### SET METHODU FLAG'LERİ
* **`EX`**: Second cinsinden verinin rediste kalacağı süre

> <b>NodeJS Syntax</b>
> ````javascript
> await client.set('first_key', 'Redis Essentials', {EX: 5});
>````
> <b>Redis Syntax</b>
> ````SQL
> set springboot "Spring Boot in Action" EX 15
> ````

* **`PX:`** Milisecond cinsinden verinin rediste kalacağı süre.

* **`EXAT:`** Timestamp cinsinden verinin rediste kalacağı süre (saniye cinsinden). Yani bunu şu şekilde düşünebiliriz.
Bir veri rediste 300 sn kalacaksa, ve o anki epoch time 1650000000 ise biz bunu EXAT kullanıyorsak
1650000300 yapmalıyız.
* **`PXAT:`** Timestamp cinsinden verinin rediste kalacağı süre. (milisaniye cinsinden)
* **`NX:`** Key değeri yalnızca rediste halihazırda yoksa yeni bir değer tanımlamamıza olanak tanır.
Eğer aynı değerden varsa NİL döner.
> <b>NodeJS Syntax</b>
> ````javascript
> const result = await client.set('Redis', 'Redis Essentials', {NX: true});
> console.log(`RESULT => ${result}`);
> const value = await client.get('Redis');
> console.log(value);
> ````
> <b>Redis Syntax</b>
> ````SQL
> set springboot "Spring Boot in Action"
> get springboot
> set springboot "Spring Boot in Action" NX
> ````
* **`XX:`** Key değeri yalnızca rediste halihazırda varsa yeni bir değer tanımlamamıza olanak tanır.
> <b>NodeJS Syntax</b>
> ````javascript
> await client.set('springboot', 'Spring Boot in Practice');
> const result2 = await client.set('springboot', 'Spring Boot in Action', {XX: true});
> console.log(`RESULT2 => ${result2}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> set springboot "Spring Boot in Practice"
> get springboot
> set springboot "Spring Boot in Action" XX
> ````
> <b>Çok Önemli Not:</b> XX değeri aynı key değerinden varsa o key değerini yeniden ayarlar. Yoksa NİL döner.

* **`GET:`** Redis'te GET flag'ı SET komutuyla birlikte kullanıldığında, anahtarın eski değerini döndürürken aynı anda 
yeni bir değer atamanıza olanak tanır. Bu, "get-and-set" işlemi olarak bilinir ve atomik bir işlemdir, 
yani iki işlem (değer almak ve değer atamak) birbirinden ayrılmaz ve ardışık olarak gerçekleşir.
> <b>NodeJS Syntax</b>
> ````javascript
> await client.set('docker', 'Docker in Action');
> const value4 = await client.get('docker');
> console.log(`VALUE4 => ${value4}`);
> const result4 =  await client.set('docker', 'Docker in Practice', {GET: true});
> console.log(`RESULT4 => ${result4}`);
> console.log(await client.get('docker'));
> ````
> <b>Not:</b> İlk satırda "docker" key'ine "docker in action" değerini yazar. Sonrasındaki set methodundan sonra tekrar 
>docker in action değerini yazar ama en son satırda docker in practice yazar.
> 
> <b>Redis Syntax</b>
> ````SQL
> set docker "docker in action"
> get docker
> SET docker "docker in practice" GET
> get docker
> ````
> <b>Not:</b> 3. satırdaki komut geriye "docker in action" değerini döner ama bir aşağı satırdaki get docker
komutundan "Docker in practice" değerini getirir.
* <b><ins>`KEEPTTL:`</ins></b> Redis'te EX ile TTL verdiğimiz bir değeri güncellediğimizde bu flag'le birlikte
güncellediğimiz değerin de TTL değeri aynen korunur.
> <b>NodeJS Syntax</b>
> ````javascript
> await client.set('kubernetes', 'Kubernetes in Action', {EX: 15});
> console.log(await client.get('kubernetes'));
> await client.set('kubernetes', 'Kubernetes in Practice', {KEEPTTL: true});
> ````
> <b>Redis Syntax</b>
>`````SQL
> set kubernetes "kubernetes in action" EX 15
> get kubernetes
> set kubernetes "kubernetes in practice" KEEPTTL
>`````