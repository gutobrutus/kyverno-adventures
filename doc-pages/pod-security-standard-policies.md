## 3. **Instalação - Kyverno Pod Security Standard Policies**

Consiste em uma série de políticas baseadas nas definições de segurança de Pod do próprio Kubernetes. Essa instalação é recomendada.

Instalar via helm:
```shell
helm install kyverno-policies kyverno/kyverno-policies -n kyverno
```

[Principal](README.md) - [Anterior](instalacao-kyverno.md) - [Seguir](instalacao-kyverno-cli.md)
