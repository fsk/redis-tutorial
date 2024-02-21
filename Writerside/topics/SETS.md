# SET

* Redis'te bir Set, tekrarlanana elemanların eklenemeyeceği farklı String'lerin sırasız bir collectionudur.
* Bir Set hash table olarak uygulanmıştır. Bu da aslında bazı işlerin daha da optimize edilmesine olanak sağlamıştır.
* Eleman ekleme, eleman silme ve arama işlemleri sabit zamanda(constant time) yani O(1)'de gerçekleşir.
* Bir Set'in tutabileceği maksimum eleman sayısı 2^32-1'dir, bu da Set başına 4 milyardan fazla eleman olabileceği
  anlamına gelir.
* Eğer Set içindeki tüm elemanlar tamsayı ise, bellek kullanımı daha verimli hale gelir.
* <i>Set-Max-Intset-Entries Konfigürasyonu:</i> Bu konfigürasyon, Set içindeki tamsayı elemanların maksimum sayısını belirler. 
Bu değer, Set'in nasıl saklandığını ve ne kadar bellek kullandığını etkileyebilir.