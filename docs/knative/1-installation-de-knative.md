# Install Knative Serving with istio

## Install the Knative Serving component
```shell
kubectl apply -f https://github.com/knative/serving/releases/download/knative-v1.0.0/serving-crds.yaml
kubectl apply -f https://github.com/knative/serving/releases/download/knative-v1.0.0/serving-core.yaml
```

## Install a networking layer - Istio
```shell
kubectl apply -l knative.dev/crd-install=true -f https://github.com/knative/net-istio/releases/download/knative-v1.0.0/istio.yaml
kubectl apply -f https://github.com/knative/net-istio/releases/download/knative-v1.0.0/istio.yaml
kubectl apply -f https://github.com/knative/net-istio/releases/download/knative-v1.0.0/net-istio.yaml

# Check if EXTERNAL-IP is available
kubectl --namespace istio-system get service istio-ingressgateway
# NAME                   TYPE           CLUSTER-IP     EXTERNAL-IP                        PORT(S)                                      AGE
# istio-ingressgateway   LoadBalancer   10.43.54.149   172.23.0.2,172.23.0.3,172.23.0.4   15021:30414/TCP,80:32734/TCP,443:31295/TCP   82s

```

## Deploy the first service - Hello World
Firstly install Knative CLI : https://knative.dev/docs/install/client/install-kn/

```shell
kn service create helloworld3-go --image gcr.io/knative-samples/helloworld-go --env TARGET="Go Sample v1"
sudo bash -c 'echo "127.0.0.1 helloworld3-go.default.example.com" >> /etc/hosts'

# Remove the service
kn service delete helloworld3-go
```
