# loglevel

redis.conf dosyasındaki tanımı

> loglevel notice

Bu ayar, Redis’in log bilgilerinin ne kadar detaylı olacağını belirler. Log, Redis’in ne yaptığını metin olarak kaydeden 
bir tür kayıt sistemidir.

**`debug:`** Çok ayrıntılı bilgi. Genellikle geliştiriciler veya hata ayıklama için kullanır.

**`verbose:`** Oldukça detaylı, ancak debug kadar aşırı değil.

**`notice:`** Orta düzeyde detay. Prod ortamlarında genellikle iyi bir dengedir. Önemli olaylar kaydedilir.

**`warning:`** Sadece çok önemli veya kritik mesajları yazar.

**`nothing:`** Hiçbir şey kaydetmez, tamamen sessizdir.


