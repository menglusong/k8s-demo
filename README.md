# Demo

## Cluster setup

Use Kind (Kubernetes in Docker) <https://kind.sigs.k8s.io/docs/user/quick-start/> to test on local

```bash
brew install kind
kind create cluster --config=kind-config.yaml
```

### ClusterIp

```bash
kubectl port-forward service/foo-service 8080:80
```

### Load Balancer

Expose service with LoadBalancer <https://kind.sigs.k8s.io/docs/user/loadbalancer/>

Install Cloud Provider KIND

```bash
go install sigs.k8s.io/cloud-provider-kind@latest
sudo install ~/go/bin/cloud-provider-kind /usr/local/bin
# MacOS
sudo cloud-provider-kind
```

Reference: <https://github.com/kubernetes-sigs/cloud-provider-kind?tab=readme-ov-file#install>

### Ingress

Install NGINX ingress controller

```bash
kubectl apply -f ingress-controller.yaml

kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s

kubectl -n ingress-nginx get all
```
