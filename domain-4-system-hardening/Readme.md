# Domain - Core Concepts

The code mentioned in this document are used in the Certified Kubernetes Security Specialist 2023 video course.

# Video-Document Mapper

| Sr No | Document Link |
| ------ | ------ |
| 1 | [Overview of AppArmor](apparmor.md) |
| 2 | [Integration of AppArmor with Kubernetes](apparmor-k8s.md) |
| 3 | [Configuring containerd and runc](oci.md)
| 4 | [Container Runtime Interface](kubeadm-containerd.md)
| 5 | [Implementing RunTimeClass - gVisor](gvisor.md) |
| 6 | [Kubeadm and Calcio](kubeadm-calico.md) |
| 7 | [Understanding Network Policies - Part 01](netpol.md) |
| 8 | [Understanding Network Policies - Part 02](netpol-02.md) |


# AppArmor
DAC shortcomings:
* app of user has everything that user has 

MAC: 
* set by admin
* confined and not confined processes 
	* confined - not trusted, explicitly list things they can do
	* not confined - more trusted

Apparmor can enforce specific lists based on what it observes from a trial run. 
aa-genprof
* enforce - don't let bad things happen
* complain - logged but allowed. 
## AA in K8s
profiles specified per container based on annotations
