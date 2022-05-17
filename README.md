# kube-vagrant
![Kubernetes Logo](https://raw.githubusercontent.com/kubernetes-sigs/kubespray/master/docs/img/kubernetes-logo.png)

Deploy kubernetes multi master ha cluster with Vagrant and virtualbox and using ceph as storage class (ceph rook)
after that deploy ha cluster mariadb with galera in ceph storage class and monitor that by prometheus and grafana
all steps is automated.
In this scenario we have 3 nodes with 6 core CPU and 12 Gi RAM.

## Requirements

- Host with 24 core CPU and 48 Gi RAM and any OS nested virtualization will be enabled.
- Download and install Vagrant in host from <https://www.vagrantup.com/downloads> .
- Download and install VirtualBox from <https://www.virtualbox.org/wiki/Downloads> .

Attention : we use <https://shecan.ir/> DNS in this project for lifting of sanctions

### Before deploy 
You need to install vagrant-disksize plugin for extra disk cause ceph rook need second hard disk drive in raw (without format)
for install please run in host:
```ShellSession
vagrant plugin install vagrant-disksize
````
## Quick Start

To deploy the cluster you can use :

### Vagrant

#### Usage

```ShellSession
# Install dependencies from ``requirements.txt``
sudo pip install -r requirements.txt
vagrant up
```

#### If a vagrant run failed and you've made some changes to fix the issue causing the fail, here is how you would re-run ansible:

```ShellSession
ansible-playbook -vvv -i .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory --become --become-user=root cluster.yml
```


### Access to cluster nodes and manage

```ShellSession
vagrant ssh k8s-1
sudo cp /etc/kubernetes/admin.conf .
sudo chown vagrant:vagrant admin.conf
export KUBECONFIG=admin.conf

kubectl get nodes -o wide
```

### Check cluster nodes state

```ShellSession
kubectl get nodes -o wide
```


## About the solutions use in this challenge

for deploying we use [kubespry] <https://github.com/kubernetes-sigs/kubespray.git>
in this project we have changed features and added new features :
* Use shecan dns by adding role in roles/ for lifting of sanctions
* Change in Vagrantfile for nodes extra disk
* Add rook_ceph role in cluster.yaml and create rook role in roles/kubernetes-app/ for installing rook as kubernetes storage class
* Add galera_cluster role in cluster.yaml and create galera_cluster role in roles/kubernetes-app/ for installing galera cluster database on storage class
* Add monitoring role in cluster.yaml and create monitoring role in roles/kubernetes-app/ for installing prometheus and grafana and monitor mysql metrics in dashboard with helm
