### Kubernetes,

**Getting started ,** https://kubernetes.io/docs/setup/

**Install Tools ,** https://kubernetes.io/docs/tasks/tools/

**Installing Kubernetes with deployment tools ,** https://kubernetes.io/docs/setup/production-environment/tools/

<br>

### KUBEADM,

**Bootstrapping clusters with kubeadm ,** https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/

**Installing kubeadm ,** https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

<br>

### System configuration,

**Keep firewalld and SELinux turned off,**

`[anup@rhel-92-04 ~]$ sudo systemctl status firewalld.service `

`[anup@rhel-92-04 ~]$ sudo systemctl stop firewalld.service `

`[anup@rhel-92-04 ~]$ sudo systemctl disable firewalld.service`
 

`[anup@rhel-92-04 ~]$ sudo setenforce 0`

<br>

**For : [WARNING Swap]: swap is enabled; production deployments should disable swap unless testing the NodeSwap feature gate of the kubelet**

`[anup@rhel-92-04 ~]$ sudo swapoff -a`

<br>

### Troubleshoot

`[anup@rhel-92-04 ~]$ sudo kubeadm reset`

`[anup@rhel-92-04 ~]$ ls -ltra`

<br>

<br>

### GO,

**Go Download,** https://go.dev/doc/install

`[anup@rhel-92-04 ~]$ wget https://go.dev/dl/go1.20.5.linux-amd64.tar.gz`

<br>

### Go installation,

**Remove any previous Go installation,**

`[anup@rhel-92-04 ~]$ rm -rf /home/anup/go && tar -C /home/anup -xzf go1.20.5.linux-amd64.tar.gz`

<br>

**Add /usr/local/go/bin to the PATH environment variable,**

`[anup@rhel-92-04 ~]$ export PATH=$PATH:/home/anup/go/bin`

`[anup@rhel-92-04 ~]$ echo $PATH`

<br>

**Verify installation,**

`[anup@rhel-92-04 ~]$ go version`

<br>

<br>

### Install Docker Engine on Red Hat,

https://docs.docker.com/engine/install/rhel/

### Uninstall old versions,

    [anup@rhel-92-04 ~]$ sudo yum remove docker \
                      docker-client \
                      docker-client-latest \
                      docker-common \
                      docker-latest \
                      docker-latest-logrotate \
                      docker-logrotate \
                      docker-engine \
                      podman \
                      runc


### Install using the rpm repository,

**Set up the repository,**

    [anup@rhel-92-04 ~]$ sudo yum update
    
    [anup@rhel-92-04 ~]$ sudo yum install -y yum-utils
    
    [anup@rhel-92-04 ~]$ sudo yum-config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
    
    [anup@rhel-92-04 ~]$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

**Install Docker Engine,**

    [anup@rhel-92-04 ~]$ sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

<br>

    [anup@rhel-92-04 ~]$ docker --version
    
    [anup@rhel-92-04 ~]$ sudo systemctl status docker.service 
    
    [anup@rhel-92-04 ~]$ sudo systemctl start docker.service
    
    [anup@rhel-92-04 ~]$ sudo systemctl enable docker.service 
    
    [anup@rhel-92-04 ~]$ sudo docker run hello-world


### Manage Docker as a non-root user,

    [anup@rhel-92-04 ~]$ sudo groupadd docker
    
    [anup@rhel-92-04 ~]$ sudo usermod -aG docker $USER
    
    [anup@rhel-92-04 ~]$ newgrp docker
    
    [anup@rhel-92-04 ~]$ docker run hello-world

<br>

<br>

### CRI,

**Installing Container Runtime Interface (CRI),** https://kubernetes.io/docs/setup/production-environment/container-runtimes/

**Docker Engine,** https://kubernetes.io/docs/setup/production-environment/container-runtimes/#docker

1. On each of your nodes, install Docker for your Linux distribution as per Install Docker Engine, https://docs.docker.com/engine/install/#server

2. Install cri-dockerd, following the instructions in that source code repository, https://github.com/Mirantis/cri-dockerd,

**Note:** For cri-dockerd, the CRI socket is unix:///var/run/cri-dockerd.sock by default.

<br>

`[anup@rhel-92-04 ~]$ sudo yum update`

`[anup@rhel-92-04 ~]$ sudo yum install git `

`[anup@rhel-92-04 ~]$ git clone https://github.com/Mirantis/cri-dockerd.git`

`[anup@rhel-92-04 ~]$ cd cri-dockerd/`

`[anup@rhel-92-04 ~]$ sudo yum update`

`[anup@rhel-92-04 ~]$ sudo yum install make`

`[anup@rhel-92-04 cri-dockerd]$ make cri-dockerd`

<br>

`[anup@rhel-92-04 ~]$ cd cri-dockerd`

`[anup@rhel-92-04 cri-dockerd]$ mkdir -p /usr/local/bin`

`[anup@rhel-92-04 cri-dockerd]$ sudo install -o root -g root -m 0755 cri-dockerd /usr/local/bin/cri-dockerd`

`[anup@rhel-92-04 cri-dockerd]$ sudo install packaging/systemd/* /etc/systemd/system`

`[anup@rhel-92-04 cri-dockerd]$ sudo sed -i -e 's,/usr/bin/cri-dockerd,/usr/local/bin/cri-dockerd,' /etc/systemd/system/cri-docker.service`

<br>

