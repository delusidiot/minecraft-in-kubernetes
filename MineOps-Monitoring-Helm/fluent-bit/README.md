# Fluent-Bit

## Create RBAC resources for Fluent Bit

### Create ServiceAccount
```shell
kubectl create -f ./fluent-bit/fluent-bit-service-account.yaml
```
### Create ClusterRole
```shell
kubectl create -f ./fluent-bit/fluent-bit-role.yaml
```
### Create ClusterRoleBinding
```shell
kubectl create -f ./fluent-bit/fluent-bit-role-binding.yaml
```

## Create Fluent Bit ConfigMap

### Create Configmap
```shell
kubectl create -f ./fluent-bit/fluent-bit-configmap.yaml
```


## Fluent Bit Daemonset
```shell
kubectl create -f ./fluent-bit/fluent-bit-daemonset.yaml
```


## create all
```shell
kubectl create -f ./fluent-bit/fluent-bit-service-account.yaml
kubectl create -f ./fluent-bit/fluent-bit-role.yaml
kubectl create -f ./fluent-bit/fluent-bit-role-binding.yaml
kubectl create -f ./fluent-bit/fluent-bit-configmap.yaml
kubectl create -f ./fluent-bit/fluent-bit-daemonset.yaml
```
## Delete all

```shell
kubectl delete -f ./fluent-bit/fluent-bit-service-account.yaml
kubectl delete -f ./fluent-bit/fluent-bit-role.yaml
kubectl delete -f ./fluent-bit/fluent-bit-role-binding.yaml
kubectl delete -f ./fluent-bit/fluent-bit-configmap.yaml
kubectl delete -f ./fluent-bit/fluent-bit-daemonset.yaml
```