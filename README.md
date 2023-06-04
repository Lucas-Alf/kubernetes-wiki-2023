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

## Instalar os pacotes do Curl
```
sudo apt install -y apt-transport-https curl
```

## Instalar o pacote do Kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
```
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```
```
kubectl version
```
