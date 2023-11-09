
#### Step 1 - Create Nginx Pod:
```sh
kubectl run example-pod --image=nginx -l=app=nginx
```
#### Step 2 - Create Service:
```sh
kubectl expose pod example-pod --port=80 --target-port=80 --name=example-svc
```
#### Step 3 - Installing Ingress Controller:
```sh
#kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.41.2/deploy/static/provider/baremetal/deploy.yaml
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.2/deploy/static/provider/baremetal/deploy.yaml
# ended up using helm, but not sure if it was just the ingress class annotation that I was always missing. 
helm upgrade --install ingress-nginx ingress-nginx \\n  --repo https://kubernetes.github.io/ingress-nginx \\n  --namespace ingress-nginx --create-namespace
```
#### Step 4 - Create Ingress:
```sh
nano ingress.yaml
```
```sh
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    kubernetes.io/ingress.class: nginx
spec:
  rules:
  - host: example.internal
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: example-svc
            port:
              number: 80
```
```sh
kubectl apply -f ingress.yaml
```
#### Step 5 - Verification:
```sh
kubectl get svc -n ingress-nginx
ifconfig eth0
```
Add appropriate host entry accordingly.
```sh
nano /etc/hosts
```
```sh
curl http://example.internal:30429
```
