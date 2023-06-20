## 5.4. **Validar existência de label em Pod**

Nesse exemplo, o cenário é que todo Pod que for implantado no namespace prod ou production, deverá ter obrigatoriamente uma label scope com um padrão de nome iniciando com "prod-".

### 5.4.1. **Arquivo da policy**

Crie o arquivo pod-prod-required-label.yaml com o conteúdo abaixo:

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: pod-prod-required-label
spec:
  validationFailureAction: Enforce
  rules:
  - name: pod-prod-check-label
    match:
      resources:
        kinds:
        - Pod
        operations:
        - CREATE
        - UPDATE
        namespaceSelector:
          matchExpressions:
            - key: scope 
              operator: In
              values: 
              - prod
              - production
    validate:
      message: "The label with key `scope` prefixed with `prod-` is required."
      pattern:
        matadata:
          labels:
            scope: "prod-*"
```

### 5.4.2. **Aplicar a policy**

```shell
kubectl apply -f disallow_latest_tag.yaml
```

Para listar as cluster policies, execute:

```shell
kubectl get cpol
```

### 5.4.3. **Teste**

Caso o namespace **prod** ainda não exista, crie-o com o comando abaixo:

```shell
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Namespace
metadata:
  name: prod
  labels:
    purpose: prod
EOF
```

Com o namespace criado, pode-se testar a efetividade da cluster policy através do próprio kubectl com:

```shell
kubectl run nginx --image=nginx --namespace prod
```

Qual foi o resultado?

A cluster policy validou e apresentou o erro, não permitindo a implantação do deployment e consequentemente o pod sem a label scope com o padrão definido na policy.

Se foi criada a [policy disallow latest tag](policy-tag-latest.md), também falhará nessa policy.

Executando da forma que passe pela cluster policy:

```shell
kubectl run nginx --image=nginx:1.25.1-alpine --namespace prod --labels scope=prod-nginx
```

[Principal](README.md) - [Anterior](instalacao-kyverno.md) - [Seguir](pod-security.md)