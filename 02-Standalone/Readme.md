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

### Validar
Validar elasticSearch
```shell
kubectl exec -n il-observability elasticsearch-es-default-0 -- curl -s -k -u "elastic:$(kubectl get secret elasticsearch-es-elastic-user -n il-observability -o jsonpath='{.data.elastic}' | base64 -d)" https://localhost:9200/_cluster/health?pretty
```

```shell
kubectl exec -n il-observability deployments/kibana-kb -- curl -s -k -H "kibana-xsrf: true" -u "elastic:$(kubectl get secret elasticsearch-es-elastic-user -n il-observability -o jsonpath='{.data.elastic}' | base64 -d)" https://localhost:5601/api/status
```


