# HINCRBY , HINCRBYFLOAT , INCRBY , INCRBYFLOAT Methodları

* HINCRBY methodu bir Hset içerisindeki bir field'ın değerini verilen parametre kadar arttırır.
> <b>Node.js Syntax</b>
> ````javascript
> const prices = await client.hSet("phones",
>     {
>         phone1: "iphone15", price1: 150,
>         phone2: "samsung", price2: 130
>     }
> );
>
> console.log(`Create Prices ==>  ${prices}`);
>
> const hIncrByPrice = await client.hIncrBy('phones', 'price1', 17);
> console.log(`HINCRBY price iphone ==>  ${hIncrByPrice}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> HSET notlar "matematik" 90 "Turkce" 70
> hincrby notlar "matematik" 10
> ````

* INCRBY methodu keyde saklanan sayıyı increment kadar artırır. Eğer key mevcut değilse, işlemi gerçekleştirmeden önce
  0 olarak ayarlanır. Key yanlış bir tipin değerini içeriyorsa veya bir tamsayı olarak temsil edilemeyen bir string
  içeriyorsa bir hata döndürülür.
* Bu işlem 64 bit işaretli tamsayılarla sınırlıdır.
* HINCRBYFLOAT ve INCRBYFLOAT methodları da aslında bu methodları tam sayı değilde noktalı sayılar cinsinden yapılmasını
  sağlar.