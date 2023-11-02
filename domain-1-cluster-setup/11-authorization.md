
#### Step 1 - Enable AlwaysDeny Authorization Mode

```sh
nano /etc/systemd/system/kube-apiserver.service

--authorization-mode=AlwaysDeny 

systemctl daemon-reload
systemctl restart kube-apiserver

kubectl get secret --server=https://127.0.0.1:6443 --client-certificate /root/certificates/alice.crt --certificate-authority /root/certificates/ca.crt --client-key /root/certificates/alice.key
# Forbidden, secrets is forbidden
```
#### Step 2 - Enable RBAC Authorization Mode

```sh
nano /etc/systemd/system/kube-apiserver.service

--authorization-mode=RBAC

systemctl daemon-reload
systemctl restart kube-apiserver

kubectl get secret --server=https://127.0.0.1:6443 --client-certificate /root/certificates/alice.crt --certificate-authority /root/certificates/ca.crt --client-key /root/certificates/alice.key
# User "alice" cannot list resource "secrets"
```
#### Step 3 - Create Super User

```sh
cd /root/certificates
openssl genrsa -out bob.key 2048
openssl req -new -key bob.key -subj "/CN=bob/O=system:masters" -out bob.csr
openssl x509 -req -in bob.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out bob.crt -extensions v3_req  -days 1000
```
#### Step 4 Verification:

```sh
kubectl get secret --server=https://127.0.0.1:6443 --client-certificate /root/certificates/bob.crt --certificate-authority /root/certificates/ca.crt --client-key /root/certificates/bob.key
# No resources found
```