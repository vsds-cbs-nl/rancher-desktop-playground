# argocd

Installation files are from <https://github.com/argoproj/argo-cd/releases>

## Install

Install argocd with these lines:

```bash
# navigate to the argocd/overlays/localdev folder
cd argocd/overlays/localdev
kubectl apply -k .
```

To get the initial admin password:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

## Setup

1. create access token in github (see https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)
1. browse to https://argocd.localdev.me
1. click Advanced and the link Continue to argo.localdev.me
1. login with username _admin_ and the initial admin password (see Install section)
1. go to settings > repositories > connect repo
    - use Default for the project
    - use https://github.com/<YOUR_NAME_HERE>/rancher-desktop-playground.git for repository url and replace <YOUR_NAME_HERE> with your github account
    - use git for the username
    - use the access token for the password
1. edit the environments/localdev/app-of-apps.yaml git urls - look for <YOUR_NAME_HERE>
1. edit the bootstrap/localdev/*.yaml git urls - look for <YOUR_NAME_HERE>
1. commit and push these changes
1. add localdev.yaml as app-of-apps to argocd
    - new app
    - application name: localdev
    - project name: default
    - repository url: the repository you connected before
    - revision: HEAD
    - path: environments/localdev
    - cluster url: https://kubernetes.default.svc
    - namespace: argocd
1. click sync on the app to initialize the sync - else the state remains _Missing_

You'll end up with this
![argocd with 5 apps](../assets/argocd01.jpg)

## Upgrade

After installation you can upgrade argocd with argocd. Just edit the overlays/localdev/kustomization.yaml to use the new argocd installation files. Here is an example how to upgrade from 2.9.3 to 2.9.6:

### kustomization.yaml

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
namespace: argocd

resources:
- ../../2.9.6 # <- changed this to the new version
- ingress-http.yaml
- ingress-grpc.yaml

patches:
# ... more ...
```

[back to index](../)