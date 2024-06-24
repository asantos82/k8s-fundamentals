# k8s-fundamentals
Repo for activities of the k8s fundamentals training

## k8s-fundamentals - Lab Instances

The k8s fundamentals training has some lab activities.
So to perform them I needed to set up a lab with a control plane instance and 3 worker nodes.
The training does not provide any scripts or guidance on the best way to build the lab.
Only individual commands are shown, this will make the process tedious to provision 4 instances, and difficult to replicate.

I've decided to provision the instances with Vagrant and manage them with Ansible.

Currently, I have a MacBook Pro with an arm processor.
Virtualbox doesn't yet have a release for macOS arm CPUs. So I've decided to do the following:
  - Install on a Windows PC Vagrant, and provision the Virtualbox instances with it
  - Expose the instances on my LAN, so I can access them from my laptop
  - Provision/Manage the software on the instances with Ansible

### k8s-fundamentals - Lab Instances - Vagrant

After cloning this repo one should install [Vagrant for Windows](https://developer.hashicorp.com/vagrant/install?product_intent=vagrant#windows)


#### k8s-fundamentals - Lab Instances - Vagrant - Bring up the environment

Before bringing up the environment check [Vagrantfile](k8s-fundamentals-vagrant/Vagrantfile) and its configurations.

As is the following is assumed:

  - One Control Plane node will be provisioned
  - Three worker nodes will be provisioned
  - Network will be configured in Bridge mode
  - CIDR of your lan `192.168.1.0/24`
  - Control plain instances will start provisioning at address `192.168.1.10`
  - Worker instances will start provisioning at address `192.168.20.10`

> [!WARNING]  
>
> Review the configs and adjust them to your needs.
>
> Take extra attention to the networking settings

After you've reviewed the setting it is time to bring up the environment

```console
> vagrant up
```

<details>
  <summary>vagrant up - details of the execution</summary>

```console
> vagrant up
Bringing machine 'c1-cp1' up with 'virtualbox' provider...
Bringing machine 'c1-node1' up with 'virtualbox' provider...
Bringing machine 'c1-node2' up with 'virtualbox' provider...
Bringing machine 'c1-node3' up with 'virtualbox' provider...
==> c1-cp1: Box 'ubuntu/jammy64' could not be found. Attempting to find and install...
    c1-cp1: Box Provider: virtualbox
    c1-cp1: Box Version: 20240207.0.0
==> c1-cp1: Loading metadata for box 'ubuntu/jammy64'
    c1-cp1: URL: https://vagrantcloud.com/api/v2/vagrant/ubuntu/jammy64
==> c1-cp1: Adding box 'ubuntu/jammy64' (v20240207.0.0) for provider: virtualbox
    c1-cp1: Downloading: https://vagrantcloud.com/ubuntu/boxes/jammy64/versions/20240207.0.0/providers/virtualbox/unknown/vagrant.box
Download redirected to host: cloud-images.ubuntu.com
    c1-cp1:
==> c1-cp1: Successfully added box 'ubuntu/jammy64' (v20240207.0.0) for 'virtualbox'!
==> c1-cp1: Importing base box 'ubuntu/jammy64'...
==> c1-cp1: Matching MAC address for NAT networking...
==> c1-cp1: Checking if box 'ubuntu/jammy64' version '20240207.0.0' is up to date...
==> c1-cp1: Setting the name of the VM: c1-cp1
==> c1-cp1: Clearing any previously set network interfaces...
==> c1-cp1: Preparing network interfaces based on configuration...
    c1-cp1: Adapter 1: nat
    c1-cp1: Adapter 2: bridged
==> c1-cp1: Forwarding ports...
    c1-cp1: 22 (guest) => 2222 (host) (adapter 1)
==> c1-cp1: Configuring storage mediums...
    c1-cp1: Disk 'vagrant_primary' needs to be resized. Resizing disk...
==> c1-cp1: Running 'pre-boot' VM customizations...
==> c1-cp1: Booting VM...
==> c1-cp1: Waiting for mstachine to boot. This may take a few minutes...
    c1-cp1: SSH address: 127.0.0.1:2222
    c1-cp1: SSH username: vagrant
    c1-cp1: SSH auth method: private key
    c1-cp1: 
    c1-cp1: Vagrant insecure key detected. Vagrant will automatically replace
    c1-cp1: this with a newly generated keypair for better security.
    c1-cp1: 
    c1-cp1: Inserting generated public key within guest...
    c1-cp1: Removing insecure key from the guest if it's present...
    c1-cp1: Key inserted! Disconnecting and reconnecting using new SSH key...
==> c1-cp1: Machine booted and ready!
==> c1-cp1: Checking for guest additions in VM...
    c1-cp1: The guest additions on this VM do not match the installed version of
    c1-cp1: VirtualBox! In most cases this is fine, but in rare cases it can
    c1-cp1: prevent things such as shared folders from working properly. If you see
    c1-cp1: shared folder errors, please make sure the guest additions within the
    c1-cp1: virtual machine match the version of VirtualBox you have installed on
    c1-cp1: your host and reload your VM.
    c1-cp1:
    c1-cp1: Guest Additions Version: 6.0.0 r127566
    c1-cp1: VirtualBox Version: 7.0
==> c1-cp1: Setting hostname...
==> c1-cp1: Configuring and enabling network interfaces...
==> c1-cp1: Mounting shared folders...
    c1-cp1: /vagrant => C:/Users/dresa/Documents/git/personal/k8s-fundamentals/k8s-fundamentals-vagrant
==> c1-cp1: Running provisioner: shell...
    c1-cp1: Running: inline script
==> c1-node1: Box 'ubuntu/jammy64' could not be found. Attempting to find and install...
    c1-node1: Box Provider: virtualbox
    c1-node1: Box Version: 20240207.0.0
==> c1-node1: Loading metadata for box 'ubuntu/jammy64'
    c1-node1: URL: https://vagrantcloud.com/api/v2/vagrant/ubuntu/jammy64
==> c1-node1: Adding box 'ubuntu/jammy64' (v20240207.0.0) for provider: virtualbox
==> c1-node1: Importing base box 'ubuntu/jammy64'...
==> c1-node1: Matching MAC address for NAT networking...
==> c1-node1: Checking if box 'ubuntu/jammy64' version '20240207.0.0' is up to date...
==> c1-node1: Setting the name of the VM: c1-node1
==> c1-node1: Fixed port collision for 22 => 2222. Now on port 2200.
==> c1-node1: Clearing any previously set network interfaces...
==> c1-node1: Preparing network interfaces based on configuration...
    c1-node1: Adapter 1: nat
    c1-node1: Adapter 2: bridged
==> c1-node1: Forwarding ports...
    c1-node1: 22 (guest) => 2200 (host) (adapter 1)
==> c1-node1: Configuring storage mediums...
    c1-node1: Disk 'vagrant_primary' needs to be resized. Resizing disk...
==> c1-node1: Running 'pre-boot' VM customizations...
==> c1-node1: Booting VM...
==> c1-node1: Waiting for machine to boot. This may take a few minutes...
    c1-node1: SSH address: 127.0.0.1:2200
    c1-node1: SSH username: vagrant
    c1-node1: SSH auth method: private key
    c1-node1: 
    c1-node1: Vagrant insecure key detected. Vagrant will automatically replace
    c1-node1: this with a newly generated keypair for better security.
    c1-node1: 
    c1-node1: Inserting generated public key within guest...
    c1-node1: Removing insecure key from the guest if it's present...
    c1-node1: Key inserted! Disconnecting and reconnecting using new SSH key...
==> c1-node1: Machine booted and ready!
==> c1-node1: Checking for guest additions in VM...
    c1-node1: The guest additions on this VM do not match the installed version of
    c1-node1: VirtualBox! In most cases this is fine, but in rare cases it can
    c1-node1: prevent things such as shared folders from working properly. If you see
    c1-node1: shared folder errors, please make sure the guest additions within the
    c1-node1: virtual machine match the version of VirtualBox you have installed on
    c1-node1: your host and reload your VM.
    c1-node1:
    c1-node1: Guest Additions Version: 6.0.0 r127566
    c1-node1: VirtualBox Version: 7.0
==> c1-node1: Setting hostname...
==> c1-node1: Configuring and enabling network interfaces...
==> c1-node1: Mounting shared folders...
    c1-node1: /vagrant => C:/Users/dresa/Documents/git/personal/k8s-fundamentals/k8s-fundamentals-vagrant
==> c1-node1: Running provisioner: shell...
    c1-node1: Running: inline script
==> c1-node2: Box 'ubuntu/jammy64' could not be found. Attempting to find and install...
    c1-node2: Box Provider: virtualbox
    c1-node2: Box Version: 20240207.0.0
==> c1-node2: Loading metadata for box 'ubuntu/jammy64'
    c1-node2: URL: https://vagrantcloud.com/api/v2/vagrant/ubuntu/jammy64
==> c1-node2: Adding box 'ubuntu/jammy64' (v20240207.0.0) for provider: virtualbox
==> c1-node2: Importing base box 'ubuntu/jammy64'...
==> c1-node2: Matching MAC address for NAT networking...
==> c1-node2: Checking if box 'ubuntu/jammy64' version '20240207.0.0' is up to date...
==> c1-node2: Setting the name of the VM: c1-node2
==> c1-node2: Fixed port collision for 22 => 2222. Now on port 2201.
==> c1-node2: Clearing any previously set network interfaces...
==> c1-node2: Preparing network interfaces based on configuration...
    c1-node2: Adapter 1: nat
    c1-node2: Adapter 2: bridged
==> c1-node2: Forwarding ports...
    c1-node2: 22 (guest) => 2201 (host) (adapter 1)
==> c1-node2: Configuring storage mediums...
    c1-node2: Disk 'vagrant_primary' needs to be resized. Resizing disk...
==> c1-node2: Running 'pre-boot' VM customizations...
==> c1-node2: Booting VM...
==> c1-node2: Waiting for machine to boot. This may take a few minutes...
    c1-node2: SSH address: 127.0.0.1:2201
    c1-node2: SSH username: vagrant
    c1-node2: SSH auth method: private key
    c1-node2: 
    c1-node2: Vagrant insecure key detected. Vagrant will automatically replace
    c1-node2: this with a newly generated keypair for better security.
    c1-node2: 
    c1-node2: Inserting generated public key within guest...
    c1-node2: Removing insecure key from the guest if it's present...
    c1-node2: Key inserted! Disconnecting and reconnecting using new SSH key...
==> c1-node2: Machine booted and ready!
==> c1-node2: Checking for guest additions in VM...
    c1-node2: The guest additions on this VM do not match the installed version of
    c1-node2: VirtualBox! In most cases this is fine, but in rare cases it can
    c1-node2: prevent things such as shared folders from working properly. If you see
    c1-node2: shared folder errors, please make sure the guest additions within the
    c1-node2: virtual machine match the version of VirtualBox you have installed on
    c1-node2: your host and reload your VM.
    c1-node2:
    c1-node2: Guest Additions Version: 6.0.0 r127566
    c1-node2: VirtualBox Version: 7.0
==> c1-node2: Setting hostname...
==> c1-node2: Configuring and enabling network interfaces...
==> c1-node2: Mounting shared folders...
    c1-node2: /vagrant => C:/Users/dresa/Documents/git/personal/k8s-fundamentals/k8s-fundamentals-vagrant
==> c1-node2: Running provisioner: shell...
    c1-node2: Running: inline script
==> c1-node3: Box 'ubuntu/jammy64' could not be found. Attempting to find and install...
    c1-node3: Box Provider: virtualbox
    c1-node3: Box Version: 20240207.0.0
==> c1-node3: Loading metadata for box 'ubuntu/jammy64'
    c1-node3: URL: https://vagrantcloud.com/api/v2/vagrant/ubuntu/jammy64
==> c1-node3: Adding box 'ubuntu/jammy64' (v20240207.0.0) for provider: virtualbox
==> c1-node3: Importing base box 'ubuntu/jammy64'...
==> c1-node3: Matching MAC address for NAT networking...
==> c1-node3: Checking if box 'ubuntu/jammy64' version '20240207.0.0' is up to date...
==> c1-node3: Setting the name of the VM: c1-node3
==> c1-node3: Fixed port collision for 22 => 2222. Now on port 2202.
==> c1-node3: Clearing any previously set network interfaces...
==> c1-node3: Preparing network interfaces based on configuration...
    c1-node3: Adapter 1: nat
    c1-node3: Adapter 2: bridged
==> c1-node3: Forwarding ports...
    c1-node3: 22 (guest) => 2202 (host) (adapter 1)
==> c1-node3: Configuring storage mediums...
    c1-node3: Disk 'vagrant_primary' needs to be resized. Resizing disk...
==> c1-node3: Running 'pre-boot' VM customizations...
==> c1-node3: Booting VM...
==> c1-node3: Waiting for machine to boot. This may take a few minutes...
    c1-node3: SSH address: 127.0.0.1:2202
    c1-node3: SSH username: vagrant
    c1-node3: SSH auth method: private key
    c1-node3: 
    c1-node3: Vagrant insecure key detected. Vagrant will automatically replace
    c1-node3: this with a newly generated keypair for better security.
    c1-node3: 
    c1-node3: Inserting generated public key within guest...
    c1-node3: Removing insecure key from the guest if it's present...
    c1-node3: Key inserted! Disconnecting and reconnecting using new SSH key...
==> c1-node3: Machine booted and ready!
==> c1-node3: Checking for guest additions in VM...
    c1-node3: The guest additions on this VM do not match the installed version of
    c1-node3: VirtualBox! In most cases this is fine, but in rare cases it can
    c1-node3: prevent things such as shared folders from working properly. If you see
    c1-node3: your host and reload your VM.
    c1-node3: your host and reload your VM.
    c1-node3:
    c1-node3: Guest Additions Version: 6.0.0 r127566
    c1-node3: VirtualBox Version: 7.0
==> c1-node3: Setting hostname...
==> c1-node3: Configuring and enabling network interfaces...
==> c1-node3: Mounting shared folders...
    c1-node3: /vagrant => C:/Users/dresa/Documents/git/personal/k8s-fundamentals/k8s-fundamentals-vagrant
==> c1-node3: Running provisioner: shell...
    c1-node3: Running: inline script
```

</details>

#### k8s-fundamentals - Lab Instances - Vagrant - ssh to the instances

The created instances have the following names:

  - `c1-cp1` - control plane node name
  - `c1-node{1-3}` - worker nodes names

So to ssh to `c1-cp1` instance we execute:

```console
> vagrant ssh c1-cp1
Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 5.15.0-92-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

  System information as of Tue Feb 20 12:28:57 UTC 2024

  System load:             0.0
  Usage of /:              1.5% of 96.86GB
  Memory usage:            10%
  Swap usage:              0%
  Processes:               100
  Users logged in:         0
  IPv4 address for enp0s3: 10.0.2.15
  IPv4 address for enp0s8: 192.168.1.10
  IPv6 address for enp0s8: 2001:8a0:f0bc:900:a00:27ff:fe3b:b32e


Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

vagrant@c1-cp1:~$ 
```

> [!IMPORTANT]  
>
> `vagrant ssh <box>` only works from withing the Windows PC where vagrant + virtualbox are running.
>
> To access from other instances on your LAN, ssh config will need to be exported. More on that later

## k8s-fundamentals -Install k8s

Next, we'll be installing k8s dependencies onto the control-plane node as well as to the 3 worker nodes

### k8s-fundamentals -Install k8s - Control Plane Node

To install the k8s onto the control plane node the following commands must be executed.

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

sudo sysctl --system

sudo apt-get install -y containerd

sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml

sudo sed -i 's/            SystemdCgroup = false/            SystemdCgroup = true/' /etc/containerd/config.toml

sudo systemctl restart containerd

sudo apt-get install -y apt-transport-https ca-certificates curl gpg
sudo curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
apt-cache policy kubelet | head -n 20 

# Explicitly installing 1.29.3 version
VERSION=1.29.3-1.1
sudo apt-get install -y kubelet=$VERSION kubeadm=$VERSION kubectl=$VERSION 
sudo apt-mark hold kubelet kubeadm kubectl containerd

# Explicitly installing 3.28 calico version
wget --inet4-only https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/calico.yaml

# VMs lunched by vagran have at least two interfaces (one nat, one bridge) and we need to tell calico to bind to the bridge interface
# In this case it is interface enp0s8, that is what we are doing with the sed command below
sed -i '/- name: FELIX_HEALTHENABLED/{N;s/value: "true"/value: "true"\n            - name: IP_AUTODETECTION_METHOD\n              value: "interface=enp0s8"/}' calico.yaml

# Explicitly installing 1.29.3 version
VERSION=1.29.3
# 102.168.1.10 is the IP address of the lan interface for the control plane c1-cp1
# 10.0.8.0/21 is the CIDR for the pod network. Choose it wisely so it doesnt clash with the nat interface created by vagrant
sudo kubeadm init --kubernetes-version $VERSION --apiserver-advertise-address 192.168.1.10 --pod-network-cidr 10.0.8.0/21

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl apply -f calico.yaml

# Auto-complete instalation
sudo apt-get install -y bash-completion
echo "source <(kubectl completion bash)" >> ~/.bashrc
source ~/.bashrc
```

#### k8s-fundamentals -Install k8s - Control Plane Node - Checks

Check if the control plane node is in a healthy state and no pods are in a loop state or stuck starting up

```bash
#Gives pod status info
kubectl get pods --all-namespaces
kubectl get pods --all-namespaces -o wide

#Gives nodes status info
kubectl get nodes 
```

### k8s-fundamentals -Install k8s - Worker nodes

To install the k8s onto the worker nodes execute the following commands in each of the 3 worker nodes

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

sudo sysctl --system

sudo apt-get install -y containerd

sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml

sudo sed -i 's/            SystemdCgroup = false/            SystemdCgroup = true/' /etc/containerd/config.toml

sudo systemctl restart containerd

sudo apt-get install -y apt-transport-https ca-certificates curl gpg
sudo curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
apt-cache policy kubelet | head -n 20 

# Explicitly installing 1.29.3 version
VERSION=1.29.3-1.1
sudo apt-get install -y kubelet=$VERSION kubeadm=$VERSION kubectl=$VERSION 
sudo apt-mark hold kubelet kubeadm kubectl containerd

#### EXECUTE ON C1-CP1 ####
#You can also use print-join-command to generate token and print the join command in the proper format
#COPY THIS INTO YOUR CLIPBOARD
kubeadm token create --print-join-command

#### EXECUTE ON THE NODE ####

sudo kubeadm join 192.168.1.10:6443 --token pc66nu --discovery-token-ca-cert-hash sha256:370f9c9f 
```

#### k8s-fundamentals -Install k8s - Worker Nodes - Checks

Check if the worker nodes are in a healthy state and no pods are in a loop state or stuck starting up. nodes should also be in a healthy state

```bash
#Gives pod status info
kubectl get pods --all-namespaces
kubectl get pods --all-namespaces -o wide

#Gives nodes status info
kubectl get nodes 
```

## k8s-fundamentals - pods, deployments, services

Now that we have our cluster up and running we should run some workloads there.

### k8s-fundamentals - deployments and pods

Create a deployment using --dry-run to create the scaffolding

```console
vagrant@c1-cp1:~$ kubectl create deployment hello-world \
 --image=gcr.io/google-samples/hello-app:1.0 \
 --dry-run=client -o yaml > deployment.yaml
```

```yaml
vagrant@c1-cp1:~$ cat deployment.yaml 
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: hello-world
  name: hello-world
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-world
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: hello-world
    spec:
      containers:
      - image: gcr.io/google-samples/hello-app:1.0
        name: hello-app
        resources: {}
status: {}
```

```console
vagrant@c1-cp1:~$ kubectl create deployment hello-world --image=gcr.io/google-samples/hello-app:1.0
deployment.apps/hello-world created
```

Now create a pod

```console
vagrant@c1-cp1:~$ kubectl run hello-world-pod --image=gcr.io/google-samples/hello-app:1.0
pod/hello-world-pod created
```

Now it is time to check the pod status, their logs and try to access the service running

```console
vagrant@c1-cp1:~$ kubectl get pods -o wide
NAME                          READY   STATUS    RESTARTS   AGE    IP            NODE       NOMINATED NODE   READINESS GATES
hello-world-7879445f4-8pxz2   1/1     Running   0          3s     10.0.15.67    c1-node1   <none>           <none>
hello-world-pod               1/1     Running   0          110s   10.0.10.195   c1-node3   <none>           <none>

vagrant@c1-cp1:~$ kubectl logs hello-world-7879445f4-8pxz2 
2024/05/21 13:41:01 Server listening on port 8080
vagrant@c1-cp1:~$ kubectl logs hello-world-pod 
2024/05/21 13:39:18 Server listening on port 808

vagrant@c1-cp1:~$ curl http://10.0.15.67:8080
Hello, world!
Version: 1.0.0
Hostname: hello-world-7879445f4-8pxz2
vagrant@c1-cp1:~$ curl http://10.0.10.195:8080
Hello, world!
Version: 1.0.0
Hostname: hello-world-pod
```

When running `kubectl get pods` we can see that the deployment `NAME`is made of the deployment name plus a hash, i.e. `hello-world-7879445f4-8pxz2`
The pod is running with its name, no hash added, i.e. `hello-world-pod`

We can check which pods are running by using `crictl`
`crictl is a command-line interface for CRI-compatible container runtimes. You can use it to inspect and debug container runtimes and applications on a Kubernetes node. crictl and its source are hosted in the cri-tools repository.`

```console
vagrant@c1-node1:~$ sudo crictl --runtime-endpoint unix:///run/containerd/containerd.sock ps
CONTAINER           IMAGE               CREATED             STATE               NAME                ATTEMPT             POD ID              POD
1b3ad91f2b38e       dd1b12fcb6097       2 hours ago         Running             hello-app           0                   d0755c96096a9       hello-world-7879445f4-8pxz2
4099afd5b9ac0       4e42b6f329bc1       4 days ago          Running             calico-node         0                   e66183b37ba37       calico-node-9pndl
93fe4d9a77c94       a1d263b5dc5b0       4 days ago          Running             kube-proxy          0                   d3074b6147dd8       kube-proxy-zfkfk                  d3074b6147dd8       kube-proxy-zfkfk

vagrant@c1-node3:~$ sudo crictl --runtime-endpoint unix:///run/containerd/containerd.sock ps
CONTAINER           IMAGE               CREATED             STATE               NAME                ATTEMPT             POD ID              POD
3b587332b9994       dd1b12fcb6097       2 hours ago         Running             hello-world-pod     0                   e05d5c140f991       hello-world-pod
3c82d27bf16a1       4e42b6f329bc1       4 days ago          Running             calico-node         0                   32125165a158d       calico-node-lnzkw
5d0e922053a9b       a1d263b5dc5b0       4 days ago          Running             kube-proxy          0                   2a37aa68adb25       kube-proxy-8vkgt
```

Now let's exec into one of the pods.


> [!IMPORTANT]
>
> Container image was built using a distroless image for its base. So no bash or any other OS binaries are present
> https://github.com/GoogleCloudPlatform/kubernetes-engine-samples/blob/7fed665b624c92d7e787ca75b506b2d163a2cafb/quickstarts/hello-app/Dockerfile
> Let's get an older version of the container not build on a distroless image

```console
vagrant@c1-cp1:~$ kubectl exec -it hello-world-pod -- /bin/sh
error: Internal error occurred: error executing command in container: failed to exec in container: failed to start exec "fe21d2458d4c772946c6cd51aaf8e3de3c0531e37e6c326593d55cb5be177cfb": OCI runtime exec failed: exec failed: unable to start container process: exec: "/bin/sh": stat /bin/sh: no such file or directory: unknown
```

```console
vagrant@c1-cp1:~$ kubectl run hello-world-pod-old --image=gcr.io/google-samples/hello-app@sha256:2b0febe1b9bd01739999853380b1a939e8102fd0dc5e2ff1fc6892c4557d52b9
pod/hello-world-pod-old created

vagrant@c1-cp1:~$ kubectl get pods -o wide
NAME                          READY   STATUS    RESTARTS   AGE    IP            NODE       NOMINATED NODE   READINESS GATES
hello-world-7879445f4-8pxz2   1/1     Running   0          141m   10.0.15.67    c1-node1   <none>           <none>
hello-world-pod               1/1     Running   0          143m   10.0.10.195   c1-node3   <none>           <none>
hello-world-pod-old           1/1     Running   0          9s     10.0.11.3     c1-node2   <none>           <none>

vagrant@c1-cp1:~$ kubectl logs hello-world-pod-old
2024/05/21 16:02:43 Server listening on port 8080
vagrant@c1-cp1:~$ curl http://10.0.11.3:8080
Hello, world!
Version: 2.0.0
Hostname: hello-world-pod-old
vagrant@c1-cp1:~$ kubectl exec -it hello-world-pod-old -- /bin/sh
/ # hostname
hello-world-pod-old
/ # ip add
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: tunl0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
3: eth0@if9: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1480 qdisc noqueue state UP qlen 1000
    link/ether b2:49:5a:54:b1:59 brd ff:ff:ff:ff:ff:ff
    inet 10.0.11.3/32 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::b049:5aff:fe54:b159/64 scope link
       valid_lft forever preferred_lft forever
/ # exit
```

### k8s-fundamentals - kubectl describe

Let us take a closer look at our deployments and replicasets.

```console
vagrant@c1-cp1:~$ kubectl get deployments.apps hello-world 
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
hello-world   1/1     1            1           143m
vagrant@c1-cp1:~$ kubectl get replicasets
NAME                    DESIRED   CURRENT   READY   AGE
hello-world-7879445f4   1         1         1       144m
vagrant@c1-cp1:~$ kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
hello-world-7879445f4-8pxz2   1/1     Running   0          144m
hello-world-pod               1/1     Running   0          146m
hello-world-pod-old           1/1     Running   0          2m54s

vagrant@c1-cp1:~$ kubectl describe deployment hello-world 
Name:                   hello-world
Namespace:              default
CreationTimestamp:      Tue, 21 May 2024 13:41:00 +0000
Labels:                 app=hello-world
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=hello-world
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=hello-world
  Containers:
   hello-app:
    Image:        gcr.io/google-samples/hello-app:1.0
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   hello-world-7879445f4 (1/1 replicas created)
Events:          <none>

vagrant@c1-cp1:~$ kubectl describe replicaset hello-world
Name:           hello-world-7879445f4
Namespace:      default
Selector:       app=hello-world,pod-template-hash=7879445f4
Labels:         app=hello-world
                pod-template-hash=7879445f4
Annotations:    deployment.kubernetes.io/desired-replicas: 1
                deployment.kubernetes.io/max-replicas: 2
                deployment.kubernetes.io/revision: 1
Controlled By:  Deployment/hello-world
Replicas:       1 current / 1 desired
Pods Status:    1 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=hello-world
           pod-template-hash=7879445f4
  Containers:
   hello-app:
    Image:        gcr.io/google-samples/hello-app:1.0
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Events:           <none>

vagrant@c1-cp1:~$ kubectl describe pod hello-world-7879445f4-8pxz2 
Name:             hello-world-7879445f4-8pxz2
Namespace:        default
Priority:         0
Service Account:  default
Node:             c1-node1/192.168.1.20
Start Time:       Tue, 21 May 2024 13:41:00 +0000
Labels:           app=hello-world
                  pod-template-hash=7879445f4
Annotations:      cni.projectcalico.org/containerID: d0755c96096a9a1a5a1e1eea828aca9ad18806e90b9d096015ac619e1508094d
                  cni.projectcalico.org/podIP: 10.0.15.67/32
                  cni.projectcalico.org/podIPs: 10.0.15.67/32
Status:           Running
IP:               10.0.15.67
IPs:
  IP:           10.0.15.67
Controlled By:  ReplicaSet/hello-world-7879445f4
Containers:
  hello-app:
    Container ID:   containerd://1b3ad91f2b38e334432c21c52cc2e0752cea92b033a4243f4d9e9fc0dc0010b9
    Image:          gcr.io/google-samples/hello-app:1.0
    Image ID:       gcr.io/google-samples/hello-app@sha256:b1455e1c4fcc5ea1023c9e3b584cd84b64eb920e332feff690a2829696e379e7
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Tue, 21 May 2024 13:41:01 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-8hn26 (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       True
  ContainersReady             True
  PodScheduled                True
Volumes:
  kube-api-access-8hn26:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:                      <none>
```

### k8s-fundamentals - expose service

```console
vagrant@c1-cp1:~$ kubectl expose deployment hello-world --port=80 --target-port=8080
service/hello-world exposed

vagrant@c1-cp1:~$ kubectl get service hello-world 
NAME          TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
hello-world   ClusterIP   10.107.4.201   <none>        80/TCP    46s

vagrant@c1-cp1:~$ kubectl describe service hello-world 
Name:              hello-world
Namespace:         default
Labels:            app=hello-world
Annotations:       <none>
Selector:          app=hello-world
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.107.4.201
IPs:               10.107.4.201
Port:              <unset>  80/TCP
TargetPort:        8080/TCP
Endpoints:         10.0.15.67:8080
Session Affinity:  None
Events:            <none>

vagrant@c1-cp1:~$ curl http://10.107.4.201
Hello, world!
Version: 1.0.0
Hostname: hello-world-7879445f4-8pxz2

vagrant@c1-cp1:~$ kubectl get endpoints hello-world 
NAME          ENDPOINTS         AGE
hello-world   10.0.15.67:8080   5m59s

vagrant@c1-cp1:~$ kubectl get deployment hello-world -o yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
  creationTimestamp: "2024-05-21T13:41:00Z"
  generation: 1
  labels:
    app: hello-world
  name: hello-world
  namespace: default
  resourceVersion: "81400"
  uid: cecbe4d4-b183-4293-a274-f0a623fd6a29
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: hello-world
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: hello-world
    spec:
      containers:
      - image: gcr.io/google-samples/hello-app:1.0
        imagePullPolicy: IfNotPresent
        name: hello-app
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2024-05-21T13:41:01Z"
    lastUpdateTime: "2024-05-21T13:41:01Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2024-05-21T13:41:00Z"
    lastUpdateTime: "2024-05-21T13:41:01Z"
    message: ReplicaSet "hello-world-7879445f4" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 1
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1

  vagrant@c1-cp1:~$ kubectl get all
NAME                              READY   STATUS    RESTARTS   AGE
pod/hello-world-7879445f4-8pxz2   1/1     Running   0          14d
pod/hello-world-pod               1/1     Running   0          14d
pod/hello-world-pod-old           1/1     Running   0          14d

NAME                  TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
service/hello-world   ClusterIP   10.107.4.201   <none>        80/TCP    8m48s
service/kubernetes    ClusterIP   10.96.0.1      <none>        443/TCP   18d

NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/hello-world   1/1     1            1           14d

NAME                                    DESIRED   CURRENT   READY   AGE
replicaset.apps/hello-world-7879445f4   1         1         1       14d
vagrant@c1-cp1:~$ kubectl delete service hello-world 
service "hello-world" deleted
vagrant@c1-cp1:~$ kubectl delete deployment hello-world 
deployment.apps "hello-world" deleted
vagrant@c1-cp1:~$ kubectl delete pods hello-world-pod
pod "hello-world-pod" deleted
vagrant@c1-cp1:~$ kubectl delete pods hello-world-pod-old 
pod "hello-world-pod-old" deleted
vagrant@c1-cp1:~$ kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   19d
```

### k8s-fundamentals - deployments - replicas

Now we are going to create a deployment and update its number of replicas

```console
vagrant@c1-cp1:~$ kubectl create deployment hello-world \
> --image=gcr.io/google-samples/hello-app:1.0 \
> --dry-run=client -o yaml > deployment.yaml

vagrant@c1-cp1:~$ cat deployment.yaml 
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: hello-world
  name: hello-world
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-world
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: hello-world
    spec:
      containers:
      - image: gcr.io/google-samples/hello-app:1.0
        name: hello-app
        resources: {}
status: {}

vagrant@c1-cp1:~$ kubectl apply -f deployment.yaml 
deployment.apps/hello-world created
vagrant@c1-cp1:~$ kubectl get deployment
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
hello-world   1/1     1            1           12s

vagrant@c1-cp1:~$ kubectl expose deployment hello-world \
--port=80 --target-port=8080  
--dry-run=client -o yaml > service.yaml
vagrant@c1-cp1:~$ cat service.yaml 
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: hello-world
  name: hello-world
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 8080
  selector:
    app: hello-world
status:
  loadBalancer: {}

vagrant@c1-cp1:~$ kubectl get service
NAME          TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
hello-world   ClusterIP   10.99.52.33   <none>        80/TCP    2s
kubernetes    ClusterIP   10.96.0.1     <none>        443/TCP   19d
vagrant@c1-cp1:~$ kubectl get all
NAME                              READY   STATUS    RESTARTS   AGE
pod/hello-world-7879445f4-bb277   1/1     Running   0          3m17s

NAME                  TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
service/hello-world   ClusterIP   10.99.52.33   <none>        80/TCP    16s
service/kubernetes    ClusterIP   10.96.0.1     <none>        443/TCP   19d

NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/hello-world   1/1     1            1           3m17s

NAME                                    DESIRED   CURRENT   READY   AGE
replicaset.apps/hello-world-7879445f4   1         1         1       3m17s

# Increase replicastas to 20
# Modify the deployment.yaml
vagrant@c1-cp1:~$ kubectl apply -f deployment.yaml
deployment.apps/hello-world configured

vagrant@c1-cp1:~$ kubectl get deployments.apps hello-world 
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
hello-world   20/20   20           20          33m
```

```console
vagrant@c1-cp1:~$ kubectl get pods -o wide
```

<details>
  <summary>kubectl get pods -o wide - multiple pods running</summary>

```console
vagrant@c1-cp1:~$ kubectl get pods -o wide
NAME                          READY   STATUS    RESTARTS   AGE   IP            NODE       NOMINATED NODE   READINESS GATES
hello-world-7879445f4-2tnbp   1/1     Running   0          27m   10.0.11.9     c1-node2   <none>           <none>
hello-world-7879445f4-45gxp   1/1     Running   0          27m   10.0.11.6     c1-node2   <none>           <none>
hello-world-7879445f4-5vbjc   1/1     Running   0          16m   10.0.15.70    c1-node1   <none>           <none>
hello-world-7879445f4-8jbjl   1/1     Running   0          16m   10.0.15.74    c1-node1   <none>           <none>
hello-world-7879445f4-8t9xj   1/1     Running   0          16m   10.0.15.77    c1-node1   <none>           <none>
hello-world-7879445f4-b4htx   1/1     Running   0          27m   10.0.11.8     c1-node2   <none>           <none>
hello-world-7879445f4-bb277   1/1     Running   0          33m   10.0.11.4     c1-node2   <none>           <none>
hello-world-7879445f4-bkrtq   1/1     Running   0          16m   10.0.10.206   c1-node3   <none>           <none>
hello-world-7879445f4-cv4pb   1/1     Running   0          27m   10.0.10.198   c1-node3   <none>           <none>
hello-world-7879445f4-dv2b9   1/1     Running   0          27m   10.0.10.197   c1-node3   <none>           <none>
hello-world-7879445f4-f2n59   1/1     Running   0          16m   10.0.10.207   c1-node3   <none>           <none>
hello-world-7879445f4-mssz4   1/1     Running   0          16m   10.0.10.204   c1-node3   <none>           <none>
hello-world-7879445f4-nc5d4   1/1     Running   0          16m   10.0.10.203   c1-node3   <none>           <none>
hello-world-7879445f4-p8njf   1/1     Running   0          16m   10.0.10.205   c1-node3   <none>           <none>
hello-world-7879445f4-pkm5d   1/1     Running   0          16m   10.0.15.73    c1-node1   <none>           <none>
hello-world-7879445f4-plzqf   1/1     Running   0          27m   10.0.11.5     c1-node2   <none>           <none>
hello-world-7879445f4-qt5j2   1/1     Running   0          16m   10.0.15.75    c1-node1   <none>           <none>
hello-world-7879445f4-rv7w2   1/1     Running   0          27m   10.0.10.201   c1-node3   <none>           <none>
hello-world-7879445f4-sjgn4   1/1     Running   0          27m   10.0.10.196   c1-node3   <none>           <none>
hello-world-7879445f4-xv8k2   1/1     Running   0          27m   10.0.11.7     c1-node2   <none>           <none>
```

</details>

```console
vagrant@c1-cp1:~$ kubectl get service hello-world 
NAME          TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
hello-world   ClusterIP   10.99.52.33   <none>        80/TCP    26m

vagrant@c1-cp1:~$ curl http://10.99.52.33
Hello, world!
Version: 1.0.0
Hostname: hello-world-7879445f4-q7bn2
vagrant@c1-cp1:~$ curl http://10.99.52.33
Hello, world!
Version: 1.0.0
Hostname: hello-world-7879445f4-q7bn2
vagrant@c1-cp1:~$ curl http://10.99.52.33
Hello, world!
Version: 1.0.0
Hostname: hello-world-7879445f4-plzqf
vagrant@c1-cp1:~$ curl http://10.99.52.33
Hello, world!
Version: 1.0.0
Hostname: hello-world-7879445f4-js4kf
vagrant@c1-cp1:~$ curl http://10.99.52.33
Hello, world!
Version: 1.0.0
Hostname: hello-world-7879445f4-nc5d4
vagrant@c1-cp1:~$ curl http://10.99.52.33
Hello, world!
Version: 1.0.0
Hostname: hello-world-7879445f4-8t9xj
vagrant@c1-cp1:~$ 

# Edit the replica and apply it immediately
vagrant@c1-cp1:~$ kubectl edit deployments.apps hello-world
deployment.apps/hello-world edited

vagrant@c1-cp1:~$ kubectl get deployments.apps hello-world 
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
hello-world   30/30   30           30          35m

vagrant@c1-cp1:~$ kubectl scale deployment hello-world --replicas=40
deployment.apps/hello-world scaled
vagrant@c1-cp1:~$ kubectl get deployments.apps hello-world
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
hello-world   40/40   40           40          36m

```