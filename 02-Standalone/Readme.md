## Requisitos

### Desplegar ECK
1. Actualizar los CRDs del operador para dar soporte a la rama 9.x
kubectl create -f https://download.elastic.co/downloads/eck/3.4.1/crds.yaml

2. Actualizar el despliegue del operador en el namespace elastic-system
kubectl apply -f https://download.elastic.co/downloads/eck/3.4.1/operator.yaml

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

### Validar
Validar elasticSearch
```shell
kubectl exec -n il-observability elasticsearch-es-default-0 -- curl -s -k -u "elastic:$(kubectl get secret elasticsearch-es-elastic-user -n il-observability -o jsonpath='{.data.elastic}' | base64 -d)" https://localhost:9200/_cluster/health?pretty
```

```shell
kubectl exec -n il-observability deployments/kibana-kb -- curl -s -k -H "kibana-xsrf: true" -u "elastic:$(kubectl get secret elasticsearch-es-elastic-user -n il-observability -o jsonpath='{.data.elastic}' | base64 -d)" https://localhost:5601/api/status
```


