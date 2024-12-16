# databases

redis.conf dosyasındaki tanımı

> databases 16

* Redis, tek bir sunucu içinde birden fazla database barındırabilir. Varsayılan olarak 16 tane db kullanıma hazırdır.
* DB0, DB1, … DB15 olmak üzere numaralanır.
* Bir client olarak Redis’e bağlandığınızda otomatik olarak DB0 seçili gelir.
* `SELECT <dbid>` komutu ile istediğiniz db'ye geçebilirsiniz. Mesela `SELECT 2` derseniz 2 numaralı db'yi kullanmaya 
başlarsınız.
