## 5.3 **Validar a política Disallow Latest Tag usando a CLI**

Referência: [link](https://kyverno.io/docs/kyverno-cli/) 

Para ilustrar, serão validados os templates renderizados, a partir de um helm chart.

### 5.3.1. **Criar um chart helm (nginx default)**

```shell
helm create charts/site
```
Edite o arquivo Chart.yaml, alterando appVersion: "1.16.0" para appVersion: "latest".

### 5.3.2. **Gerar os templates com helm**

```shell
helm template charts/site > manifestos.yaml
```

### 5.3.3. **Executar o kyverno cli para validar**

```shell
kyverno apply disallow_latest_tag.yaml --resource manifestos.yaml
```

Os dois passos anteriores, podem ser executados em um único comando:

```shell
helm template charts/site | kyverno apply disallow_latest_tag.yaml --resource -
```

[Principal](../README.md) - [Anterior](policy-tag-latest.md) - [Seguir](pod-label-required.md.md)