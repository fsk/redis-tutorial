# EVALSHA Method Örneği

````java
return Hello Scripting
````

Yukarıdaki ifadeyi redis'e load etmek istediğimiz zaman yapmamız gereken aslında şudur.

``SCRIPT LOAD "return 'Hello, Scripting.!'"``

Bu commandi redis-cli'ya yazdığımız zaman bize bir sha1 key'i verecektir. Bu key'i ve EVALSHA methodunu kullanarak
dilediğimiz gibi bu scripti çalıştırabiliriz.

```
EVALSHA 8068f91fecf5290765067fd3f1e6b38a23bad339 0
```