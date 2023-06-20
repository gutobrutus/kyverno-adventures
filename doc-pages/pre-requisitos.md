## 1. **Instalação dos pacotes - pré-requisitos**

Na VM/Host Linux - Bastion Host, instalação do helm e kubectl descrita abaixo, considerando um sistema operacional Ubuntu.

### 1.1. **Instalação do Helm**

Download:
```shell
curl https://get.helm.sh/helm-v3.11.3-linux-amd64.tar.gz -o helm.tar.gz
```
Descompactar:
```shell
tar -xvzf helm.tar.gz
```
Mover o binário:
```shell
mv linux-amd64/helm /usr/local/bin/helm
```
Verificando a versão:
```shell
helm version
```

### 1.2. **Instalação do kubectl**

Download:
```shell
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
Adicionando permissão de execução:
```shell
chmod +x kubectl
```
Mover o binário:
```shell
mv kubectl /usr/local/bin/
```
Verificando a versão:
```shell
kubectl version
```

### 1.3. **Definindo variável KUBECONFIG**

Para acesso ao cluster, considerando um cluster que é gerenciado no Rancher, basta copiar ou realizar download da configuração do cluster e colocar em um arquivo .yaml.

Considerando que o arquivo salvo no bastion host tem o nome kubeconfig.yaml, para definir a variável:

```shell
export KUBECONFIG=$(pwd)/kubeconfig.yaml
```
O comando acima deve ser executado dentro do diretório que está o arquivo kubeconfig.yaml.

[Principal](../README.md) - [Seguir](instalacao-kyverno.md)