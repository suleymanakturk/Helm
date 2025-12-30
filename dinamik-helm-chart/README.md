Helm chartlar üzerinde ortama göre (stage, dev, prod) gibi ortamlar için bazı değerlerin değişmesi gerekebilir. Örneğin; replica sayısı, kaynak kısıtlamaları, objelerin isimleri vb.  Helm üzerinde bu işlemleri values.yaml isminde bir dosyada belirtiyoruz. Manifest dosyalarımızda ise bu değerler için tanımlama yapmamız gerekmektedir.

Klasör yapısı 

```
.
├── charts
├── Chart.yaml
├── my-values.yaml
├── templates
│   ├── python-app-database-service.yaml
│   ├── python-app-database.yaml
│   ├── python-app-deployment-service.yaml
│   └── python-app-deployment.yaml
└── values.yaml
```

Aşağıdaki gibi bir "values.yaml" dosyam var.

```
replica: 2
image:
  name: suleymanakturk/devops-project
  tag: 770cae23

resources:
  limits:
    cpu: 250m
    memory: 500Mi
  requests:
    cpu: 125m
    memory: 125Mi
```
Manifest dosyamda aşağıdaki gibi , eğer ben özel bir values.yaml dosya oluşturmazsam yukarıda değerler ile uygulamayı ayağa kaldırır. "values.yaml" dosyasında her şey hiyerarşik bir yapıda ilerliyor bunun içinde manifest dosyasında da buna dikkat ederek yazmamız gerekiyor.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: python-app
  labels:
    app: python-app
spec:
  replicas: {{ .Values.replica }}
  selector:
    matchLabels:
      app: python-app
  template:
    metadata:
      labels:
        app: python-app
    spec:
      containers:
      - name: python-app
        image: "{{ .Values.image.name }}:{{ .Values.image.tag }}"
        ports:
        - containerPort: 5000
        # Resources konteyner seviyesinde olmalı
        resources:
          limits:
            cpu: "{{ .Values.resources.limits.cpu }}"
            memory: "{{ .Values.resources.limits.memory }}"
          requests:
            cpu: "{{ .Values.resources.requests.cpu }}"
            memory: "{{ .Values.resources.requests.memory }}"
      # imagePullSecrets konteyner listesiyle aynı hizada olmalı
      imagePullSecrets:
      - name: regcred
```

`my-values.yaml` dosyamda replica: 3 yaparsam uygulamam 3 replica ile ayağaca kalkacaktır. Değiştirmediğim değerler için values.yaml dosyasındaki değerler geçerli olacaktır.

``helm install ilk-helm-chart -f my-values.yaml`` ile oluşturduğumuzda uygulamalarımızın istediğim şekilde ayağa kalkmasını sağlayabiliriz.

