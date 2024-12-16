# protected mode

redis.conf dosyasındaki tanımı

> protected-mode yes

Bu ayar aslında bir güvenlik katmanıdır. Bazı durumlarda Redis’i fark etmeden internete tamamen açık halde bırakabiliriz. 
Bu, verilerinize herkesin erişebilmesi gibi ciddi bir güvenlik açığına yol açabilir.

Protected mode açıkken ve varsayılan kullanıcıya (yani Redis’in "default user" olarak adlandırdığı kimliğe) bir parola 
belirlenmediyse, Redis sadece local bağlantılara izin verir. “Local bağlantı” demek;

* 127.0.0.1 (IPv4 için loopback adresi)
* ::1 (IPv6 için loopback adresi)
* Unix domain sockets (makine üzerinde ağ kullanmadan, aynı sistemde çalışan uygulamaların birbirleriyle konuşması için 
özel bir dosya tabanlı iletişim kanalı)
  
Bu adresler dışındaki hiç kimse, başka makinelerden veya farklı IP adreslerinden bağlanamaz.



