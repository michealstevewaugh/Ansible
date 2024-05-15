# onpremcluster
# onpremcluster-datasirpi

In hosts file we will define all required machines to connect 

[loadbalancer]

loadbalancer1 ansible_host=49.12.34.72 ansible_user=root

[masters]

master ansible_host=167.235.237.37 ansible_user=root
master1 ansible_host=159.69.247.153 ansible_user=root
master2 ansible_host=91.107.207.153 ansible_user=root

[workers]

worker1 ansible_host=49.12.75.51 ansible_user=root


[all:vars]

ansible_python_interpreter=/usr/bin/python3

non-root-user.yml 

 it will create ubuntu user on all host mentioned in  hosts file

kube-dependencies.yml 

 it will install all required components for master and worker nodes

master-cluster.yml 

 it will make master node to be available  with calico network installed

workers-cluster.yml

 it will make worker node to be available  with kubeadm init command

haproxy.yml 

 it will make ha proxy available for master nodes

Crictcl:
  
  crictcl images will not work initially due to docker issue this command below command ran manully we need to make it in ansible

**crictl config --set runtime-endpoint=unix:///run/containerd/containerd.sock --set image-endpoint=unix:///run/containerd/containerd.sock**

Public ip needed load load balancer 5.75.209.63 which will be assigned to each worker node in the path of worked node machine

**cd /etc/network ** in this interface file below line need to be added


auto eth0:1
iface eth0:1 inet static
 address <5.75.209.63>
 netmask 32

MetalLB is installed by below command:

**kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.9/config/manifests/metallb-native.yaml**

metallb confuguration files are placed in metallb folder path

Ingress controller metallB annotation:


    annotations:
      metallb.universe.tf/address-pool: production
