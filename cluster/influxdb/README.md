# InfluxDB

## Deploy

Create a folder on the target node at `/var/k8s/pvs/influxdb/`.

```
kubectl apply -f influxdb-storage.yaml
kubectl apply -f deployment.yaml

```

## Setup

Create the database

```
kubectl port-forward influxdb 8086
curl -i -XPOST 'http://localhost:8086/query' --data-urlencode "q=CREATE DATABASE greenhouse"
```
