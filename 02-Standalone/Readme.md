## Requisitos
### Namespace
```shell
kubectl create namespace il-obserbavility
```

### Credentials
Necesitaremos crear las credenciales para elastic
```shell
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: elastic-credentials
  namespace: il-obserbavility
type: Opaque
stringData:
  ELASTIC_PASSWORD: "password"
EOF
```
