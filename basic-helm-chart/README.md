Öncelikle aşağıdaki komutla helm chart oluşturacağız, bu komut bize helm için dosya şablonunuda oluşturacak

helm create my-helm-chart 

```
my-helm-chart/
├── charts
├── Chart.yaml
├── templates
│   ├── deployment.yaml
│   ├── _helpers.tpl
│   ├── hpa.yaml
│   ├── httproute.yaml
│   ├── ingress.yaml
│   ├── NOTES.txt
│   ├── serviceaccount.yaml
│   ├── service.yaml
│   └── tests
│       └── test-connection.yaml
└── values.yaml

```

Benim bir python uygulamam  ve bağlanması gereken bir mysql veritabanı var ve ikisi de içinde servisler var.  Ben bu dosyaları templates altına atıyorum , mevcuttaki dosyaları silebiliriz.

```
../templates/
├── python-app-database-service.yaml
├── python-app-database.yaml
├── python-app-deployment-service.yaml
├── python-app-deployment.yaml
└── README.md
```

Ben ana klasörde `` helm install ilk-helm-chart . `` komutunu çalıştırsam aşağıdaki sonucu alacağız.

```
root@master:~/helm/my-helm-chart# helm install ilk-helm-chart .
NAME: ilk-helm-chart
LAST DEPLOYED: Tue Dec 30 20:31:14 2025
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

REVISON: 1 demek bu uygulamanın ilk versiyonu demektedir. Eğer bir değişiklik yapıp ``helm upgrade ilk-helm-chart .`` komutunu çalıştırsak REVISION: 2 olacaktır.

Uygulamamızı `` helm uninstall ilk-helm-chart `` komutu ile hızlıca kaldırabiliriz.
