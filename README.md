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

1. [Pré-requisitos](doc-pages/pre-requisitos.md)
2. [Instalação do Kyverno](doc-pages/instalacao-kyverno.md)
3. [Pod Security Standard Policies](doc-pages/pod-security-standard-policies.md)  
4. [Instalação - Kyverno CLI](doc-pages/instalacao-kyverno-cli.md)  
5. Políticas de Validação (validate)  
5.1. [Validação Básica](doc-pages/validacao-basica.md)  
5.2. [Política para validar tag latest](doc-pages/policy-tag-latest.md)  
5.3. [Validar política tag latest via CLI](doc-pages/policy-tag-latest-cli.md)  
6. Políticas de Mudanças (mutate)  
6.1. [Adição de label em Namespaces existentes](doc-pages/add-label-namespaces.md)
