# Veri Tipleri


Lua, dinamik olarak tip belirlenen bir dildir, bu yüzden değişkenlerin türü yoktur, sadece değerlerin türü vardır.
Değerler değişkenlerde saklanabilir, parametre olarak geçirilebilir ve sonuç olarak döndürülebilir.

Lua'da değişkenler için veri türümüz olmasa da, değerler için türlerimiz vardır.

**1. nil**<br />
Değeri, bazı verilere sahip olanlardan veya hiçbir veriye sahip olmayanlardan (nil) ayırt etmek için kullanılır.

**2. boolean**<br />
Değer olarak true ve false içerir. Genellikle durum kontrolü için kullanılır.

**3. number**<br />
Gerçek (double precision floating point) sayıları temsil eder.

**4. string**<br />
Karakter dizilerini temsil eder.

**5. function**<br />
C veya Lua dilinde yazılmış bir methodu temsil eder.

**6. userdata**<br />
Rastgele C verilerini temsil eder.

**7. thread**<br />
Bağımsız yürütme thread'leri temsil eder ve coroutineleri uygulamak için kullanılır.

**8. table**<br />
Sıradan arrayleri, symbol tables'ları, set'leri, recordları, graph'ları, tree'leri vb. temsil eder ve ilişkisel dizileri uygular.
Herhangi bir değeri tutabilir (nil hariç).

> **Type Function**
>
> Lua'da değişkenin tipini bilmemizi sağlayan 'type' adında bir fonksiyon vardır. Aşağıda kullanımı gösterilmiştir.
>
{style="note"}

```Java
print(type(nil));
print(type(true));
print(type(print));
```