# Domain - Core Concepts

The code mentioned in this document are used in the Certified Kubernetes Security Specialist 2023 video course.


# Video-Document Mapper

| Sr No | Document Link |
| ------ | ------ |
| 1 | [Creating User for RBAC](1-user-rbac.md) |
| 2 | [Role Based Access Controls (RBAC)](2-rbac.md) |
| 3 | [ClusterRole and ClusterRoleBinding](3-clusterrole.md)
| 4 | [Creating Ingress and Ingress Controller](4-deploying-ingress.md)
| 4 | [Ingress Security](4-ingress-security.md) |
| 5 | [Overview of Service Accounts](5-service-account.md) |
| 6 | [Service Accounts - Points to Note](6-sa-pointers.md) |
| 7 | [Service Account Security](7-sa-security.md) |
| 8 | [Upgrading kudeadm Clusters](8-kubeadm-version.md) |

# RBAC
Options to get a user to authenticate:
* create a private key for john and a CSR, sign with CA cert and key
* create a private key for john and a CSR, approve the CSR and get cert. Better one
	* Then export that into a crt and use it to create a kube context for a new user 
# Role and Role Binding
* Role - contains rules that represent permissions
* Role Binding - grands permissions in the role to a user or set of users
# Cluster scope
Role and Rolebinding are only namespace scoped
* Cluster Role - grant same permissions as role, but to cluster
# Combination
* Role-Rolebinding - good
* ClusterRole-ClusterRolebinding - good
* ClusterRole-Rolebinding - good
* Role - ClusterRoleBinding - not a thing
# Ingress
* want to add a L7 gateway so that we can route to multiple services from one LB. 
* Can do laod balancing, ssl termination, name based routing
## Resource
* The rules 
## Controllers
* Handles proxy to implement rules
* Rules/Resource has no effect without controller 

Once it's all set up locally:
ingress-controller nodeport service -> ingress-controller pod -> pod (based on ingress resource)

## Ingress Security
Controller automatically adds HTTPS. Can add a cert as shown and then include it in the yaml for the ingress. 

# Service Accounts
Each NS has one at least that gets put onto nodes. Seen in mounts in pod spec.  
Can assign other ones and give them permissions. 
The default gets only api discovery permission.
Can edit that permissions list or opt out from the mounting at the service account level or the pod level.

