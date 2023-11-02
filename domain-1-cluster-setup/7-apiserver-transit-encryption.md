
#### Step 1 - Verify Certificate Details
```sh
openssl s_client -showcerts -connect localhost:6443 2>/dev/null | openssl x509 -inform pem -noout -text
```

#### Step 2 - Generate Configuration File for CSR Creation:
```sh
cd /root/certificates
```
Replace the IP.2 with your IP
```sh
cat <<EOF | sudo tee api.conf
[req]
req_extensions = v3_req
distinguished_name = req_distinguished_name
[req_distinguished_name]
[ v3_req ]
basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
subjectAltName = @alt_names
[alt_names]
DNS.1 = kubernetes
DNS.2 = kubernetes.default
DNS.3 = kubernetes.default.svc
DNS.4 = kubernetes.default.svc.cluster.local
IP.1 = 127.0.0.1
IP.2 = 143.110.250.107
IP.3 = 10.0.0.1
EOF
```
#### Step 3 - Generate Certificates for API Server
Default ones are created if we don't use them. 
```sh
openssl genrsa -out kube-api.key 2048
openssl req -new -key kube-api.key -subj "/CN=kube-apiserver" -out kube-api.csr -config api.conf
openssl x509 -req -in kube-api.csr -CA ca.crt -CAkey ca.key -CAcreateserial  -out kube-api.crt -extensions v3_req -extfile api.conf -days 1000
```

#### Step 4 - Start kube-apiserver with following flags:
```sh
nano /etc/systemd/system/kube-apiserver.service
```
```sh
--tls-cert-file=/root/certificates/kube-api.crt
--tls-private-key-file=/root/certificates/kube-api.key
```
```sh
systemctl daemon-reload
systemctl restart kube-apiserver
```

Final command now:
```sh
/usr/local/bin/kube-apiserver \
  --advertise-address=10.0.2.15 
  --etcd-cafile=/root/certificates/ca.crt 
  --etcd-certfile=/root/certificates/apiserver.crt 
  --etcd-keyfile=/root/certificates/apiserver.key 
  --etcd-servers=https://127.0.0.1:2379 
  --service-account-key-file=/root/certificates/service-account.crt 
  --service-cluster-ip-range=10.0.0.0/24 
  --service-account-signing-key-file=/root/certificates/service-account.key 
  --service-account-issuer=https://127.0.0.1:6443 
  --tls-cert-file=/root/certificates/kube-api.crt 
  --tls-private-key-file=/root/certificates/kube-api.key
```
#### Step 5 - Verification
```sh
# output the certs that the server shows
openssl s_client -showcerts -connect localhost:6443 2>/dev/null | openssl x509 -inform pem -noout -text

curl -k https://localhost:6443
```
