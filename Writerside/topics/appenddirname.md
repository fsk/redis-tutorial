# appenddirname

redis.conf dosyasındaki tanımı

> appenddirname "appendonlydir"

AOF dosyalarının saklanacağı özel bir dizin (klasör) belirtir. Bu sayede RDB dosyalarınız ayrı yerde, AOF dosyalarınız 
ayrı bir klasörde tutulabilir. Varsayılan olarak “appendonlydir” adında bir klasör kullanılır.

Bu, düzeni sağlamaya ve dosyaları kategorize etmeye yarar.