# Demo

## Cluster setup

Use Kind (Kubernetes in Docker) <https://kind.sigs.k8s.io/docs/user/quick-start/> to test on local

```bash
brew install kind
kind create cluster --config=kind-config.yaml
```

### Deploy app

```bash
# create a pod
kubectl apply -f pod.yaml
# create deployment
kubectl apply -f deployment.yaml
```

### Access app outside cluster

#### Service - ClusterIp

```bash
# create service with ClusterIp type
kubectl apply -f svc.yaml
kubectl port-forward service/foo-service 8080:8080
# test
curl http://localhost:8080/hostname
```

#### Service - NodePort

```bash
# create service with NodePort type
kubectl apply -f svc-nodeport.yaml
# test
curl http://localhost:30000/hostname
```

#### Service - Load Balancer

Expose service with LoadBalancer <https://kind.sigs.k8s.io/docs/user/loadbalancer/>

Install Cloud Provider KIND

```bash
go install sigs.k8s.io/cloud-provider-kind@latest
sudo install ~/go/bin/cloud-provider-kind /usr/local/bin
# MacOS to start cloud provider
sudo cloud-provider-kind
```

Create service and test

```bash
# create service with LoadBalancer type
kubectl apply -f svc-lb.yaml

# test
LB_IP=$(kubectl get svc/foo-service -o=jsonpath='{.status.loadBalancer.ingress[0].ip}')
curl http://${LB_IP}:8080/hostname
```

Reference: <https://github.com/kubernetes-sigs/cloud-provider-kind?tab=readme-ov-file#install>

#### Ingress

Install NGINX ingress controller

```bash
kubectl apply -f ingress-controller.yaml

# check ingress controller is created
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s

kubectl -n ingress-nginx get all
```

```bash
# create Ingress
kubectl apply -f ingress.yaml

# test
curl http://localhost/foo/hostname
```
