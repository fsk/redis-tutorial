# CONSISTENCY GARANTİSİ

Redis Cluster, strongly consistency garantisi vermez. Pratik anlamda bu, belirli koşullar altında, sistem tarafından 
clienta onaylanmış olan yazma işlemlerinin Redis Cluster tarafından kaybedilebileceği anlamına gelir.

Redis Cluster'ın yazma işlemlerini kaybetme nedenlerinin ilki, asenkron replikasyon kullanmasıdır. 

Asenkron replikasyon sürecinde, master node, istemciye yazma işleminin başarılı olduğunu hemen bildirir, 
ancak bu yazma işleminin replika node'lara yayılması biraz zaman alabilir.

Bu süre zarfında, eğer master node başarısız olursa ve yazma işlemi henüz replikalara ulaşmadıysa, bu yazma işlemi 
kaybolabilir. Bu, özellikle yüksek kullanılabilirlik ve hızlı yanıt süreleri gerektiren sistemlerde tercih edilen bir 
yaklaşım olsa da, bazı durumlarda yazma işlemlerinin kaybolabileceği riskini barındırır.

Bu model, Redis Cluster'ın veri tutarlılığı ve sistem performansı arasında bir denge kurmasını sağlar.


>**SAKIN UNUTMA.!**
> 
>  Master node, client'a yazma işleminin başarılı olduğunu hemen bildirir, ancak bu işlemin replikalara ulaşması biraz 
> zaman alabilir. Eğer master node, replikalara yazma işlemini yaymadan önce herhangi bir sebepten ötürü çökerse, 
> bu yazma işlemi kaybolur. Daha sonra, yazma işlemini alamamış olan replikalardan biri yeni master olarak terfi 
> ettirilirse, bu, kaydedilen verinin sonsuza dek kaybolmasına neden olabilir. Redis Cluster bu riski azaltmak için 
> belirli stratejiler sunsa da, asenkron replikasyonun doğası gereği bu tür bir veri kaybı riski 
> tamamen ortadan kaldırılamaz.
>
{style="warning"}

* Redis Cluster, kesinlikle gerekli olduğunda senkron yazma işlemlerini destekler ve bu, _WAIT_ komutu aracılığıyla 
gerçekleştirilir. Bu, yazma işlemlerinin kaybolma olasılığını önemli ölçüde azaltır. Ancak, senkron replikasyon kullanılsa 
bile Redis Cluster'ın strongly consistency sağlamadığını unutmamak önemlidir. Daha karmaşık başarısızlık senaryoları 
altında, yazmayı alamayan bir replikanın master olarak seçilmesi her zaman mümkündür.

* Redis Cluster'ın yazma işlemlerini kaybettiği bir diğer önemli senaryo ise, bir ağ bölünmesi (network partition) 
sırasında bir client'in, en az bir master içeren azınlık örnekleriyle izole edildiği durumlarda meydana gelir. 
Bu tür bir ağ bölünmesi durumunda, izole edilmiş master, hala yazma işlemlerini kabul edebilir ve istemciye onay verebilir. 
Ancak, bu master, cluster'ın geri kalanıyla iletişim kuramadığı için, yazılan verilerin cluster'ın geri kalanındaki 
node'lar arasında replike edilmesi mümkün olmayabilir. Eğer bu izole master daha sonra çoğunlukla yeniden birleştiğinde, 
yazma işlemleri kaybolmuş olabilir çünkü bu sırada başka bir master seçilmiş ve veri seti güncellenmiş olabilir.

* Örneğimizde 6 node'lu cluster'ı, 3 master (A, B, C) ve 3 replika (A1, B1, C1) ile düşünelim. Ayrıca Z1 adında bir client 
olduğunu varsayalım. Bir ağ bölünmesi meydana geldiğinde, bölünmenin bir tarafında A, C, A1, B1, C1 node'ları, diğer 
tarafında ise B ve Z1 client'i bulunabilir. Z1, B'ye yazma işlemleri yapmaya devam edebilir ve B bu yazma işlemlerini 
kabul edecektir. Eğer ağ bölünmesi çok kısa sürede çözülürse, cluster normal şekilde çalışmaya devam eder. 
Ancak, ağ bölünmesi B1'in çoğunluk tarafında master olarak terfi ettirilmesi için yeterli süre kadar sürerse, 
bu süre zarfında Z1'in B'ye gönderdiği yazma işlemleri kaybolacaktır. Bu senaryoda, B1'in master olarak terfi ettirilmesi, 
B'nin izole edilmiş tarafında yapılan yazma işlemlerinin cluster'ın geri kalanına uygulanamayacağı anlamına gelir. Ağ 
bölünmesi çözüldüğünde ve B tekrar cluster'ın bir parçası haline geldiğinde, B'deki yazma işlemleri, artık geçerli master 
olan B1'in veri setiyle uyumsuz hale gelir. Bu, dağıtık sistemlerde ağ bölünmeleri ve replikasyon stratejileri ile ilgili 
kritik bir durumdur ve veri kaybı riskini yönetmek için dikkatli planlama ve tasarım gerektirir.