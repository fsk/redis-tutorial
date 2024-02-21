# HASHES

* Hash'ler keyleri valueler ile eşleyebildiğimiz bir veri yapısıdır. Hem bellek kullanımını etkin bir şekilde optimize
  ederler hem de verileri çok hızlı ararlar. Bir Hash'te hem alan adı hem de değer String'dir. Dolayısıyla, bir Hash,
  String'den String'e bir eşlemedir.
* Hash'lerin bir diğer büyük avantajı bellek optimizasyonudur. Bu optimizasyon,
_hash-max-ziplist-entries_ ve _hash-max-ziplist-value_ konfigürasyonlarına dayanır. 
İç yapısı itibarıyla, bir Hash ya bir _ziplist_ ya da bir _hash table_ olabilir. Bir ziplist, 
bellek açısından etkin olacak şekilde tasarlanmış çift yönlü bir linked list'tir. Bir ziplist'te, 
tamsayılar karakter dizisi yerine gerçek tamsayılar olarak saklanır. Bir ziplist bellek optimizasyonlarına sahip olsa da, 
aramalar sabit zamanda gerçekleştirilmez. Diğer taraftan, bir hash table sabit zamanda arama yapabilir ancak bellek açısından 
optimize edilmemiştir