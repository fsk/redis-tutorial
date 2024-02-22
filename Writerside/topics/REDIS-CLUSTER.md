# REDIS CLUSTER

* Redis, Redis Cluster adı verilen bir dağıtım topolojisi ile yatay olarak ölçeklenir. 
* Redis Cluster, Redis'in yüksek performanslı bir dağıtık veritabanı çözümüdür.

### Redis Cluster Hedefleri
* 1000 Node'a kadar yüksek performans ve doğrusal ölçeklenebilirlik. Proxy yoktur, asenkron çoğaltma kullanılır ve 
value'ler üzerinde birleştirme işlemi gerçekleştirilmez.

* Kullanılabilirlik: Redis Cluster, ana node'ların çoğunluğuna erişilebilen ve artık erişilemeyen her ana düğüm için en 
az bir erişilebilir kopyanın bulunduğu bölümlerde hayatta kalabilir. Ayrıca replika geçişini kullanarak, artık herhangi 
bir replika tarafından çoğaltılmayan ana bilgisayarlar, birden fazla replika tarafından 
kapsanan bir ana kopyadan bir kopya alacaktır.

* Redis Cluster, otomatik bölümleme (sharding) yoluyla ölçeklenebilirliği sağlar. Veriler, birden fazla Redis düğümü (node) 
arasında bölümlere ayrılır. Bu, daha fazla veri ve daha fazla client isteğini desteklemek için Redis Cluster'ın yatay 
olarak ölçeklenmesine imkan tanır. Her bir bölüm (shard), belirli bir anahtar aralığını yönetir. 

* Redis Cluster, node arızalarına karşı dirençlidir. Her bir bölümde, birincil (master) ve 
birden fazla yedek (slave) düğüm bulunabilir. Birincil düğümde bir sorun oluşursa, yedek düğümlerden biri otomatik olarak 
birincil düğüm olarak terfi eder, böylece veri erişilebilirliği ve yazma işlemleri devam eder.

* Redis Cluster, veri bütünlüğünü ve eventual consistency modelini destekler. Bu, birincil düğüme yazılan verilerin 
yedek düğümlere asenkron olarak kopyalanması anlamına gelir. Ağ bölünmeleri (network partitions) sırasında, 
Redis Cluster bölünmüş beyin (split-brain) sorununu önlemek için özel mekanizmalar kullanır.


### Redis Cluster ile Alakalı Kısa Bilgiler
* Redis Cluster, tek bir anahtar üzerinde çalışan komutları (örneğin, GET, SET, DEL gibi) destekler. Bu, Redis'in temel 
işlevselliğinin dağıtık ortamda da korunduğu anlamına gelir.

* Redis Cluster, birden fazla anahtarın kullanıldığı işlemler için destek sunar, ancak bu işlemler yalnızca tüm 
anahtarlar aynı hash yuvasına düşerse mümkündür. Bu, işlemin atomikliğini ve verimliliğini korumak için gereklidir.

* Kullanıcılar, "{ }" içine alınan karakter dizileri ile hash tagler kullanarak, belirli anahtarların aynı hash yuvasına 
düşmesini sağlayabilir. Bu, çoklu anahtar işlemlerinin belirli durumlarda kullanılabilmesini sağlar.

* Redis Cluster'da, yuvaların yeniden dağıtılması (resharding) işlemi sırasında, çoklu anahtar işlemleri geçici olarak 
kullanılamaz hale gelebilir. Bu, veri tutarlılığını ve işlem bütünlüğünü korumak için gereklidir. 
Ancak, tek anahtar işlemleri etkilenmez ve her zaman kullanılabilir.

* Redis Cluster, Redis'in bağımsız versiyonunda bulunan çoklu veritabanı desteğine sahip değildir. 
Yalnızca tek bir veritabanı (0 numaralı) desteklenir ve SELECT komutu ile başka bir veritabanına geçiş yapılması mümkün 
değildir. Bu, dağıtık sistemin basitliğini ve yönetilebilirliğini artırır.