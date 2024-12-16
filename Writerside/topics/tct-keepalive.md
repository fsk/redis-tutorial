# tcp-keepalive

redis.conf dosyasındaki tanımı

> tcp-keepalive 300

tcp-keepalive ayarı, Redis sunucusunun bağlı kaldığı client'lara belli aralıklarla küçük sinyaller `(ACK mesajları)` 
göndermesini sağlar. Bu sinyallerin amacı iki sebeptendir:

1. **`Ölü client'ları tespit etmek:`** Eğer client kapandıysa veya ağ bağlantısı koptuysa, bu sinyaller bir süre sonra 
yanıt alamaz. Böylece Redis, bağlantının gerçekten ölü olduğunu anlar ve onu kapatır.
2. **`Ağ ekipmanını uyanık tutmak:`** Bazı ağ yönlendiricileri, uzun süre kullanılmayan bağlantıları “unutur”. Düzenli 
aralıklarla gönderilen bu sinyaller, yönlendiricilere `“Bu bağlantı hala kullanılıyor”` mesajı verir, böylece bağlantı 
aktif tutulur.