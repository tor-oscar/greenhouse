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

## Additional information

### Slack Alerts

 1. Follow the instructions at
    `https://get.slack.help/hc/en-us/articles/115005265063-Incoming-WebHooks-for-Slack`
    to add an app and a webhook to your Slack workspace. Slack apps are listen
    at `https://api.slack.com/apps`.

 2. In the grana ui, use the bell icon on the left to add a `notification channel`.
    Set the type to `Slack` and set the webhook url you got from the the slack
    setup. The `Recipient`, `Mention` and `Token` fields are optional.
