#### See Taint Details of Worker node

kubectl get nodes
kubectl describe node kubeadm-master

#### Run Nginx POD with current Taint

```sh
kubectl run nginx-pod --image=nginx
kubectl describe pod nginx-pod
#### Remove Taint from the Worker Node:

kubectl taint node pat-virtualbox node-role.kubernetes.io/master-
kubectl taint node pat-virtualbox node-role.kubernetes.io/control-plane-
```

#### Verification:
```sh
kubectl run nginx-pod --image=nginx
kubectl get pods
```
