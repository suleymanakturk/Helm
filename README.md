**Helm Nedir** \
Kubernetes kullanan herkesin mutlaka öğreneceği bir yazılım olan helm için kubernetesin paket yöneticisi diyebiliriz. İşletim sistemlerinde ki apt, dnf gibi hızlı bir şekilde uygulamalarımızı kurmamızı sağlamaktadır.


Örneğin; wordpress altyapısı için bir wordpress yazılımı ve arka tarafında mysql veritabanı çalışması gerekmektedir. Biz bu yapıyı manuel olarak deploymentlar, statefulset ve servisler ile ayağa kaldırabiliriz. Ama helm ile bu yapıyı tek bir komutla ayağa kaldırabiliriz.

Wordpress iki bileşenden oluşuyor ama uygulamamız için deploymentlar, statefulset, servisler, persistencevolume, serviceaccount, rolebinding gibi fazla sayıda objemiz var ise bu yapıyı farklı clusterlarda ayağa kaldırmak iş yükü çıkartacaktır. Helm ile bu yapıyı bir paket haline getirip tek komut ile ayağa kaldırabiliriz.

Bir güzel özelliği daha ortama bağlı olarak dinamik değerlerin kullanılabilmesidir. Örneğin; dev veya prod ortamları için farklı kaynak limitleri, pod replika sayıları vb. oluşturabiliriz.
