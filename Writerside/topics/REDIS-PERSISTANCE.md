# REDIS PERSISTANCE

Redis in memory bir çözümdür. Memory ise geçicidir. Bu yüzden redis'teki veriler kalıcı değildir. Herhangibir şekilde
Redis instance'ı çökerse, kapanırsa ya da yeniden başlatılması gerekirse datalar kaybolacaktır.

Redis bu noktada persistance problemiyle alakalı 2 tane mekanizma sunar.

**Redis Database (RDB)**

**Append-only File (AOF)**

Bu mekanizmaların her ikisi de aynı Redis instance'ında ayrı ayrı veya aynı anda kullanılabilir.