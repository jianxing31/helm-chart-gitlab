# Host gitlab in kubernetes with helm chart
## About
This repo is a demo for deploying gitlab in k8s with helm/helmfile.

## Getting Started
### 1. requirements
- install [helm](https://helm.sh/docs/intro/install/) and [helmfile](https://github.com/helmfile/helmfile)
- install helm-diff  
  Depending on the OS used, you may face some issues in this step. Everything works fine in my macos, but for my Windows terminal, I faced `helm diff not found error` and [this answer fixed my problem](https://github.com/databus23/helm-diff/issues/316#issuecomment-1814806292).
```shell
helm plugin add https://github.com/databus23/helm-diff
```
- k8s environment   
  Set up cloud environment like eks or local environment like minikube

### 2. deploy with helmfile
```shell
helmfile apply
```
or with [helm command](https://docs.gitlab.com/charts/installation/deployment.html#deploy-using-helm)
```shell
helm repo add gitlab https://charts.gitlab.io/
helm repo update
helm upgrade --install gitlab gitlab/gitlab \
  --timeout 600s \
  --set global.hosts.domain=example.com \
  --set global.hosts.externalIP=10.10.10.10 \
  --set certmanager-issuer.email=me@example.com \
  --set postgresql.image.tag=13.6.0
```

### 3. login to gitlab
- get initial password
```shell
  kubectl get secret gitlab-gitlab-initial-root-password -n gitlab -ojsonpath='{.data.password}' | base64 --decode ; echo
```
- login to our local domain: gitlab.mygitlab.com in this demo

## Reference
1. [Deploy the GitLab Helm chart](https://docs.gitlab.com/charts/installation/deployment.html#deploy-using-helm)
2. [Gitlab helm char repo](https://gitlab.com/gitlab-org/charts/gitlab/-/tree/master?ref_type=heads)
