## Requisitos
### Namespace
```shell
kubectl create namespace il-observability
```
O tambien podemos ejecutar
```shell
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Namespace
metadata:
  name: il-observability
  labels:
    environment: developer
    project: observability
EOF
```

### Credentials
Necesitaremos crear las credenciales para elastic
```shell
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: elastic-credentials
  namespace: il-observability
type: Opaque
stringData:
  ELASTIC_PASSWORD: "Password!"
EOF
```
