# Node.js Ve Lua Örnek 1

* Öncelik olarak, `Script1.lua` adında bir tane lua script dosyası açalım ve içerisine aşağıdaki kodu yazalım.

```SQL
return redis.call("SET", "book:1", "Kafka in Action");
```

* Aynı dizinde bir tane `EvalMethod.js` javascript file'ı açalım ve `npm init -y` ile node.js için gerekli konfigürasyonları
ekleyelim.

* `npm install redis` ile redis paketini import edelim.

* `package.json` içerisinde `type: "module"` olarak değiştirelim. (Bu adım opsiyoneldir. Type olarak common ile de devam
edilebilir.)

* Son adım olarak, `fs` modülünü import edip aşağıdaki kodu yazabiliriz.

``` Javascript
import { createClient } from "redis";
import { readFile } from "fs/promises";

const client = createClient();

await client.connect();

const scriptContent = await readFile('/SizinLuaScriptinizinBulunduguPath/Script1.lua');

let result = await client.eval(scriptContent, 0);

console.log(`Eval Result ==> ${result}`);

client
    .quit()
    .then(item => console.log(item))
    .catch(err => console.log(err));
```

* Bu kodu çalıştırdığımız zaman redis terminal'den `GET "book:1"` komutunu çalıştırdığımız zaman, `Kafka in Action`
çıktısını görebiliriz.

* Opsiyonel olarak buradaki `eval` methodu içerisine _string olarak yazılmış Lua Script'ini de verebiliriz._