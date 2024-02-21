# Stringler

Stringler character'lerin birleşiminden oluşan dizelerdir.
String'ler 3 farklı şekilde oluşturulabilir.
* Tek tırnak arasındaki karakterler
```
string1 = 'Lua Programming Language
```
* Çift tırnak arasındaki karakterler
```
string1 = "Lua Programming Language
```
* Köşeli parantezler arasındaki karakterler
```
string1 = [["Lua Programming Language"]]
```

```
stringIfade = "Lua Programming Language";
```

| Method Adı                                                         | Method Amacı                                                                                        | Method Kullanımı                                                |
|--------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| upper(argument)                                                    | Bütün String'i büyük harfe çevirir.                                                                 | string.upper(stringIfade)                                       |
| lower(argument)                                                    | Bütün String'i küçük harfe çevirir                                                                  | string.lower(stringIfade)                                       |
| gsub(mainString, findString, replaceString)                        | findString'in oluşumlarını replace ile değiştirerek bir string dönderir.                            | string.find(mainString, findString, replaceString)              |
| find(mainString, findString, optionalStartIndex, optionalEndIndex) | Ana String'deki findString'in başlangıç indeksini ve bitiş indeksini döndürür. Bulamazsa nil döner. | string.find(mainString, findString, optionalStart, optionalEnd) |
| reverse(arg)                                                       | String'i ters çevirir.                                                                              | string.reverse(string1)                                         |
| format(...)                                                        | Formatlanmış bir String döndürür.                                                                   | string.format(...)                                              |
| char(arg) and byte(arg)                                            | String'lerin sayısal ve karakter temsillerini döndürür.                                             | string.char(arg)                                                |
| len(arg)                                                           | String'in uzunluğunu döndürür.                                                                      | string.len(stringIfade)                                         |
| rep(string, n)                                                     | Aynı string'i n sayıda yineleyerek bir String döndürür.                                             | string.rep(string, n)                                           |
| ..                                                                 | İki String'i birleştirir.                                                                           |                                                                 |
 