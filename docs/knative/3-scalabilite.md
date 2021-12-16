# Installation du projet sur Knative avec scalabilité

## Deploy the service with scalability
### Add HPA serving to Knative
```shell
kubectl apply -f https://github.com/knative/serving/releases/download/knative-v1.0.0/serving-hpa.yaml
```

### Deploy services
```shell
kubectl create namespace demo2
kubectl apply -f knative/scalable/all.yml
sudo sudo bash -c 'echo "127.0.0.1 www.demo2.example.com blog.demo2.example.com meteo.demo2.example.com" >> /etc/hosts'
```
