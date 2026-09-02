### Namespace
```shell
kubectl create namespace il-automation
```
O tambien podemos ejecutar
```shell
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Namespace
metadata:
  name: il-automation
  labels:
    environment: developer
    project: observability
EOF
```
