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

Password is name of argocd-server pod in argocd 1.8

kubectl get po -n argocd  #optional

In argocd 1.9+ there is a password

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d



argocd account update-password

# 1.Default project examples

kubectl apply -f argo/projects.yaml


## Deploy prometheus operator
kubectl apply -f argo-default/prometheus-operator.yaml

argocd app list
argocd app sync prometheus-operator

## Deploy kube-prometheus stack
kubectl apply -f argo-default/kube-prometheus.yaml
argocd app list
argocd app sync kube-prometheus

## Deploy demo app
kubectl apply -f argo-default/godemo.yaml
argocd app list
argocd app sync go-metrics

# 2.New project examples

## Create projects from file
kubectl apply -f argo/projects.yaml

## Argocd project commands
argocd proj list

## Deploy prometheus operator
kubectl apply -f argo/prometheus-operator.yaml

argocd app list
argocd app sync prometheus-operator

## Deploy kube-prometheus stack
kubectl apply -f argo/kube-prometheus.yaml
argocd app list
argocd app sync kube-prometheus

## Deploy demo app
kubectl apply -f argo/godemo.yaml
argocd app list
argocd app sync go-metrics


# TODO: 
[ x ] - Create demo for projects in argo
[ ] - Create demo for encrypted secrets