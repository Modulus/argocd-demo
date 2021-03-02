# Argocd tooling demo

# Prerequisites
```
kind create cluster --config kind-config.yaml --name argo-cluster
```
Or start a minikube cluster

```
minikube start
```

# Install argocd
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Install cli 
brew install argocd


# Create loadBalancer from the service (Optional)
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'


# Port forwarding and access to the gui
kubectl port-forward svc/argocd-server -n argocd 8080:443

argocd login localhost:8080 

Password is name of argocd-server pod

kubectl get po -n argocd  #optional


argocd account update-password

# Deploy argo application
kubectl apply -f argo/godemo.yaml


# Argocd commands

argocd app list
argocd app sync go-metrics

# Create project
argocd proj create core
argocd proj create demo

# Create project from file
kubectl apply -f argo/projects


# Deploy prometheus operator
kubectl apply -f argo/prometheus-operator.yaml

argocd app list
argocd app sync prometheus-operator

# Deploy kube-prometheus stack
kubectl apply -f argo/kube-prometheus.yaml
argocd app list
argocd app sync kube-prometheus