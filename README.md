# argo-istio-integration

# Overview
* Install mkcert and create a certificate
* Install Istio
* Install cert-manager and configure it
* Install ArgoCD and configure it
* Configure Istio

# Mkcert

Check https://github.com/FiloSottile/mkcert#macos

**Summary:**
```
brew install mkcert
mkcert -install
mkcert argocd-local.mydomain.com localhost 127.0.0.1 ::1
```

> Don't forget to change **mydomain.com** to another one!

# Istio Install

Check https://istio.io/latest/docs/setup/getting-started/#install

**Summary:**
```
brew install istioctl
istioctl install --set profile=demo -y
```

# Cert-manager

Check https://cert-manager.io/docs/installation/#default-static-install

**Summary:**
```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.11.0/cert-manager.yaml
kubectl -n cert-manager create secret tls mydomain.com-tls-secret --key=~/Library/Application\ Support/mkcert/rootCA-key.pem --cert=~/Library/Application\ Support/mkcert/rootCA.pem
kubectl apply -f ca-mkcert.yml -f certificate-mkcert.yml
```

> Don't forget to change **mydomain.com** to another one in the command-line argument and files!


# ArgoCD

Check https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd

**Summary:**
```
kubectl create namespace argocd
kubectl label namespace argocd istio-injection=enabled
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl apply -f ConfigMap.yaml
```

# Istio Configure
```
kubectl apply -f Services.yaml -f Gateway.yaml -f VirtualService.yaml
```
