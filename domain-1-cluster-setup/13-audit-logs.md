#### Reference Websites:

https://jsonformatter.curiousconcept.com/#

#### Step 1 - Create Sample Audit Policy File:
```sh
nano /root/certificates/logging.yaml

apiVersion: audit.k8s.io/v1
kind: Policy
rules:
- level: Metadata
```

#### Step 2 - Audit Configuration:
```sh
nano /etc/systemd/system/kube-apiserver.service
```
```sh
--audit-policy-file=/root/certificates/logging.yaml --audit-log-path=/var/log/api-audit.log --audit-log-maxage=30  --audit-log-maxbackup=10  --audit-log-maxsize=100 
```
```sh
systemctl daemon-reload
systemctl restart kube-apiserver
```
#### Step 3 Run Some Queries using Bob user

```sh
kubectl get secret --server=https://127.0.0.1:6443 --client-certificate /root/certificates/bob.crt --certificate-authority /root/certificates/ca.crt --client-key /root/certificates/bob.key
```
#### Step 4: Verification
```sh
cd /var/log
grep -i bob api-audit.log

{"kind":"Event","apiVersion":"audit.k8s.io/v1","level":"Metadata","auditID":"bf57a9a9-bbfd-43ef-a266-0397d5afd9cb","stage":"RequestReceived","requestURI":"/api/v1/namespaces/default/secrets?limit=500","verb":"list","user":{"username":"bob","groups":["system:masters","system:authenticated"]},"sourceIPs":["127.0.0.1"],"userAgent":"kubectl/v1.24.2 (linux/amd64) kubernetes/f66044f","objectRef":{"resource":"secrets","namespace":"default","apiVersion":"v1"},"requestReceivedTimestamp":"2023-11-02T14:15:49.382369Z","stageTimestamp":"2023-11-02T14:15:49.382369Z"}
{"kind":"Event","apiVersion":"audit.k8s.io/v1","level":"Metadata","auditID":"bf57a9a9-bbfd-43ef-a266-0397d5afd9cb","stage":"ResponseComplete","requestURI":"/api/v1/namespaces/default/secrets?limit=500","verb":"list","user":{"username":"bob","groups":["system:masters","system:authenticated"]},"sourceIPs":["127.0.0.1"],"userAgent":"kubectl/v1.24.2 (linux/amd64) kubernetes/f66044f","objectRef":{"resource":"secrets","namespace":"default","apiVersion":"v1"},"responseStatus":{"metadata":{},"code":200},"requestReceivedTimestamp":"2023-11-02T14:15:49.382369Z","stageTimestamp":"2023-11-02T14:15:49.383965Z","annotations":{"authorization.k8s.io/decision":"allow","authorization.k8s.io/reason":""}}
```
