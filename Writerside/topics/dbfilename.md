# dbfilename

redis.conf dosyasınaki tanımı

> dbfilename dump.rdb

Bu ayar, Redis’in diske kaydettiği RDB dosyasının adını belirler.

* Varsayılan olarak `dump.rdb` adında bir dosya oluşturur. Eğer isterseniz farklı bir isim verebilirsiniz,
mesela `dbfilename myredis.rdb` gibi.

* Bu dosya `dir` ayarıyla belirlenen dizinde oluşturulur.