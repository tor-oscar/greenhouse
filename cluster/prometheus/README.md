# Prometheus stack

## Resources

 * https://github.com/carlosedp/prometheus-operator-ARM

## Deploy

Add services `kube-controller-manager.yaml` and `kube-scheduler.yaml`.

```
kubectl apply -f prometheus-operator.yaml
```

Wait until it has created all its CRD.

```
kubectl apply -f node-exporter.yaml
kubectl apply -f node-exporter-service-monitor.yaml
kubectl apply -f prometheus-pv.yaml
kubectl apply -f prometheus.yaml
```
