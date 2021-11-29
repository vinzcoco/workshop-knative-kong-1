### Cluster management template project

Example [cluster management](https://docs.gitlab.com/ee/user/clusters/management_project.html) project.

This project is based on a GitLab [Project Template](https://docs.gitlab.com/ee/gitlab-basics/create-project.html).

For more information, see [the documentation for this template](https://docs.gitlab.com/ee/user/clusters/management_project_template.html).

Improvements can be proposed in the [original project](https://gitlab.com/gitlab-org/project-templates/cluster-management).


### Test ingress classic

CREATE a local cluster
```bash 
ssh -i ~/.ssh/azureuser.pem azureuser@20.107.102.158
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
sudo apt install -y docker-ce
sudo systemctl status docker
sudo usermod -aG docker ${USER}
su - ${USER}

## build foo.py service
https://gist.github.com/digitalronin/424134ef9a68a11ddcccaed3878b7c68

# https://www.linkedin.com/pulse/install-k3d-linux-kubernetes-installation-guide-prayag-sangode/?trk=articles_directory
# install k3d
curl -s https://raw.githubusercontent.com/rancher/k3d/main/install.sh | bash
k3d cluster create dev1
k3d completion bash > /etc/bash_completion.d/k3d

# kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
mv kubectl /usr/local/bin
kubectl version
echo 'source <(kubectl completion bash)' >>~/.bashrc
echo "alias k='kubectl'" >>~/.bashrc
echo 'complete -F __start_kubectl k' >>~/.bashrc
kubectl config use-context k3d-dev1

# install k9s
sudo snap install k9s

k3d registry create reg1
k3d cluster create --registry-use k3d-reg1:39375
```

EXAMPLE on how to create deployment and service
```bash 
k create deployment chuck --image=gamussa/reactive-quote-service:0.0.3 --dry-run=client -o yaml > chuck-deploy.yml
k apply -f .
k expose deployment chuck --port=8080 -o yaml --dry-run=client > chuck-service.yml
```

Back2Future deployment/service

```bash 
cat << EOF > future-deploy.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: future
  name: future
spec:
  replicas: 1
  selector:
    matchLabels:
      app: future
  template:
    metadata:
      labels:
        app: future
    spec:
      containers:
      - name: future
        image: gamussa/reactive-quote-service:0.0.3
        env:
        - name: KUBERNETES_NAMESPACE
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
        - name: QUOTE_SERVICE
          value: backToFuture
        ports:
        - containerPort: 8080
          name: web
          protocol: TCP
EOF
cat << EOF > future-service.yml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: future
  name: future
spec:
  ports:
  - port: 8080
  selector:
    app: future
EOF
k apply -f .
```

chuck-noris deployment/service

```bash 
cat << EOF > chuck-deploy.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: chuck
  name: chuck
spec:
  replicas: 1
  selector:
    matchLabels:
      app: chuck
  template:
    metadata:
      labels:
        app: chuck
    spec:
      containers:
      - name: chuck
        image: gamussa/reactive-quote-service:0.0.3
        env:
        - name: KUBERNETES_NAMESPACE
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
        - name: QUOTE_SERVICE
          value: CHUCK
        ports:
        - containerPort: 8080
          name: web
          protocol: TCP
EOF
cat << EOF > chuck-service.yml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: chuck
  name: chuck
spec:
  ports:
  - port: 8080
  selector:
    app: chuck
EOF
k apply -f .
```

Traefik ingress (specific to ingress controller)
```bash 
cat << EOF > api-ingress.yml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: api
  annotations:
    kubernetes.io/ingress.class: traefik
spec:
  rules:
  - http:
      paths:
      - path: /chuck
        backend:
          serviceName: chuck
          servicePort: http
      - path: /future
        backend:
          serviceName: future
          servicePort: http
EOF
```

```bash 

```

```bash 

```
