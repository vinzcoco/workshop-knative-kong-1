# Installation du projet sur Knative

## Deploy the simple service
```shell
kubectl create namespace demo1
kubectl apply -f knative/simple/all.yml
sudo bash -c 'echo "127.0.0.1 www.demo1.example.com blog.demo1.example.com meteo.demo1.example.com" >> /etc/hosts'
```
