# REDIS CLUSTER ÇALIŞMA PRENSİBİ

* Redis Cluster, her bir node'un iki adet TCP bağlantı noktasına (port) ihtiyaç duyduğu bir yapılandırmadır.

**_Redis TCP Portu:_** Bu, client'ların Redis server'a bağlanıp komutlar gönderdiği porttur. Genellikle 6379 olarak 
ayarlanır. Bu port, hem dış dünyadan gelen client bağlantıları için hem de cluster içindeki diğer node'larla yapılan 
key migrationları gibi işlemler için kullanılır.

**_Cluster Bus Portu:_** Node'ların birbiriyle iletişim kurduğu porttur. Varsayılan olarak, bu port, Redis TCP portunun 
üzerine 10000 eklenerek belirlenir (örneğin, eğer Redis TCP portu 6379 ise, cluster bus portu 16379 olur). Bu port 
üzerinden, node'lar arası yapılandırma güncellemeleri, arıza tespiti, failover yetkilendirme gibi işlemler 
gerçekleştirilir. İletişim, az bant genişliği ve işlem süresi gerektiren ikili bir protokol aracılığıyla yapılır.

Her iki portun da firewall üzerinden açık olması gerekmektedir. Aksi takdirde, Redis Cluster node'ları birbirleriyle 
iletişim kuramaz. Bu durum, cluster'ın düzgün çalışmasını engelleyebilir.


Özetle, her Redis Cluster node'u için şunlar gereklidir:

**Client İletişim Portu (Redis TCP Portu):** Client'ların ve diğer node'ların erişebileceği, client isteklerini kabul 
eden port.

**Cluster Bus Portu:** Yalnızca node'ların birbirleriyle iletişim kurduğu, cluster içi koordinasyon ve yönetim 
işlemlerinin gerçekleştiği port.

Bu iki portun düzgün ayarlanması ve erişilebilir olması, Redis Cluster'ın sağlıklı ve etkin bir şekilde çalışması için 
kritik öneme sahiptir.