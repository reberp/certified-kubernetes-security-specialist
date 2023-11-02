# Domain 1 - Cluster Setup

The code mentioned in this document are used in the Certified Kubernetes Security Specialist 2023 course.


# Video-Document Mapper

| Sr No | Document Link |
| ------ | ------ | 
| 1 | [Configuring ETCD Binaries](./1-install-etcd.md) |
| 2 | [Overview of Certificate Authority](./2-configure-ca.md) |
| 3 | [In-Transit Encryption with HTTPS](./3-etcd-transit-encryption.md) |
| 4 | [Client Authentication in ETCD](./4-etcd-client-auth.md) |
| 5 | [Integrating Systemd with ETCD](./5-etcd-systemd.md) |
| 6 | [Configuring API Server](./6-configure-apiserver.md) |
| 7 | [Transport Security for API Server](./7-apiserver-transit-encryption.md) |
| 8 | [Static Token Authentication](./8-token-authentication.md) |
| 9 | [Downsides - Static Token Authentication](./9-downside-token-auth.md) |
| 10 | [Implementing X509 Client Authentication](./10-certificate-auth-k8s.md) |
| 11 | [Authorization](./11-authorization.md) |
| 12 | [Encryption Providers](./12-encryption-provider.md)|
| 13 | [Implementing Auditing](./13-audit-logs.md) |
| 14 | [Setting up kubeadm cluster](./14-kubeadm.md) |
| 15 | [Revising Taints and Tolerations](./15-taint.md) |
| 16 | [Kubelet Security](./16-kubelet-security.md) |
| 17 | [Verifying Platform Binaries](17-verify-binaries.md) |



# Access Control
Authentication -> authorization -> admission controllers -> object

### Authentication
https://kubernetes.io/docs/reference/access-authn-authz/authentication/
Static token, x509, OIDC
Token:
* Downsides of token: Tokens are cleartext on apiserver. Tokens can't be rotated without restarting server. 
509: as long as cert is signed by CA is trusted. 
* Downside of x509: private key might be insecure. Certificates live a long time. K8s doesn't support certificate revocation. Groups might need a new certificate, and then users need new certificate. Something else required to revoke certs. 
OpenID Connect Authentication 
* preferred or other external identity provider
* API Server has flags for OIDC
### Authorization
https://kubernetes.io/docs/reference/access-authn-authz/authorization/
Usually want RBAC. Can be Node, AlwaysDeny|Allow
RBAC
* through roles and rolebindings
* system:masters is an unrestricted group to everything. ```-subj "/CN=bob/O=system:masters"```
* CN is username, O is group. 
### Admission Controller
https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
Bunch of already available ones, and can be customized. 

# Encryption

# Audit
Different levels: None, Metadata, Request, RequestResponse

# Kubelet 
Want to secure like the apiserver
not AlwaysAllow, not anonymous auth etc. 
from /var/lib/kubelet/config.yaml has good practices mostly
```
apiVersion: kubelet.config.k8s.io/v1beta1
authentication:
  anonymous:
    enabled: false
  webhook:
    cacheTTL: 0s
    enabled: true
  x509:
    clientCAFile: /etc/kubernetes/pki/ca.crt
authorization:
...
```
Node Authorizer authorizes kubelets who are in the system:nodes group