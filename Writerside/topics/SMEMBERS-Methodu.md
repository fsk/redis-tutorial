# SMEMBERS Methodu

* Parametre olarak verilen Set'teki elemanları geriye dönderir.
* Eğer SINTER Methodu'da tek parametre ile çalıştırılırsa aynı etkiyi yapar.
> <b>Node.js Syntax</b>
> ````javascript
>  const allMembers = await client.sMembers('programminglanguages');
>  allMembers.forEach(item => {
>        console.log(item);
>  });
> ````
> <b>Redis Syntax</b>
> ````SQL
> smembers programminglanguages
> ````