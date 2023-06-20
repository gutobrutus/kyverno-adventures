## 5.1. **Validação básica em criação de namespace**

Para ilustrar, um exemplo simples, que está na documentação [oficial](https://kyverno.io/docs/writing-policies/validate/). Foi realizada uma pequena adaptação no exemplo, ao invés de ter uma label com um valor pré-determinado, apenas deve ter a label.

### 5.1.1. **Criar o arquivo da policy**

Crie um arquivo namespace-label-purpose.yaml:
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
            purpose: "?*"
```

Com a cluster policy acima, sempre que for criado um namespace no cluster, este deve ter uma label "purpose".

### 5.1.2. **Aplicar a policy**

```shell
kubectl apply -f namespace-label-purpose.yaml
```

Para listar e confirmar a criação da cluster policy, pode-se exeutar:

```shell
kubectl get cpol
```

Na saída do comando acima, deverá existir na lista de policies a policy criada, dentre outras:

```shell
NAME                             BACKGROUND   VALIDATE ACTION   READY   AGE     MESSAGE
require-ns-purpose-label         true         Enforce           True    6s      Ready
```
Mais informações sobre o campo BACKGROUND [aqui](https://kyverno.io/docs/writing-policies/policy-settings/) e [aqui](https://kyverno.io/docs/policy-reports/background/).

### 5.1.3. **Teste**

Para testar a política aplicada no cluster, crie um arquivo de manifesto namespace-label.yaml:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: prod-bus-app1
  labels:
    purpose: prod
```

Agora aplique o manifesto criado:

```shell
kubectl apply -f namespace-label.yaml
```
O namespace prod-bus-app1 foi criado, pois passou na validação

Agora crie um outro arquivo de manifesto namespace.yaml:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: prod-bus-app2
```

Aplique o arquivo anterior:
```shell
kubectl apply -f namespace.yaml
```

O que aconteceu?

***O que precisaria ser feito para que ao invés de bloquear a criação do namespace, apenas fosse avisada ou auditada a violação da política?***

[Principal](../README.md) - [Anterior](instalacao-kyverno-cli.md) - [Seguir](policy-tag-latest.md)