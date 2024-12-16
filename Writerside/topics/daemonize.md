# daemonize

redis.conf dosyasındaki tanımı

> daemonize no

Bu ayar, Redis sunucusunun arka planda (arka planda çalışan bir progress olarak) çalışıp çalışmayacağını belirtir.

**`Arka planda çalışmak (daemonize):`** Bir uygulamanın, ekranınızda direkt olarak görünmeden, sessizce görevini yapmasıdır. 
Örneğin, bir müzik çalar arka planda çalışarak sürekli müzik çalabilir ama size herhangi bir pencere göstermeyebilir.

`daemonize no:` Redis normal bir program gibi çalışır; başlattığınızda ekranınızda kalır. İşlemi keserseniz (örneğin 
terminali kapatırsanız), Redis de durur.

`daemonize yes:` Redis arka planda, görünmez bir şekilde çalışır, siz terminali kapatsanız bile hizmet vermeye devam eder.


> **NOT**
>
> Modern sistemlerde Redis genellikle `systemd` veya `upstart` gibi servis yöneticileri ile başlatılır. Bu durumlarda 
> daemonize ayarı bir etki göstermez, çünkü systemd/upstart zaten süreci yönetir.
>
{style="tip"}