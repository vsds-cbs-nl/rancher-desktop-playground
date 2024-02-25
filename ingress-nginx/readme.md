# ingress nginx

Disable Traefik in Rancher Desktop in Preferences > Kubernetes by unchecking Enable Traefix.

First create a certificate to use for https connections. Create the tls secret with the lines below in the default namespace. The first line creates a new certificate, the second line creates a secret with the generated certificate (server.key and server.crt) and the last line adds the certificate to the Trusted Root Certification Authories so it is trusted by the browser.

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:4096 -keyout server.key -out server.crt -subj "/CN=*.localdev.me" -addext "subjectAltName = DNS:*.localdev.me"
kubectl create secret tls ingress-tls --key server.key --cert server.crt -n default
certutil -addstore root server.crt
```

Install nginx as ingress with the lines below:

```bash
# navigate to the ingress-nginx folder
cd ingress-nginx
helm install ingress-nginx ./4.7.5 --values=overlays/localdev/values.yaml --namespace ingress-nginx --create-namespace
```

[back to index](../)