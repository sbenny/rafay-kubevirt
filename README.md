# rafay-kubevirt
Rafay recipes

# VMs on Kubernetes
## Introduction to KubeVirt:
An open-source project that aims to run virtual machine workloads natively within a Kubernetes cluster. It allows the co-existence of VMs and containers unified under one control plane and the familiar tooling of Kubernetes (kubectl and the optional virtctl).

KubeVirt consists of a set of services:

```
            |
    Cluster | (virt-api-server) (virt-controller)
            |
------------+---------------------------------------
            |
 Kubernetes | (VM CRD) (VMI CRD)
            |
------------+---------------------------------------
            |
  DaemonSet | (virt-handler) (virt-launcher vm-pod managed by KubeVirt)
            |
```
virt-api-server: HTTP API server which serves as the entry point for all virtualization-related flows. It validates the VMI(CRD).
virt-controller: Manages all cluster-wide virtualization functionality by monitoring the VMI(CRDs) and managing the associated pods.
virt-handler: Every node needs a single instance of virt-handler. It is deployed as a DaemonSet. Like the virt-controller, the virt-handler is also watching for changes of the VMI object, once detected it will perform all necessary operations to change a VMI to meet the required state.
virt-launcher: For every VMI object one virt-launcher pod is created. This pod's primary container runs the virt-launcher KubeVirt component.
VM/VMI (CRD): VMI definitions are kept as custom resource definitions inside the Kubernetes API server. Defines VM properties CPU/RAM/NICs etc.
## Provision KubevVirt: 
The above components need to be deployed into the clusters first. Use Rafay blueprint to deploy KubeVirt to one or more clusters. 

Use the following  YAML definition file to create a KubeVirt add-on

Option 1:
* Use KubeVirt basic add-on without CDI operator
* Add CDI operators as workloads 
* Add CDI-CR as workload
* Add VM or VMI as workloads

Option 2:
* Use KubeVirt basic+cdi as an add-on.
* Add VM or VMI as workloads
