# BITMAPS

* Bitmap Redis içerisinde reel bir data type değildir. Temel olarak BitMap bir String'dir.
* Bir BitMap her bir bit'in 0 veya 1 depolayabildiği bir bit dizisidir. Yani bundan dolayı bir BitMap'i
  0'lar ve 1'lerden oluşan bir Array olarak düşünebilirsiniz.
* Bir Bitmap'i String üzerinde bit işlemleri yapan bir set olarak da düşünebiliriz.
* Bitmap'ler ayrıca <i>BitArray</i> ve <i>Bitset</i> olarak da bilinir.
* Redis dokümantasyonu, Bitmap indekslerine offsetler olarak atıfta bulunur. Her bir Bitmap indeksinin ne anlama geldiği uygulama alanı tarafından belirlenir.
  Bitmap'ler hafıza açısından verimli olup, hızlı veri aramalarını destekler ve 2^32 bit'e kadar (4 milyardan fazla bit) saklayabilir.