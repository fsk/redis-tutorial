# Döngüler

## While
Belirli bir koşul doğruyken bir ifadeyi veya ifade grubunu tekrarlar. Döngü body'sini çalıştırmadan önce durumu test eder.

Kod olarak şema aşağıdaki şekildedir.

```
while(condition)
do
    statement(s)
end
```

Statement tek satır da olabilir, block şeklinde de bir statement olabilir.


**NOT:** Burada dikkat edilmesi gereken en önemli nokta while döngüsünün hiç çalıştırılmayabileceğidir.
Statement test edildiğinde ve sonuç yanlış olduğunda döngü body'si atlanacak ve while döngüsünden sonraki ilk ifade
yürütülecektir.

```SQL
local a = 10;

while a < 20 do
    print("value of a ==> ", a)
    a = a + 1;
end
```

## For
For döngüsü, belirli sayıda yürütülmesi gereken bir döngüyü verimli bir şekilde yazmanıza olanak tanıyan bir tekrarlama
kontrol yapısıdır.

Kod olarak aşağıdaki şekildedir.

```
for init,max/min value, increment
do
   statement(s)
end
```

* <i>init</i> adımı önce ve yalnızca bir kez gerçekleştirilir. Bu adım, herhangi bir döngü kontrol değişkenini bildirmenize 
ve başlatmanıza olanak tanır.
* <i>maxs/min</i>, Bu, döngünün çalışmaya devam ettiği maksimum veya minimum değerdir. Başlangıç değeri ile 
maksimum/minimum değer arasında karşılaştırma yapmak için dahili olarak bir durum kontrolü oluşturur
* For döngüsünün gövdesi yürütüldükten sonra kontrolün akışı, increment/decrement ifadesine geri atlar.
  Bu ifade herhangi bir döngü kontrol değişkenini güncellemenizi sağlar.
* Durum şimdi tekrar değerlendirilmektedir. Doğruysa döngü yürütülür ve süreç kendini tekrar eder (döngü gövdesi,
  ardından artış adımı ve ardından yeniden koşul). Koşul false olduktan sonra for döngüsü sonlandırılır.

```SQL
for i = 10,1,-1
do
   print(i)
end
```

## repeat..until
Döngünün üst kısmındaki döngü koşulunu test eden for ve while döngülerinden farklı olarak, Lua programlama dilindeki
Repeat...until döngüsü, döngünün altındaki durumunu kontrol eder.

Repeat...until döngüsü while döngüsüne benzer, tek farkı do...while döngüsünün en az bir kez yürütülmesinin garanti
edilmesidir.


Kod olarak aşağıdaki şekildedir.

```
repeat
   statement(s)
until( condition )
```

Koşullu ifadenin döngünün sonunda göründüğüne dikkat edin, dolayısıyla döngüdeki ifade(ler) koşul test edilmeden önce
bir kez çalıştırılır.

Koşul yanlışsa, kontrol akışı do'ya geri döner ve döngüdeki ifade(ler) yeniden yürütülür.
Bu işlem verilen koşul gerçekleşene kadar tekrarlanır.

```SQL
local number = 10
repeat
    print("value of a ==> ", number)
    number = number + 1
until (number > 15)
```

**NOT:** Eğerki <i>until(number>15)</i> ifadesi false olsaydı repeat altındaki ifade 1 kere çalışacaktı ve döngüden
çıkacaktı.

