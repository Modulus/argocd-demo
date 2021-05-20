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
kubectl create ns demo
kubectl create ns monitoring

kubectl apply -f argo/projects.yaml

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
kubectl apply -f argo/demo.yaml
argocd app list
argocd app sync application




# TODO: 
[ x ] - Create demo for projects in argo
[ ] - Create demo for encrypted secrets


## Rollouts
kubectl create namespace argo-rollouts
kubectl apply -n argo-rollouts -f https://raw.githubusercontent.com/argoproj/argo-rollouts/stable/manifests/install.yaml

https://argoproj.github.io/argo-rollouts/

brew install argoproj/tap/kubectl-argo-rollouts
