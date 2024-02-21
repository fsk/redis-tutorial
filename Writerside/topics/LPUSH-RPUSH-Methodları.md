# LPUSH, RPUSH Methodları

* LPUSH methodu veriyi listenin başına ekler. STACK yapısını taklit etmek için kullanılır.
* Listeye ilk başta bir eleman eklenirse geriye 1 döner. Eğer başka bir eleman eklenirse bu sefer 2 döner. Aynı anda 3
  eleman daha eklenirse 5 döner. Yani bu şekildeki her bir operasyondan sonra geriye listenin size'ını döner.
> <b>Node.js Syntax LPUSH Methodu</b>
> ````javascript
> const value1 = await client.lPush('books', 'Clean Code');
> console.log(`VALUE1 ==> ${value1}`);
> const value2 = await client.lPush('books', ['Redis Essentials', 'Refactoring']);
> console.log(`VALUE2 ==> ${value2}`);
> ````
> <b>Redis Syntax LPUSH Methodu</b>
> ````SQL
> lpush bookList "spring-boot-in-action"
> lpush bookList "docker-in-action"
> lpush bookList "kubernetes-in-action" "redis-essentials"
> ````

**`NOT:`** İlk önce <i>spring-boot-in-action</i> değerini ekler. Sonrasında listenin başına 
<i>docker-in-action</i> değerini ekler.
En sonunda liste
1) redis-essentials
2) kubernetes-in-action
3) docker-in-action
4) spring-boot-in-action
şeklinde olur.

* RPUSH methodu veriyi listenin sonuna ekler. QUEUE Yapısını taklit etmek için kullanılır.
* Listeye ilk başta bir eleman eklenirse geriye 1 döner. Eğer başka bir eleman eklenirse bu sefer 2 döner. Aynı anda 3
  eleman daha eklenirse 5 döner. Yani bu şekildeki her bir operasyondan sonra geriye listenin size'ını döner.

> <b>Node.js Syntaxı</b>
> ````javascript
> const addSingleValue = await client.rPush('languages', 'Javascript');
> console.log(`SINGLE VALUE ==> ${addSingleValue}`);
> const addMultipleValue = await client.rPush('languages', ['Java', 'Python', 'C']);
>console.log(`ADD MULTIPLE VALUE ==> ${addMultipleValue}`);
> ````
> <b>Redis Syntax RPUSH Methodu</b>
> ````SQL
> rpush bookList "spring-boot-in-action"
> rpush bookList "docker-in-action"
> rpush bookList "kubernetes-in-action" "redis-essentials"
> ````
<b>NOT:</b> RPUSH methodunda hangi sırayla veri geldiyse o şekilde eklenir.
Yani liste şu şekilde olur.
1) Javascript
2) Java
3) Python
4) C