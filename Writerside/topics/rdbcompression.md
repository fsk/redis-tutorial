# rdbcompression

redis.conf dosyasındaki tanımı

> rdbcompression yes

Redis, verileri RDB (Redis’in kendi veri yedek formatı) dosyasına kaydederken varsayılan olarak sıkıştırma kullanır. 
Sıkıştırma, dosyadaki verinin daha az yer kaplamasını sağlar, ancak veriyi sıkıştırmak ve açmak biraz işlem gücü gerektirir.

**`yes:`** Veriler sıkıştırılır. RDB dosyası daha az yer kaplar, diskten okuma/yazma daha hızlı olabilir ama CPU tüketimi 
biraz daha artar.

**`no:`** Sıkıştırma yoktur. Dosya daha büyük olur, ama veriyi yazarken/okurken CPU daha az uğraşır.

