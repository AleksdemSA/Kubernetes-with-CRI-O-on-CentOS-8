# Install Promtail, Loki and Grafana

Create NS
```
kubectl create namespace loki
```

Install PLG
```
helm repo add grafana https://grafana.github.io/helm-charts
helm upgrade --install loki grafana/loki-stack --namespace=loki --set grafana.enabled=true
```

Get password for admin
```
kubectl get secret loki-grafana --namespace=loki -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

And see Grafana in browser
```
kubectl port-forward --namespace loki service/loki-grafana 3000:80
```