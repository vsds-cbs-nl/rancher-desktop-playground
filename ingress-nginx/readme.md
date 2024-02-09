# ingress nginx

Disable Traefik in Rancher Desktop in Preferences > Kubernetes by unchecking Enable Traefix.

Install nginx as ingress with the lines below:

```bash
# navigate to the ingress-nginx folder
cd ingress-nginx
helm install ingress-nginx ./4.7.5 --values=overlays/localdev/values.yaml --namespace ingress-nginx --create-namespace
```

[back to index](../)