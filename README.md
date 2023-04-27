# **Prática com Kyverno**

Aqui são mostrados os passos para a parte prática da aula introdutória sobre o [Kyverno](https://kyverno.io/).

Para realizar a prática com a utilização do Kyverno, deve-se atender os seguintes pré-requisitos:
- VM/Host Linux para servir como host de acesso (bastion host).
- Instalação dos pacotes abaixo:
  - helm
  - kubectl
- Um cluster kubernetes
- Configurar variável KUBECONFIG para acesso ao cluster, a partir do arquivo de configuração yml.

## **Sumário**

### [1. Pré-requisitos](pre-requisitos.md)
### [2. Instalação do Kyverno](instalacao-kyverno.md)
### [3. Pod Security Standard Policies](pod-security.md)
### [4. Instalação - Kyverno CLI](instalacao-kyverno-cli.md)
### [5. Validação Básica](validacao-basica.md)
### [6. Política para validar tag latest](policy-tag-latest.md)
### [7. Validar política tag latest via CLI](policy-tag-latest-cli.md)
