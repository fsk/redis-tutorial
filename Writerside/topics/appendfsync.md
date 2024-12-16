# appendfsync

redis.conf dosyasındaki tanımı

> appendfsync everysec

Bu ayar, Redis’in AOF dosyalarını disk üzerine ne zaman fiziksel olarak yazacağını belirler. Disk yazma işlemini 
hızlandırmak için işletim sistemi bazen verileri bellekte tutar, hemen diske yazmaz. fsync işlemi, bu verilerin gerçekten 
diske yazılmasını zorunlu kılar.

**`always:`** Her yazma işleminden sonra diske yaz. En güvenli, ama en yavaş.

**`everysec:`** Her 1 saniyede bir fsync yap. Hem veri kaybı riski çok düşük, hem de performans fena değil.

**`no:`** Hiç fsync yapma, işletim sistemi ne zaman isterse o zaman diske yazsın. En hızlı, ama elektrik kesilirse son 
yazılan bazı veriler kaybolabilir.

Varsayılan everysec, makul bir hız-güvenlik dengesi sağlar.