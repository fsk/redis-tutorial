# appendfilename

redis.conf dosyasındaki tanımı

> appendfilename "appendonly.aof"

AOF modunu kullanıyorsanız, verilerin kaydedileceği dosyaların isimlendirme kurallarını belirler. 
`“appendonly.aof”` bu dosya isimlendirmesinde bir taban isim olarak kullanılır. Redis, buna ek olarak numaralar, 
.base, .incr gibi uzantılar ekleyerek bir dizi dosya yönetir:

**`appendonly.aof.1.base.rdb:`** Temel bir RDB dosyası olabilir.

**`appendonly.aof.1.incr.aof:`** İlk ek dosya (incremental dosya) olabilir.

**`appendonly.aof.manifest:`** Bu dosyaların hangi sırayla uygulanacağını anlatan bir manifest dosyasıdır.

Yani, appendfilename, dosyaların genel adlandırma şablonunu belirler.