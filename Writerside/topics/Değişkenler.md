# Değişkenler

Lua programlama dilinde 3 tane değişken tipi vardır.

**1. Global Variables**<br />
Tüm değişkenler açıkça local olarak belirtilmedikçe global olarak kabul edilir.
```java
a,b = 40;
print(a);
print(b);
```

**Not:** a değişkeni için 40 değerini ekrana yazar, ama b değişkeni için _nil_ değerini yazar.

**2. Local Variables**<br />
Bir değişken için türü yerel olarak belirtildiğinde, kapsamı içinde bulundukları fonksiyonlarla sınırlıdır.

```java
local localSayi = 30;
print(localSayi);

local sayi1,sayi2 = 10, 20;
print(sayi1);
print(sayi2);
```

**Not:** Bir satırda birden fazla şekilde değişken tanımı yapılıp, bu değişkenlere yine aynı satırda değer atanabilir.

**3. Table Fields**<br />
Bu, nil dışında her şeyi tutabilen özel bir değişken türüdür, fonksiyonlar da dahil.

> **NOT**
>
> Lua'da, aynı Redis'teki gibi Null yoktur. Bunun yerine nil vardır.
>
{style="note"}