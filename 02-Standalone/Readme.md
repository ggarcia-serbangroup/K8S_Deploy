## Requisitos
Necesitaremos crear las credenciales para elastic
```shell
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: elastic-credentials
  namespace: default
type: Opaque
stringData:
  ELASTIC_PASSWORD: "password"
EOF
```
