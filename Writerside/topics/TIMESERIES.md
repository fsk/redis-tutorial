# TIMESERIES

* Bir TimeSeries, zaman aralığı boyunca yapılan sıralı değerler dizisi (veri noktaları) anlamına gelir.

* TimeSeries; istatistik, sosyal network ve iletişim mühendisliği alanlarında kullanılabilir. Ama aslında zaman ölçümü 
gerektiren alanlarda rahatlıkla kullanılabilir.

* TimeSeries, gelecekteki borsa değişikliklerini, emlak trendlerini, çevresel koşulları ve daha fazlasını tahmin etmek 
için kullanılabilir. 

* Zaman serilerine örnekler şunlardır:

1. Bir gazetede zaman içinde belirli kelimelerin veya terimlerin kullanımı
2. Yıl bazında asgari ücret
3. Hisse senedi fiyatlarında günlük değişiklikler
4. Ay bazında ürün alımları
5. İklim değişiklikleri

Birçok TimeSeries sistemi, bir veri seti çok hızlı bir şekilde büyüyebileceğinden depolama konusunda zorluklarla karşı 
karşıyadır. Her saniye olayları depolarken, her gün en az 86.400 veri noktası oluşturulur ve uzun bir süre boyunca bu 
kadar çok veri noktasını depolamak, özellikle Redis gibi bellek içi veri depoları için zorludur.