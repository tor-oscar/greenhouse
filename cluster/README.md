# K8s cluster deployment

See:
 * https://blog.hypriot.com/post/setup-kubernetes-raspberry-pi-cluster/
 * https://gist.github.com/alexellis/fdbc90de7691a1b9edb545c17da2d975
 * https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/

## Setup

### Add k8s repository

```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo sh -c 'echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/kubernetes.list'
sudo apt-get update && sudo apt-get install kubeadm
```

### Disable swap

Required by K8s:

```
sudo dphys-swapfile swapoff && \
sudo dphys-swapfile uninstall && \
sudo update-rc.d dphys-swapfile remove
```

### Enable kernel features

Edit `/boot/cmdline.txt`. Add the following options to the end of the first line

```
cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory
```

### Reboot

```
sudo reboot
```

### Pull k8s images

```
sudo kubeadm config images pull -v3
```

### Initialize master node

On master node only:

```
sudo kubeadm init --pod-network-cidr 10.93.0.0/16
```

Copy the `kubeadm join` command and run it on worker nodes after initializing network.


### Create network

```
kubectl apply -f kubeadm-kuberouter-all-features-arm.yaml
kubectl delete ds -n kube-system kube-proxy

```

Om master node:

```
docker run --privileged -v /lib/modules:/lib/modules --net=host k8s.gcr.io/kube-proxy-arm:v1.13.1 kube-proxy --cleanup
```

### Join workers

Use the command from the `kubeadm init` command.

```
sudo kubeadm join 192.168.93.51:6443 --token <TOKEN> --discovery-token-ca-cert-hash <HASH>
```

## Misc

To run pods on the master node

```
tolerations:
  - effect: NoSchedule
    key: node-role.kubernetes.io/master
```
