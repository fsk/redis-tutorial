# TRANSACTION Rollbacks

* Rollback işlemlerinin desteklenmesi, Redis'in basitliği ve performansı üzerinde önemli bir etkiye sahip olacağından
  Redis, transactionların rollback olmasını desteklemez.

* _**DISCARD**_ bir transactionu iptal etmek için kullanılabilir. Bu durumda hiçbir komut yürütülmez ve 
bağlantının durumu normale döndürülür