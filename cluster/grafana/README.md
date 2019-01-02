# Grafana

## Resources

 * https://github.com/carlosedp/prometheus-operator-ARM

## Deploy

Create folder and set the `nodeAffinity` correctly in `grafana-storage.yaml`.

```
sudo mkdir -p /var/k8s/pvs/grafana/
sudo chown -R 65534 /var/k8s/pvs/grafana/
```

Create configuration and deploy grafana.

```
kubectl apply -f grafana-storage.yaml
kubectl apply -f grafana-datasources.yaml
kubectl apply -f grafana-dashboard-definitions.yaml
kubectl apply -f grafana-dashboards.yaml

kubectl create secret generic --dry-run grafana-credentials \
  --from-literal=user=<username> \
  --from-literal=password=<password> \
  -o yaml | kubectl apply -n monitoring -f -

kubectl apply -f grafana-deployment.yaml
kubectl apply -f grafana-service.yaml
```
