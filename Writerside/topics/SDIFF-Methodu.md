# SDIFF Methodu

* Bir veya birden fazla Set'i parametre olarak bekler.
* Bu komut ilk Set'te bulunan, ancak takip eden Set'lerde bulunmayan tüm ögeleri içeren bir array döndürür.
* Bu komutta key'lerin sırası önemlidir. Var olmayan herhangi bir key, boş bir set olarak kabul edilir.
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
>          'Clean Architecture',
>          'Effective Java',
>          'Spring Boot in Practice',
>          'Redis Essentials'
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
> ````
> <b>Redis Syntax</b>
> ````SQL
> sdiff technicalbooks backendbooks
> ````