`[anup@rhel-92-04 cri-dockerd]$ sudo systemctl daemon-reload`

`[anup@rhel-92-04 cri-dockerd]$ sudo systemctl status cri-docker.service`

`[anup@rhel-92-04 cri-dockerd]$ sudo systemctl start cri-docker.service`

`[anup@rhel-92-04 cri-dockerd]$ sudo systemctl enable cri-docker.service`

`[anup@rhel-92-04 cri-dockerd]$ sudo systemctl status --now cri-docker.socket`

`[anup@rhel-92-04 cri-dockerd]$ sudo systemctl enable --now cri-docker.socket`

`[anup@rhel-92-04 cri-dockerd]$ ls -ltr /var/run/cri-dockerd.sock `

`[anup@rhel-92-04 cri-dockerd]$ cd`

<br>

<br>

### KUBEADM , KUBELET and KUBECTL

**Installing kubeadm, kubelet and kubectl , Red Hat 9 ,** https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl

    [anup@rhel-92-04 ~]$ cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
    [kubernetes]
    name=Kubernetes
    baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
    enabled=1
    gpgcheck=1
    gpgkey=https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
    exclude=kubelet kubeadm kubectl
    EOF

`[anup@rhel-92-04 ~]$ cat /etc/yum.repos.d/kubernetes.repo`

`[anup@rhel-92-04 ~]$ sudo setenforce 0`

`[anup@rhel-92-04 ~]$ sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config`

`[anup@rhel-92-04 ~]$ sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes`

`[anup@rhel-92-04 ~]$ sudo systemctl enable --now kubelet`

<br>

`[anup@rhel-92-04 ~]$ kubelet --version`

`[anup@rhel-92-04 ~]$ kubeadm version`

`[anup@rhel-92-04 ~]$ kubectl version`

`[anup@rhel-92-04 ~]$ kubectl version --client`

`[anup@rhel-92-04 ~]$ kubectl cluster-info`

`[anup@rhel-92-04 ~]$ kubectl version -o json`

<br>

<br>

### Configuring a cgroup driver,

**Not required for Docker CRI**

<br>

<br>

### Creating a Cluster with kubeadm,

`[anup@rhel-92-04 ~]$ kubeadm init`

`[anup@rhel-92-04 ~]$ sudo kubeadm init --pod-network-cidr=10.32.0.0/12 --apiserver-advertise-address=192.168.56.4 --cri-socket=unix:///var/run/cri-dockerd.sock (Weave Net)`

or,

`[anup@rhel-92-04 ~]$ sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=192.168.56.4 --cri-socket=unix:///var/run/cri-dockerd.sock (flannel)`

or,

`[anup@rhel-92-04 ~]$ sudo kubeadm init --pod-network-cidr=192.168.0.0/16 --apiserver-advertise-address=192.168.56.4 --cri-socket=unix:///var/run/cri-dockerd.sock (calico)`

<br>

**For root user,**

`[anup@rhel-92-04 ~]$ export KUBECONFIG=/etc/kubernetes/admin.conf`

<br>

**For nonroot user,**


`[anup@rhel-92-04 ~]$ mkdir -p $HOME/.kube`

`[anup@rhel-92-04 ~]$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config`

`[anup@rhel-92-04 ~]$ sudo chown $(id -u):$(id -g) $HOME/.kube/config`

<br>

**To join master,**


    kubeadm join 192.168.56.4:6443 --token 70gkub.z2blgf5fem7kxy8b \
            --discovery-token-ca-cert-hash sha256:fb0850c6gdab6j008cke574348b5bb5ddc9ba0a10746c8e9f7367d7723f243881a56

<br>

`[anup@rhel-92-04 ~]$ kubelet --version`
 
`[anup@rhel-92-04 ~]$ kubeadm version`

`[anup@rhel-92-04 ~]$ kubectl version`

<br>

`[anup@rhel-92-04 ~]$ kubectl version --client`

`[anup@rhel-92-04 ~]$ kubectl cluster-info`

`[anup@rhel-92-04 ~]$ kubectl version -o json`

`[anup@rhel-92-04 ~]$ strace -eopenat kubectl version`

<br>

<br>

### Installing CNI for POD networking, Installing a Pod network add-on ,

**Note: The parameter pod-network-cidr changes as per the network option.**

**Example:** The suggested CIDR for flannel and canal networks is 10.244.0.0/16 and for calico network it could be 192.168.0.0/16 , The default range that Weave Net would like to use is 10.32.0.0/12

It is not necessary to provide the parameter –pod-network-cidr for other network options like Contiv, Romana and Weavenet. However, it is must for the networks Romana and Weavenet to set /proc/sys/net/bridge/bridge-nf-call-iptables to 1 by running "sysctl net.bridge.bridge-nf-call-iptables=1" to pass bridged IPv4 traffic to iptables’ chains.


https://github.com/containernetworking/cni

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#pod-network

https://kubernetes.io/docs/concepts/cluster-administration/networking/

https://github.com/kubernetes/design-proposals-archive/blob/main/network/networking.md

https://kubernetes.io/docs/concepts/cluster-administration/addons/#networking-and-network-policy

https://kubernetes.io/docs/tasks/administer-cluster/network-policy-provider/

<br>

### Weave works,

https://www.weave.works/docs/net/latest/kubernetes/kube-addon/

**Integrating Kubernetes via the Addon,**

`[anup@rhel-92-04 ~]$ kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml`

<br>
