# rdb del sync files 

redis.conf dosyasındaki tanımı

> rdb-del-sync-files no

Bu ayar, özel bir duruma yönelik. Redis bazen replikalar oluştururken veya ilk senkronizasyonda RDB dosyasını diske yazar.
Eğer sizin sisteminizde hiç kalıcılık (RDB veya AOF) yoksa ama yine de senkronizasyon için kısa süreli RDB dosyası yazıldıysa, 
bu dosyayı hemen silmek isteyebilirsiniz. Bazı kurumlar güvenlik veya regülasyon gereği diskte böyle geçici dosyaların 
kalmasını istemez.

**`yes:`** Bu dosyalar hemen silinir.

**`no:`** Bu dosyalar silinmez.

Eğer kalıcılık tamamen kapalıyken (ne RDB ne AOF) bu ayarı yes yaparsanız, bu geçici dosyalar oluşup hemen silinir. 
no ise varsayılan davranıştır, yani silmez.