Aşağıdaki gibi bir values.yaml dosyamızın olduğunu düşünelim.

```
deployment:
  name: webapp-deployment
  replica: 2

  image:
    repository: nginx
    tag: 1.19

  ports:
    port1: 80
    port2: 8080

```
 Bir deployment manifest dosyasında \
 .Values.deployment.name, \
.Values.deployment.replica, \
.Values.deployment.image.repository, \
.Values.deployment.image.tag \

şeklinde sürekli uzun uzun yazmamız gerekiyor. Bu işlemi daha az zahmetle yapmak için with bloğunu kullanacağız.

----------------------------------------------
```
{{- with .Values.deployment }}   #En tepede temel bloğumu yazıyoruz ve end ile bitiriyoruz.
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .name }}  #.Values.deployment.name  anlamına gelir.
spec:
  replicas: {{ .replica }} #.Values.deployment.replica  anlamına gelir.
  selector:
    matchLabels:
      app: {{ .name }}
  template:
    metadata:
      labels:
        app: {{ .name }}
    spec:
      containers:
        {{- with .image }}  # Burada .Values.deployment.image 
        - image: {{ .repository }}:{{ .tag }} #.Values.deployment.image.image ve tag şeklinde tamamlanır.
        {{- end }}
          name: webapp-deployment
        {{- with .ports }}  #.Values.deployment.ports
          ports:
            - containerPort: {{ .port1 }}   #.Values.deployment.ports.port1 
            - containerPort: {{ .port2 }}   #.Values.deployment.ports.port2
        {{- end }}
{{- end }}
```

Burada dikkat edilmesi gereken end bloklarını doğru yerde kapatmak. Biz en tepede .Values.deployment şeklinde bir tanım yaptık ama diyelim ki bir değerimiz bu bloğun içerisinde değil. Aşağıdaki values.yaml dosyasını incelersek ve biz eğer manifest dosyasının içerisinde "{{ .Values.container.name }}" yazarsak ".Values.deployment.Values.container.name" şeklinde algılar ve yanlış sonuç döner. Bir değeri bu bloktan muaf tutmak istiyorsak "{{ $.Values.container.name }}" şeklinde başına $ işareti koyarak gerçekleştirebiliriz.

```
deployment:
  name: deployment1
  replica: 2

container:
  name: container1
```
