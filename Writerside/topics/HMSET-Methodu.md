# HMSET Methodu

* Normalde bu method ile bir map'e aynı anda birden fazla key value değeri eklemesi yapolabilirdi.
* Fakat bu method Redis 4.0'dan beri depreceted'dir. Bunun yerine yine HSET komutu kullanarak da aynı işlem yapılabilir.
> <b>Node.js Syntax</b>
> ````javascript
> const setHashBook = await client.hSet("books",
>    {
>        book1: 'Clean Code', writer1: 'Robert C. Martin',
>        book2: 'Refactoring', writer2: 'Martin Fowler',
>    }
> );
> ````
> <b>Redis Syntax</b>
> ````SQL
> HSET books book3 'GRPC Go Microservice' writer3 'Huseyin Babal' book4 'Redis Essentials' writer4 'Hugo Lopes'
> ````