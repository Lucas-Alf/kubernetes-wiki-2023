# kubernetes-wiki-2023
## Atualizar os pacotes do sistema
```
sudo apt update
sudo apt upgrade
```

## Instalar o pacote do OpenSSH-Server
```
sudo apt install openssh-server
```

## Instalar os pacotes do Docker
O seguinte comando irá instalar Docker e todas as suas dependencies, podendo demorar um pouco.
```
sudo apt install -y docker.io
```

## Habilitar o uso do Docker sem o sudo
```
sudo groupadd docker
```
```
sudo usermod -aG docker $USER
```
```
newgrp docker
```


## Instalar os pacotes do Curl
```
sudo apt install -y apt-transport-https curl
```

## Instalar o pacote do Golang
```
curl -LO "https://go.dev/dl/go1.20.4.linux-amd64.tar.gz"
```
```
sudo tar -C /usr/local -xzf go1.20.4.linux-amd64.tar.gz
```
```
export PATH=$PATH:/usr/local/go/bin
export PATH=$PATH:$(go env GOPATH)/bin
```
```
source .profile
```
```
go version
```

## Instalar o Kind
```
go install sigs.k8s.io/kind@v0.19.0
```

## Criar um novo cluster
```
kind create cluster
```

## Instalar o pacote do Kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
```
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```
```
kubectl version --short
```

## Instalar o Dashboard do Kubernetes
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```

## Fazer o proxy de rede para ter acesso ao dashboard
```
kubectl port-forward -n kubernetes-dashboard svc/kubernetes-dashboard 8080:443 --address 0.0.0.0
```
```
https://192.168.184.128:8080/#/login
```
