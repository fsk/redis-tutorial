# REDIS CLUSTER CONSISTENCY GARANTİSİ

* Burada 2 tane consistency kavramı karşımıza çıkmaktadır. **_Eventually Consistency_**,  **_Strongly Consistency_**

**Eventually Consistency**

Eventual consistency, bir dağıtık sistemde, tüm kopyaların zamanla tutarlı hale geleceğini, ancak bunun hemen 
gerçekleşmeyebileceğini kabul eder. Bu, bir güncelleme yapıldıktan sonra, tüm kopyaların anında güncellenmeyebileceği, 
ancak bir süre sonra (sistem belirli bir istikrara ulaştığında) tüm kopyaların aynı değere sahip olacağı anlamına gelir.

* Eventual consistency, sistemdeki arızalara karşı daha yüksek bir tolerans sağlar ve kullanılabilirliği artırır, 
çünkü okuma ve yazma işlemleri, sistemdeki tüm kopyalar güncellenene kadar beklemek zorunda kalmaz.

* Veri kopyalarının hemen güncellenmesi gerekmeyen sistemler, daha fazla kullanıcı ve işlem yükünü kolayca idare edebilir.

* Kullanıcılar, verinin en son durumunu her zaman göremeyebilirler, bu da uygulama mantığını karmaşıklaştırabilir.

* Geliştiriciler, eventual consistency'nin getirdiği tutarsızlıkları yönetmek için ek mantık ve kontrol mekanizmaları 
eklemek zorunda kalabilirler.

Örnek olarak;
1) Sosyal medya platformlarında, bir kullanıcının gönderisi veya durum güncellemesi, platformun farklı bölgelerindeki 
sunuculara yayıldığında, tüm kullanıcılar bu güncellemeyi aynı anda görmeyebilir. Ancak, bir süre sonra, tüm kullanıcılar 
güncellenmiş içeriği görebilir. Bu, yüksek kullanılabilirlik sağlarken aynı zamanda veritabanı üzerindeki yükü de dağıtır.
2) Domain Name System (DNS) güncellemeleri, eventually consistency örneği olarak gösterilebilir. Bir web sitesinin IP 
adresi değiştirildiğinde, bu değişiklik tüm DNS sunucularına yayılmak için zaman alır. Kullanıcılar, bu süreç tamamlanana 
kadar eski veya yeni IP adresine yönlendirilebilir, ancak sonunda tüm kullanıcılar yeni adresi çözecektir.


**Strongly Consistency**

Strongly consistency, bir dağıtık sistemdeki veri kopyalarının her zaman güncel ve tutarlı olmasını gerektirir. 
Bu, bir veri ögesine yapılan bir güncelleme işleminden sonra, tüm sistemdeki tüm okuma işlemlerinin bu güncellemeyi 
hemen yansıtması gerektiği anlamına gelir. Güçlü tutarlılık, sistemdeki herhangi bir gecikme veya arıza durumunda bile, 
verilerin her zaman tutarlı kalmasını sağlar.

* Sistemdeki verilerin durumu her zaman belirgindir ve kullanıcılar son güncellemelerin hemen ardından bu verilere 
erişebilir.

* Yazılımcılar, veri tutarlılığı konusunda endişelenmeden uygulamalarını geliştirebilirler.

* Strongly Consistent gereksinimleri, sistem üzerinde önemli bir yük oluşturabilir ve sistem kaynaklarını aşırı 
kullanabilir, bu da performans ve ölçeklenebilirlik sorunlarına yol açabilir.

* Bazen, sistemdeki bir arıza nedeniyle, veri tutarlılığını korumak adına okuma ve yazma işlemleri geçici olarak 
durdurulabilir.

Örnek Olarak;
1) Bankacılık işlemleri, özellikle hesap bakiyelerinin yönetimi, strongly consistency gerektirir. Bir müşteri hesabından 
para çektiğinde, bu işlemin hemen hesap bakiyesinden düşülmesi ve tüm sistemdeki ilgili kayıtların güncellenmesi gerekir. 
Bu, müşterilerin aynı para birimini birden fazla kez harcamasını önlemek için kritiktir.
2) Bir e-ticaret platformunda, bir ürün satıldığında, stok miktarının hemen güncellenmesi gerekir. Bu, birden fazla 
müşterinin stokta olmayan bir ürünü satın almasını önlemek için önemlidir. Strongly Consistency, bu tür senaryolarda 
yanlış stok bilgilerinin önüne geçer.