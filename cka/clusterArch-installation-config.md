# __Cluster Architecture, Installation and Configuration__ 
> 25% of the CKA (Certified Kubernetes Adminstrator) Curriculm exam 

## __Role based access control (RBAC)__
[Using RBAC Authorization ](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)

Regulates access to a computer or network depending on the role of the individual user.

RBAC authorization uses `rbac.authorization.k8s.io` API group to drive the descion making process which allows you to dynamically configure policies using the kubernetes API. 

To enable you need to add the  `--authorization-mode` flag when starting the API server: 
`kube-apiserver --authorization-mode=Example,RBAC --other-options --more-options`

### __API Objects__ 

The RBAC API declares four kinds of Kubernetes objects: `Role,ClusterRole, RoleBinding, ClusterRoleBinding`

- Describe or amend the RBAC objects using `kubectl` like any other Kubernetes object.

#### __Role and ClusterRole__
- _Role_ or _ClusterRole_ contains a set of permissions, they are purely additive and there are no "deny" rules.

- _Role_ always sets permissions in a particular namespace; when you create a _Role_ you have to specify which `namespace` it belongs in.

- _ClusterRole_ is a `non-namespaced` resource. 

_Role_ and _ClusterRole_ have different names because a Kubernetes object has to be either a `namespaced` or `not namespaced` resource. 

##### __ClusterRole uses__
- Define permissions on namespaced resourced and be granted access within `individual` namespace(s)
- Defined permissions on namespaced resources and be granted access across `all` namespaces
- Define permissions on cluster-scoped resources 

> Use a _Role_ to define a role within a namespace 
> Use a _ClusterRole_ to define a nrole cluster-wide

#### __Role Example__ 
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```
Create a role example: `kubectl create role pod-reader --verb=get --verb=list --verb=watch --resource=pods`

#### __ClusterRole Example__ 
```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  # "namespace" omitted since ClusterRoles are not namespaced
  name: secret-reader
rules:
- apiGroups: [""]
  #
  # at the HTTP level, the name of the resource for accessing Secret
  # objects is "secrets"
  resources: ["secrets"]
  verbs: ["get", "watch", "list"]
```
example of how to create a clusterole
`kubectl create clusterrole pod-reader --verb=get,list,watch --resource=pods`

## __Kubeadm to install a basic cluster__ 

Kubeadm is a tool used to build Kubernetes clusters. It performs the actions needed to get a minimum viable cluster up and running quickly. 

Cares only about bootstrapping. not about provisionion the underlying worker and master nodes. 

Also serves as a building block for higher-level tooling. 

### __Features__
- Quick minimum viable cluster creation: Designed to have all the components you need in one place and cluster regardless of where they are running 
- Portable: Can be used to set up a cluster anywhere on any device / architecture 
- Local development: Minimal dependencies mean its an ideal candidate for creating disploable clusters on a local machine 
- Building block for other tools: Serves as a building block for other tools like Kubespray

#### __Common uses__
- Testing
- Creating baselines for more advanced K8s deployments 
- Providing new K8s users a simple starting point for cluster configuration 

#### __Kubeadm vs kOps vs Kubespray__ 

| Functionality    | Kubeadm | Kops | Kubespray |
| -------- | ------- | -------- | ------- |
| __Infrastructure__ | Does not create infra   | Creates infra | Create infra
| __Creates “production-ready” clusters__ | No    | Yes | Yes
| __Lightweight__    | Yes    | No | No |
| __Manages cluster lifecycle__    | No    | Yes | Yes |

Reduces complexity, easy to get a useable K8's cluster deployed.

### Create a Kubernetes cluster using Kubeadm
1. __Install Docker on all three nodes__ 
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository 
       "deb [arch=amd64] https://download.docker.com/linux/ubuntu 
       $(lsb_release -cs) 
       Stable"
    sudo apt-get update
    sudo apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu
    sudo apt-mark hold docker-ce
```
check if installed correctly: 
`sudo systemctl status docker`

2. __Install kubeadm, kubelet, & kubectl on All Three Nodes__
```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
    deb https://apt.kubernetes.io/ kubernetes-xenial main
    EOF

sudo apt-get update

sudo apt-get install -y kubelet=1.14.5-00 kubeadm=1.14.5-00 kubectl=1.14.5-00

sudo apt-mark hold kubelet kubeadm kubectl

```

3. __Initiate Cluster Setup on the Master Node__
```
sudo kubeadm init --pod-network-cidr=10.244.0.0/16

mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl version

kubectl get nodes
```

4. __Add the Two Worker Nodes to the Cluster__
```
kubeadm join 10.0.1.101:6443 --token mgvt0h.3ui2w5lkjkcphc2x 
        --discovery-token-ca-cert-hash sha256:4e6be4e531e704e7b919a97a3b3359896b00faf3853c6d4240bf46d3a1eb990d

kubectl get nodes

```

5. __Set up Networking for the Cluster__

Turn on iptables bridge calls on all three nodes

```
echo "net.bridge.bridge-nf-call-iptables=1" |
    sudo tee -a /etc/sysctl.conf
    sudo sysctl -p

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml

kubectl get nodes

```

### __Best practices__ 
1. Dont use kubeadm for production clusters that require autoscaling
2. Take frequent backups of etcd 
3. Keep track of machine/nodes

## __Manage a highly-available kubernetes cluster__
> The Kubernetes control plane consists of the controller manager, scheduler, and API server. When running a highly available Kubernetes cluster, the first focus on running multiple replicas of these control plane components.

- Common failure domains to consider are tied to networks, disk/date, machines, power sources, cooling systems.

K8's is designed to handle brief control plane failures. Workloads contune to run and be accessible on the worker nodes, if the worker nodes fail, the control plane is not avaialbe to reschedule the work or reconfigure the routing within the cluster; this can cause workloads and services to be innacdessible. 

__Etcd provide high availability for the data plane__
K8'S uses etcd to srote cluster related data. 
you can run etcd as a multi-node cluster in productions. 
Critcal to keep etcd running because it functures as the entire brain of the clister. 

can run with multiple replicas.

its stateful, the date stored in one replica needs to match all the others 

1. Plan for failures you need to protect from happening 
2. Run multiple replicas of the control plane component across the failure domains 
3. Run etcd as a multi-node cluster in production
4. Back up etcd
5. Choose a leader election algorithm
6. Run your application across failure domains 

