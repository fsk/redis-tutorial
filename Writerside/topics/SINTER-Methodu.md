# SINTER Methodu

* Bir veya daha fazla Set'i parametre olarak bekler.
* Eğer bir Set parametre verilirse, ilgili Set'in içerisindeki elemanlar array olarak döner.
* Eğer birden fazla Set parametre olarak verilirse, bu Set'lerdeki elemanlardan ortak olanlar geriye döner.
> <b>Node.js Syntax</b>
> ````javascript
> await client.sAdd('technicalbooks',
>     [
>         'Clean Code',
>         'Clean Architecture',
>         'Redis Essentials',
>         'ElasticSearch in Action',
>         'Effective Java'
>     ]
> );
>
> await client.sAdd('backendbooks',
>     [
>         'Clean Architecture',
>         'Effective Java',
>         'Spring Boot in Practice',
>         'Redis Essentials'
>     ]
> );
>
> await client.sAdd('devopsbooks',
>     [
>          'Redis Essentials',
>          'Clean Architecture',
>          'ElasticSearch in Action'
>     ]
> );
>
> const setArray = ['technicalbooks', 'backendbooks', 'devopsbooks']
> const allSetsMemberBooks = await client.sInter(setArray);
>
> console.log(`Set Intersection ==> ${allSetsMemberBooks}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> sadd technicalbooks 'Clean Code' 'Clean Architecture' 'Redis Essentials' 'ElasticSearch in Action' 'Effective Java'
> sadd backendbooks 'Clean Architecture' 'Effective Java' 'Spring Boot in Practice' 'Redis Essentials'
> sadd devopsbooks 'Redis Essentials' 'Clean Architecture' 'ElasticSearch in Action' 'Redis Essentials'
> 
> sinter technicalbooks
> 
> sinter technicalbooks backendbooks devopsbooks
> ````