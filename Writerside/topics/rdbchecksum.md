# rdbchecksum

redis.conf dosyasındaki tanımı

> rdbchecksum yes

RDB dosyalarının sonuna bir `"checksum" yani bir bütünlük kontrol değeri` eklenebilir. Bu sayede dosya bozulmuş mu, 
eksik mi, hatalı mı anlaşılabilir. Checksum hesaplamak da ek bir işlem gerektirir ve yaklaşık %10 performans maliyeti olabilir.

**`yes:`** Dosyanın sonuna bir kontrol bilgisi eklenir. Dosya hasar görürse ya da bir aktarım sırasında bozulursa Redis 
bunu fark edebilir. Daha güvenlidir.

**`no:`** Checksum oluşturulmaz. Biraz daha hızlı olabilir ama dosya bozulduğunda fark etmeyebilirsiniz.

Genelde yes önerilir. no sadece maksimum performans için, veri bütünlüğü kontrolünün önemsiz olduğu ortamlarda kullanılabilir.