# Tables

Lua'da, arrayler ve dictionary'ler  gibi farklı türleri oluşturmamıza yardımcı olan tek veri yapısı tablolardır. 
Lua, ilişkisel arrayler kullanır ve bunlar sadece sayılarla değil, nil hariç stringlerle de indekslenebilir. 
Tabloların sabit bir boyutu yoktur ve ihtiyacımıza göre büyüyebilir.

````SQL
local programmingLanguages = {lua = 1993, javascript = 1995, python = 1991, ruby = 1995}

print("Lua was created in " .. programmingLanguages["lua"])
print("JavaScript was created in " .. programmingLanguages.javascript)
````