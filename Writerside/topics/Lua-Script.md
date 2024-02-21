# Lua Scripting

## Neden Lua Öğrenmeliyim
Redis, kullanıcıların Lua komut dosyalarını sunucuya yüklemesine ve yürütmesine olanak tanır. Komut dosyaları
programatik kontrol yapılarını kullanabilir ve veritabanına erişmek için yürütülürken komutların çoğunu kullanabilir.
Scriptler sunucuda yürütüldüğü için scriptlerden veri okumak ve yazmak çok verimlidir.

Redis 2.6 versionunda redis için script yazabilme özelliği tanıtıldı. Bu sayede, Redis'i genişletme işini, Lua programlama
diliyle yapabilmeye başladık. Redis 2.6 versionundan önce Redis'i genişletmek için redis'in kaynak kodunda değişiklik
yapmamız lazımdı. Tabi bunun için de C programlama dili ile işlem yapmamız gerekirdi.

Lua, çok küçük ve basit olması ve C API'sinin diğer kütüphanelerle entegre edilmesinin çok kolay olması nedeniyle seçildi.
Hafif olmasına rağmen Lua, çok güçlü bir dildir (genellikle oyun geliştirmede kullanılır)

Lua scriptşeri atomik olarak yürütülür, bu da Redis sunucusunun script yürütme sırasında bloke olduğu anlamına gelir.
Bu nedenle, Redis herhangi bir scripti çalıştırmak için varsayılan olarak 5 saniyelik bir zaman aşımına sahiptir,
ancak bu değer lua-time-limit yapılandırması aracılığıyla değiştirilebilir.

Redis, bir Lua scripti zaman aşımına uğradığında otomatik olarak scripti sonlandırmaz.
Bunun yerine, bir betiğin çalıştığını belirten ve her komuta _BUSY_ mesajıyla yanıt vermeye başlar.
Sunucunun normal durumuna dönmesini sağlamanın tek yolu, _SCRIPT KILL_ veya _SHUTDOWN NOSAVE_ komutu ile script
yürütmesini iptal etmektir. İdeal olarak, asdasd basit, tek bir sorumluluğa sahip olmalı ve hızlı çalışmalıdır.

Popüler oyunlardan olan,  Civilization V, Angry Birds ve World of Warcraft kendi scriptlerinde Lua kullanmaktadır.

