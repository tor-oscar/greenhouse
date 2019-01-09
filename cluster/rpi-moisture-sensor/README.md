# RPI moisture sensor

Deployed as a daemonset, running on each node with a rpi-moisture-sensor.
The configuration is node specific, allowing non-identical setups on each node.

# Deploy

Edit `sensors-configmap.yaml` to reflect how your moisture sensor is connected.
Label each node with a rpi-moisture-sensor, eg `kubectl label node gh03 rpi-sensors=soil_moisture`.

```
kubectl apply -f sensors-configmap.yaml
kubectl apply -f deployment.yaml
```
