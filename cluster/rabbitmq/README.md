# RabbitMQ

## Resources

 * `https://github.com/rabbitmq/rabbitmq-peer-discovery-k8s/tree/master/examples/k8s_statefulsets`

## Deploy

Create erlang cookie for rabbitmq cluster. (https://www.rabbitmq.com/clustering.html#erlang-cookie)

```
ERL_COOKIE=<random_32_char_alpha_num_string>
kubectl create secret generic rabbitmq --dry-run \
  --from-literal="erlang-cookie=$ERL_COOKIE" \
  -o yaml | kubectl apply -f -
```

Create remaining resources.
```
kubectl apply -f rabbitmq-rbac.yaml
kubectl apply -f rabbitmq-configmap.yaml
kubectl apply -f rabbitmq-storage.yaml
kubectl apply -f rabbitmq.yaml
```
