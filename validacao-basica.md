## 5. **Validação básica em criação de namespace**

Para ilustrar, um exemplo simples, que está na documentação [oficial](https://kyverno.io/docs/writing-policies/validate/).

### 5.1. **Criar o arquivo da policy**

Crie um arquivo namespace-label-prod.yaml:
```yaml
apiVersion: kyverno.io/v1
# The `ClusterPolicy` kind applies to the entire cluster.
kind: ClusterPolicy
metadata:
  name: require-ns-purpose-label
# The `spec` defines properties of the policy.
spec:
  # The `validationFailureAction` tells Kyverno if the resource being validated should be allowed but reported (`Audit`) or blocked (`Enforce`).
  validationFailureAction: Enforce
  # The `rules` is one or more rules which must be true.
  rules:
  - name: require-ns-purpose-label
    # The `match` statement sets the scope of what will be checked. In this case, it is any `Namespace` resource.
    match:
      any:
      - resources:
          kinds:
          - Namespace
    # The `validate` statement tries to positively check what is defined. If the statement, when compared with the requested resource, is true, it is allowed. If false, it is blocked.
    validate:
      # The `message` is what gets displayed to a user if this rule fails validation.
      message: "You must have label `purpose` with value `production` set on all new namespaces."
      # The `pattern` object defines what pattern will be checked in the resource. In this case, it is looking for `metadata.labels` with `purpose=production`.
      pattern:
        metadata:
          labels:
            purpose: production
```

### 5.2. **Aplicar a policy**

```shell
kubectl apply -f namespace-label-prod.yaml
```

### 5.3. **Teste**

Para testar a política aplicada no cluster, crie um arquivo de manifesto namespace-app1-com-label.yaml:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: prod-bus-app1
  labels:
    purpose: production
```

Agora aplique o manifesto criado:

```shell
kubectl apply -f namespace-app1-com-label.yaml
```
O namespace prod-bus-app1 foi criado, pois passou na validação

Agora crie um outro arquivo de manifesto namespace-app2-sem-label.yaml:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: prod-bus-app2
```

Aplique o arquivo anterior:
```shell
kubectl apply -f namespace-app2-sem-label.yaml
```

O que aconteceu?

***O que precisaria ser feito para que ao invés de bloquear a criação do namespace, apenas fosse avisada ou auditada a violação da política?***

[Principal](README.md) - [Anterior](instalacao-kyverno-cli.md) - [Seguir](policy-tag-latest.md)