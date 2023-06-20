## 5.2. **Validação para bloquear tag latest**

Referência no [link](https://kyverno.io/policies/best-practices/disallow_latest_tag/disallow_latest_tag/).

### 5.2.1. **Arquivo de policy**

Crie o arquivo disallow_latest_tag.yaml:
```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: disallow-latest-tag
  annotations:
    policies.kyverno.io/title: Disallow Latest Tag
    policies.kyverno.io/category: Best Practices
    policies.kyverno.io/minversion: 1.6.0
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: Pod
    policies.kyverno.io/description: >-
      The ':latest' tag is mutable and can lead to unexpected errors if the
      image changes. A best practice is to use an immutable tag that maps to
      a specific version of an application Pod. This policy validates that the image
      specifies a tag and that it is not called `latest`.      
spec:
  validationFailureAction: Enforce
  background: true
  rules:
  - name: require-image-tag
    match:
      any:
      - resources:
          kinds:
          - Pod
    validate:
      message: "An image tag is required."
      pattern:
        spec:
          containers:
          - image: "*:*"
  - name: validate-image-tag
    match:
      any:
      - resources:
          kinds:
          - Pod
    validate:
      message: "Using a mutable image tag e.g. 'latest' is not allowed."
      pattern:
        spec:
          containers:
          - image: "!*:latest"
```

### 5.2.2. **Aplicar a policy**

```shell
kubectl apply -f disallow_latest_tag.yaml
```

Para listar as cluster policies, execute:

```shell
kubectl get cpol
```

### 5.2.3. **Teste**

Podemos testar de duas formas:
- Implantando via arquivo de manifesto um pod ou deployment
- Implantando via run ou create do kubectl

Teste via run:
```shell
kubectl run nginx --image nginx
```

ou

```shell
kubectl run nginx --image nginx:latest
```

O que aconteceu?


Se preferir, ao invés de executar o teste via kubectl run, o teste pode ser feito também via arquivo de manifesto:
nginx-teste.yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: nginx-demo
  name: nginx-demo
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx-demo
  template:
    metadata:
      labels:
        app: nginx-demo
    spec:
      containers:
      - image: nginx
        name: nginx
        ports:
        - containerPort: 80
```

Aplique o manifesto acima:
```shell
kubectl apply -f nginx-teste.yaml
```
O que aconteceu?

O que precisaria ser feito para que ao invés de bloquear a implantação do deployment com a tag latest, apenas fosse avisada ou auditada a violação da política?

[Principal](../README.md) - [Anterior](validacao-basica.md) - [Seguir](policy-tag-latest-cli.md)