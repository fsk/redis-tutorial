# LINDEX Methodu

* Key'in sahip olduğu listedeki verilen değerin listedeki karşılığını döner. Her programlama dilinde olduğu gibi
  redis'te de listeler 0.index'ten başlar.

> **SAKIN UNUTMA.!**
> 
> Redis'te 0. index ilk elemanı gösterirken -1. index son elemanı gösterir. Yani elimizde bir dizi
> olduğunu kabul edelim.
> [17, 13, 2, 5, 3, 7]
> Bu dizinin 0. indexi 17'dir. 1. indexi 13'tür.
> Ama aynı zamanda -1. indeksi 7'dir. -2. indexi 3'tür.
>
{style="warning"}


> <b>Node.js Syntax</b>
> ````javascript
> const lIndexZero = await client.lIndex('languages', 0);
> console.log(`0. index ==> ${lIndexZero}`);
>
> const lIndexPozitiveOne = await client.lIndex('languages', 1);
> console.log(`1. index ==> ${lIndexPozitiveOne}`);
>
> const lIndexNegativeOne = await client.lIndex('languages', -1);
> console.log(`-1. index ==> ${lIndexNegativeOne}`);
>
> const lIndexNegativeTwo = await client.lIndex('languages', -2);
> console.log(`-2. index ==> ${lIndexNegativeTwo}`);
> ````
> <b>Redis Syntax</b>
> ````SQL
> lindex languages -2
> lindex languages 0
> ````