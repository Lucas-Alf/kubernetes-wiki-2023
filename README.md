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

## Instalar os pacotes do Curl
```
sudo apt install -y apt-transport-https curl
```

## Download dos scripts
```
curl -LO "https://raw.githubusercontent.com/Lucas-Alf/kubernetes-wiki-2023/main/demo-api.yaml"
curl -LO "https://raw.githubusercontent.com/Lucas-Alf/kubernetes-wiki-2023/main/cluster-role-binding.yaml"
curl -LO "https://raw.githubusercontent.com/Lucas-Alf/kubernetes-wiki-2023/main/service-account.yaml"
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

## Instalar o pacote do Golang
```
curl -LO "https://go.dev/dl/go1.20.4.linux-amd64.tar.gz"
```
```
sudo tar -C /usr/local -xzf go1.20.4.linux-amd64.tar.gz
```

### Definir variaveis de ambiente
```
export PATH=$PATH:/usr/local/go/bin
export PATH=$PATH:$(go env GOPATH)/bin
```

### Atualizar variaveis de ambiente do shell atual
```
source .profile
```

### Verificar se o Golang foi instalado com sucesso
```
go version
```

## Instalar o Kind
```
go install sigs.k8s.io/kind@v0.19.0
```

### Criar um novo cluster
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

### Verificar se o Kubectl foi instalado com sucesso
```
kubectl version --short
```

## Instalar o Dashboard do Kubernetes
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```

### Fazer o proxy de rede para ter acesso ao dashboard
```
kubectl port-forward -n kubernetes-dashboard svc/kubernetes-dashboard 8080:443 --address 0.0.0.0
```

### URL de acesso do dashboard
```
https://192.168.184.128:8080/#/login
```

### Execução dos scripts para criar um usuário de acesso do Dashboard
```
kubectl apply -f service-account.yaml
```
```
kubectl apply -f cluster-role-binding.yaml
```

### Criar token para acessar o Dashboard
```
kubectl -n kubernetes-dashboard create token admin-user
```

## Instalar o MetalLB
```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.10/config/manifests/metallb-native.yaml
```

### Esperar todos os recursos do MetalLB serem criados
```
kubectl wait --namespace metallb-system \
                --for=condition=ready pod \
                --selector=app=metallb \
                --timeout=90s
```

### Definir faixa de IPs controladas pelo MetalLB
```
docker network inspect -f '{{.IPAM.Config}}' kind
kubectl apply -f https://kind.sigs.k8s.io/examples/loadbalancer/metallb-config.yaml
```


## Deployment da aplicação de demonstração
```
kubectl apply -f demo-api.yaml
```

### Realizar o proxy de rede para ter acesso a aplicação
```
kubectl proxy --address 0.0.0.0 --accept-hosts '.*'
```

### URL de acesso a aplicação
```
http://192.168.184.128:8001/api/v1/namespaces/default/services/http:demo-api-service:/proxy/time
```
