# TRANSACTION Watch

* Redis Dokümantasyonunda Optimistic-Locking Using Check-And-Set diye bir kavram vardır.

* **_WATCH_**, Redis transactionlarında bir check-and-set (CAS) davranışı sağlamak için kullanılır.

* *WATCH*ed key'ler, onlara karşı yapılan değişiklikleri tespit etmek için izlenir. 

* EXEC komutundan önce en az bir WATCHED key değiştirilirse tüm transaction iptal edilir ve EXEC, transactionun
başarısız olduğunu bildirmek için bir Null yanıtı döndürür.

* Bu durum özellikle Optimistic Locking mekanizmasını kullanarak race condition durumlarının önüne geçer.