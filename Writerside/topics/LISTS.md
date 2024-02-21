# LISTS

* Listeler Redis'te oldukça esnek bir yapıya sahiptir. Çünkü listeler basit bir collection gibi, stack gibi ya
  queue gibi çalışabilirler.
* Redis'teki Listeler birer linked list(bağlı liste)dir. Bu yüzden liste'nin başından ya da sonundan eleman eklemeler
  sabit zamanda O(1)'de gerçekleşir.
* Redis'teki bir listenin bir elemanına erişmek O(N)'de yani lineer zamanda gerçekleşir. Bu şu demek. Bir listenin boyutu
  ne kadar büyürse bir elemana ulaşmak o kadar uzar. Ama liste'nin boyutu ne olursa olsun ilk ve son elemanlara erişmek
  her zaman için O(1)'de yani sabit zamanda gerçekleşir.
* Bir liste'nin tutabileceği maksimum eleman sayısı 2^32 - 1 kadardır.
* Redis'teki listeler LIFO ve FIFO mantığıyla çalışırlar.
* Redis listeleri, eşzamanlı sistemlerde bile elemanların kuyrukta çakışma olmadan çıkarılmasını sağlar. Liste
  komutlarının otomatik olması, bir işlemin tamamen gerçekleşeceğini ya da hiç gerçekleşmeyeceğini garanti eder.