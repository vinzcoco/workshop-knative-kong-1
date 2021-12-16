# Installation du projet sur Knative avec scalabilité et Kong

## Install kong

```shell
kubectl create namespace demo3
kubectl label namespace demo3 istio-injection=enabled

helm repo add kong https://charts.konghq.com
helm repo update
helm install -n demo3 kong kong/kong
kubectl patch configmap/config-network \
  --namespace knative-serving \
    --type merge \
      --patch '{"data":{"ingress.class":"kong"}}'
```

## Deploy services
```shell
kubectl apply -f knative/kong/all.yml
sudo sudo bash -c 'echo "127.0.0.1 www.demo3.example.com blog.demo3.example.com meteo.demo3.example.com" >> /etc/hosts'
kubectl delete -f knative/kong/all.yml

```
