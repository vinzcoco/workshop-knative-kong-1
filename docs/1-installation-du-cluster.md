# Install kubernetes locally

## Install Cluster with k3d
```shell
# Start Cluster
k3d cluster create dev  \
          -p "443:443@loadbalancer" \
          -p "80:80@loadbalancer" \
          -p "8081:30080@loadbalancer" \
          -p "8441:30443@loadbalancer" \
          --k3s-arg "--disable=traefik@server:*" \
          --k3s-arg "--cluster-domain=example.com@loadbalancer" \
          --servers 1 \
          --api-port 6443 \
          --agents 2 \
          --wait
          
# Stop cluster
k3d cluster stop dev
         
# Delete cluster
k3d cluster delete dev         
```
## Install Cluster with Minikube

```shell
# Start Cluster
minikube profile knative
minikube start -p knative --cpus=8 --memory=15000 --addons=ingress

# Forward port
minikube -p knative tunnel

# Stop cluster
minikube stop -p knative
minikube destroy -p knative
```
