## 6.1 **Adição de label em Namespaces existentes**

Referência: [link](https://kyverno.io/policies/other/label-existing-namespaces/label-existing-namespaces/) 

Essa policy adiciona uma determinada label nos namespaces, menos em alguns namespaces, ou seja, há exceções. A parte da exceção foi adaptada aqui do que está na documentação oficial.

### 6.1.1. **Arquivo da Policy**

Crie o arquivo label-existing-namespace.yaml com o conteúdo abaixo:

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: label-existing-namespaces
  annotations:
    policies.kyverno.io/title: Label Existing Namespaces
    policies.kyverno.io/category: Other
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: Namespace
    kyverno.io/kyverno-version: 1.7.0
    policies.kyverno.io/minversion: 1.7.0
    kyverno.io/kubernetes-version: "1.23"
    policies.kyverno.io/description: >-
      Namespaces which preexist may need to be labeled after the fact and it is
      time consuming to identify which ones should be labeled and either doing so manually
      or with a scripted approach. This policy, which triggers on any AdmissionReview request
      to any Namespace, will result in applying the label `mykey=myvalue` to all existing
      Namespaces. If this policy is updated to change the desired label key or value, it will
      cause another mutation which updates all Namespaces.      
spec:
  mutateExistingOnPolicyUpdate: true
  rules:
  - name: label-existing-namespaces
    match:
      any:
      - resources:
          kinds:
          - Namespace
    exclude: 
      any:
      - resources:
          namespaces:
            - kube-system
            - kyverno
            - cattle-system
    mutate:
      targets:
        - apiVersion: v1
          kind: Namespace
      patchStrategicMerge:
        metadata:
          labels:
            cluster: kyverno
```

### 6.1.2. **Aplicar a policy**

```shell
kubectl apply -f label-existing-namespace.yaml
```

Para listar as cluster policies, execute:

```shell
kubectl get cpol
```

### 6.1.3. **Confirmando se a label foi adicionada**

```shell
kubectl edit ns NOME_DO_NAMESPACE
```

Procure pela label, se ela foi adicionada nos namespaces, menos nos namespaces kube-system, kyverno e cattle-system, em que a label não deve ter sido adicionada.

[Principal](../README.md) - [Anterior](policy-tag-latest-cli.md) - [Seguir]()