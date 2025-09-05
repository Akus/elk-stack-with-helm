# elk-stack-with-helm
```bash
kubectl create namespace elk

helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm install elasticsearch bitnami/elasticsearch --namespace elk -f your-limits.yaml
kubectl get pods --namespace elk -l app.kubernetes.io/name=elasticsearch
helm install kibana bitnami/kibana \
  --namespace elk \
  --set elasticsearch.host=elasticsearch \
  --set elasticsearch.port=9200
kubectl port-forward svc/kibana 5601:5601

kubectl create secret generic elasticsearch-password \
  --namespace elk \
  --from-literal=password=<YOUR_PASSWORD>

helm install logstash bitnami/logstash -f logstash_helm_values.yaml --namespace elk \
  --namespace elk \
  --set elasticsearch.host=elasticsearch \
  --set elasticsearch.port=9200

kubectl get pods --namespace elk -l app.kubernetes.io/name=logstash
kubectl port-forward svc/elasticsearch 9200:9200
kubectl exec -it <logstash-pod-name> -- /bin/bash
# Visit: http://localhost:5601
# Visit: http://localhost:9200
```
