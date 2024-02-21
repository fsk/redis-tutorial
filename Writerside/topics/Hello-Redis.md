# Hello-Redis

* Redis'i başlatmak için öncelikle redisin kurulu olduğu klasöre gidilir.
* src klasörünün altına girdikten sonra, bu dosya yolunda, terminal ekranda, `./redis-server` komutu çalıştırılır.
* Bu komut çalıştırıldıktan sonra bu tab'ı kapatmadan, aynı dosya yolunda farklı bir terminal tabı açılarak `./redis-cli`komutu çalıştırılır.

NodeJS'te bir Redis Client'i aşağıdaki gibi oluşturulabilir.

````javascript
import { createClient } from 'redis';
const client = createClient();

await client.connect();

await client.set('first_key', 'Redis Essentials');
const value = await client.get('first_key');

console.log(value);

client
    .quit()
    .then(r => console.log(`${r}`))
    .catch(err => console.log(`${err}`));
````