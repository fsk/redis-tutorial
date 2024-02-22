# REDIS CLUSTER MASTER REPLICA MODEL

Her master node için bir veya daha fazla replika node bulunur. Bu replikalar, master node'lardaki verilerin bir kopyasını 
tutar. Bu sayede, bir master node başarısız olduğunda, ilgili replika node, master node'un görevini üstlenerek veri kaybını 
ve sistemin durmasını önler. Ancak, bir master ve onun replikası aynı anda başarısız olursa, o hash slot aralığı için 
hizmet verilemez hale gelir ve bu, cluster'ın tamamının kullanılamaz hale gelmesine neden olabilir. 
Bu, Redis Cluster'ın tasarımında dikkate alınması gereken bir kısıtlamadır ve yüksek kullanılabilirlik için düzgün bir 
replikasyon ve failover stratejisi gerektirir.

> **BİLGİ**
> 
> `Master-replika` (bazen master-slave olarak da adlandırılır) modeli, veri tabanı yönetimi, dosya sistemleri ve ağ 
> servisleri gibi bilgi sistemlerinde yaygın olarak kullanılan bir replikasyon yöntemidir. 
> Bu modelde, "master" olarak adlandırılan birincil node veya sunucu, verilerin asıl kaynağı olarak hizmet verirken, 
> bir veya daha fazla "replika" (veya "slave") node, master'daki verilerin kopyalarını tutar.
> 
{style="note"}

> **BİLGİ**
> 
> `Master node`, tüm yazma ve güncelleme işlemlerini kabul eder. Bu, veri bütünlüğünün ve tutarlılığının korunmasını sağlar.
> Master, verileri replika node'lara aktararak verilerin kopyalarının tutulmasını sağlar. Bu işlem genellikle asenkron 
> olarak gerçekleşir, yani master'daki bir değişiklik hemen tüm replikalara yansımayabilir.
> Genellikle okuma işlemleri hem master'dan hem de replikalardan yapılabilir, ancak yazma işlemleri yalnızca master 
> üzerinden gerçekleştirilir.
> 
{style="note"}

> **BİLGİ**
> 
> `Replika node`, genellikle okuma yükünü dağıtmak için kullanılır. Bu, master üzerindeki yükü azaltır ve sistem genelinde 
> daha hızlı yanıt süreleri sağlar.
> Bir master başarısız olduğunda, bir replika master olarak terfi edebilir, böylece sistem kesintiye uğramadan çalışmaya 
> devam eder. Bu, yüksek kullanılabilirlik ve hata toleransı sağlar.
> Replikalar, veri yedeklemesi için de kullanılabilir, bu sayede kritik verilerin kaybı önlenir.
> 
{style="note"}