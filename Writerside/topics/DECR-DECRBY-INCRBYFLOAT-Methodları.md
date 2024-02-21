# DECR, DECRBY, INCRBYFLOAT Methodları

* DECR ve DECRBY methodları O(1) zamanda çalışır.
* DECR methodu ile DECRBY methodu INCR ve INCRBY methodlarının tam tersi olarak çalışır.


>
> Başlıktaki komutlar atomiktir.İki farklı client aynı anda aynı komutu çalıştıramaz ve aynı sonucu alamaz.
> Bu komutlarla ilgili race conditionlar oluşmaz. Race Condition iki farklı kaynağın aynı anda eş zamanlı olarak erişmeye
> çalıştığında oluşan bir problemdir. Redis, aynı anahtara eş zamanlı erişim durumlarında, sırayla ve tutarlı bir
> şekilde komutları işler. Böylece, iki farklı client aynı anda aynı komutu çalıştıramaz ve aynı sonucu alamaz
>
{style="note"}
