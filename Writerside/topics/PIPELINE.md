# PIPELINE

* Redis'te, bir pipeline, bireysel yanıtları beklemeden birden fazla komutun Redis sunucusuna birlikte gönderilmesi
  için bir yoldur.

* Yanıtlar, client tarafından hepsi bir kerede okunur. Bir Redis client'ının bir komut göndermesi ve Redis server'ından
  bir yanıt alması için geçen süreye Round Trip Time (RTT) denir. Birden fazla komut gönderildiğinde, birden fazla RTT olur.

* Pipelines, komutların gruplandırılması nedeniyle RTT sayısını azaltabilir, bu yüzden 10 komutlu bir pipeline
  yalnızca bir RTT'ye sahip olur. Bu, ağın performansını önemli ölçüde iyileştirebilir.
  Örneğin, bir client ile server arasındaki ağ bağlantısının RTT'si 100 ms ise, saniyede gönderilebilecek maksimum komut
  sayısı 10'dur, Redis server'ının işleyebileceği komut sayısı ne olursa olsun. Genellikle, bir Redis server'ı saniyede
  yüz binlerce komut işleyebilir ve pipeline kullanmamak kaynakların boşa harcanması anlamına gelebilir.

* Redis pipelining, her bir komutun yanıtını beklemeden birden fazla komutun bir kerede verilmesiyle performansı artıran
  bir tekniktir. Pipelining, çoğu Redis client'ı tarafından desteklenir.