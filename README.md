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
==> c1-cp1: Waiting for machine to boot. This may take a few minutes...
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