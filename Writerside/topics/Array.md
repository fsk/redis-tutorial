# Array
* Arrayler, nesnelerin sıralı bir düzenidir; bu, bir dizi satır içeren tek boyutlu bir dizi ya da çoklu satır ve sütun 
içeren çok boyutlu bir dizi olabilir.

> **BİLGİ**
> 
> Lua'da, arrayler _tam sayılarla indekslenen tablolar_ kullanılarak uygulanır. Bir dizinin boyutu sabit değildir ve
> hafıza sınırlamalarına tabi olarak ihtiyaçlarımıza göre büyüyebilir. 
>
{style="note"}

* Lua programlama dilinde bir array aşağıdaki şekilde tanımlanabilir.

```java
local arr = {"Furkan", "Sahin", "Kulaksiz"};
```

* Tanımladığımız array'i bir döngü yardımıyla dönüp çıktıyı kontrol ettiğimizde ise;

> ````Java
> local arr = {"Furkan", "Sahin", "Kulaksiz"};
>
> for i=0,2 do
>   print(arr[i])
> end
> ````
> Bu kodun çıktısı aşağıdaki gibi olacaktır.
> ````
> nil
> Furkan
> Sahin
> ````

> **BİLGİ**
> 
> Array'de olmayan bir indeksteki bir elemana erişmeye çalıştığımızda, nil döndürür. Lua'da indeksleme genellikle 
> 1 indeksinden başlar. Ancak, 0 ve 0'ın altındaki indekslerde de nesneler oluşturmak mümkündür. 
> 
{style="note"}

* Negatif indeksli bir dizi ise aşağıdaki şekilde oluşturulabilir.

> ````Java
> array = {}
> 
> for i= -2, 2 do
>   array[i] = i *2
> end
> 
> for i = -2,2 do
>   print(array[i])
> end
> ````
>