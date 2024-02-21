# Function

Bir function, birlikte belirli bir görevi gerçekleştiren ifadeler grubudur. Kodunuzu farklı fonksiyonlar arasında
bölebilirsiniz. Kodunuzu farklı fonksiyonlar arasında nasıl böleceğiniz size bağlıdır, ancak mantıksal olarak bölümleme
genellikle benzersizdir, böylece her fonksiyon belirli bir görevi yerine getirir.

Lua dili, programınızın çağırabileceği birçok yerleşik yöntem sağlar.
Örneğin, giriş olarak geçirilen argümanı konsolda yazdırmak için print() yöntemi.

Bir functiın, method veya sub-routin veya procedure vb. gibi çeşitli isimlerle bilinir.

Bir Function aşağıdaki gibi tanımlanır.

```
optional_function_scope function function_name( argument1, argument2, argument3........, arguments)
function_body
return result_params_comma_separated
end
```

**optional function scope:** Fonksiyonun kapsamını sınırlamak için local anahtar kelimesini kullanabilir veya kapsam
bölümünü yok sayabilirsiniz, bu da onu global bir fonksiyon yapacaktır.

**function name:** Bu, fonksiyonun gerçek adıdır. Fonksiyon adı ve parametre listesi birlikte fonksiyon imzasını oluşturur.

**arguments:** Bir argüman, bir yer tutucu gibidir. Bir fonksiyon çağırıldığında, bir değeri argümana geçirirsiniz.
Bu değere gerçek parametre veya argüman denir. Parametre listesi, bir metodun argümanlarının türüne, sırasına ve
sayısına atıfta bulunur. Argümanlar isteğe bağlıdır; yani, bir metod hiç argüman içermeyebilir.

**function body:** Methot gövdesi, methodun ne yaptığını tanımlayan ifadeler koleksiyonunu içerir.

**return:**  Lua'da, return anahtar kelimesini takip eden virgülle ayrılmış dönüş değerleriyle birden fazla değer
döndürmek mümkündür.

**Fonksiyon Örneği**

```SQL
print('Lutfen birinci sayi giriniz')
local inputNumber1 = io.read();
local number1 = tonumber(inputNumber1);

print('Lutfen ikinci sayiyi giriniz')
local inputNumber2 = io.read();
local number2 = tonumber(inputNumber2);

function max(n1, n2)
    local result;
    if n1 > n2 then
        result = n1;
    else 
        result = n2;
    end
    return result;
end

print('Buyuk sayi ==> ', max(number1, number2));
```


* Lua'da bir fonksiyonu değişkene atayabiliyoruz.
  **Örnek**
```SQL
myprint = function(param)
   print("This is my print function -   ##",param,"##")
end

function add(num1,num2,functionPrint)
   result = num1 + num2
   functionPrint(result)
end

myprint(10)
add(2,5,myprint)
```

* Bir fonksiyona, sayısı belli olmayan parametre geçilebilir.
  **Örnek**
```SQL
function average(...)
    local result = 0;
    local arg = { ... }
    for i,v in ipairs(arg) do
        result = result + v;
    end
    return result / #arg
end

print("The average is",average(10,5,3,4,5,6))
```
* ilk satırda <i>average(...)</i> şeklinde bir method tanımı yaptık. Burada 3 nokta koymamızın sebebi, gelen parametre
  sayısının belli olmamasından kaynaklıdır.
* Bu ifade, fonksiyona verilen değişken sayıdaki argümanları bir Lua tablosuna (diziye benzer bir yapı) dönüştürür. ... (üç nokta),
  Lua'da "vararg" olarak adlandırılan, fonksiyona verilen değişken sayıda argümanı temsil eder.
  local arg = { ... } ifadesi, bu argümanları bir tabloya alıp arg isimli bir yerel değişkene atar.
* Bu döngü, arg tablosundaki her bir elemanı sırayla ziyaret eder ve bu elemanların indeksini (i) ve değerini (v) sağlar.
<i>ipairs</i> fonksiyonu, bir tablonun elemanlarını indis sırasına göre yinelemek için kullanılır. <br />
i: Tablodaki elemanın indeksi (sırası)
v: İlgili indekse sahip elemanın değeri

```SQL
local numbers = {10, 20, 30, 40, 50}

for i, v in ipairs(numbers) do
    print("Indeks:", i, "Değer:", v)
end

```

```
//Output

Indeks: 1 Değer: 10
Indeks: 2 Değer: 20
Indeks: 3 Değer: 30
Indeks: 4 Değer: 40
Indeks: 5 Değer: 50
```


> **BİLGİ**
> 
> Lua programlama dilinde bir return ifadesinde birden fazla ifade return edileiblir.
> 
{style="note"}

````SQL
local function multipleReturn()
    return 1, true, "Hello Lua"
end

local a, b, c = multipleReturn();
print(a, b, c);
````