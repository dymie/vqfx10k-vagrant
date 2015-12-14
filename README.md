
This repository is a library of examples/demo using Vagrant and vqfx10K.  
Each example has its own directory and will contain at minima a Vagrantfile

Available examples
- full-1qfx-1srx (add link)[]
- full-2qfx (add link)[]
- light-2qfx (add link)[]
- light-2qfx-2srx (add link)[]

# How to contribute

Contribution to this repository are welcome and encouraged, especially to:
- Enhance the documentation of existing example
- Enhance existing example
- Create new example

### Guideline to create a new example



### How to provide feedback or report issue



# Vagrant providers

For now, only Virtualbox is supported as a provider  
Later, VMWare will be supported

# vQFX10K Mode, Architecture and Limitations

vqfx10k for Vagrant can work in 2 modes : **Full** and **Light**

## Mode Full
In mode full, it require 2 VMs for each instance
 - 1 Routing Engine VM, running Junos (add box name)
 - 1 Packet Forwarding Engine, running Linux (add box name)

>In mode Full, the dataplane is fully functionnal and is very close to the real QFX10K

These 2 VMS need to be interconnected with a dedicated private network on their interface number 1, (interface 0 is always reserved for Vagrant for the management).
All data-plane ports need to be connected to the Routing Engine (RE) VM, up to 12 data-plane ports are supported.
Ports name will be xe-0/0/0 to xe-0/0/11

### Limitations in Mode full
Only few features are not supported due to some architecture differences between vQFX10K and a real QFX10K

Not supported features:
 - Analytics
 - Guest VM/ Guest container

## Mode Light

In mode light, only 1 Routing Engine VM, running Junos is required for each instance

>In this mode, most dataplane features are not supported and all packets are processed in Kernel

Up to 12 ports are supported to connect to external devices, ports name will be em3 to em14

### Limitations in mode light
In this mode, most dataplane features are not supported and all packets are processed in Kernel

- To be confirmed -
interface em4 do not work
Family ethernet-switching is not supported

## VM Caracteristics

### vqfx10k-re

Require - 1G of memory

Interfaces type must be type : 82540EM

A maximum of 15 interfaces are supported:
- first interface is used by vagrant (em0)
- second interface is used to connect to the PFE VM (em1)
- third interface is a management port not used (em2)
- All remaining interfaces can be used to connected to other devices

This VM has 2 account by default:
- login: vagrant, with ssh_key authentification using vagrant insecure_key
- login: root, password: Juniper

### vqfx10k-pfe

Require:
- 1.5G of memory
- 1 dedicated core

A maximum of 2 interfaces are supported:
- first interface is used by vagrant (eth0)
- second interface is used to connect to the RE VM (eth1)

This VM has 2 account by default:
- login: vagrant, with ssh_key authentification using vagrant insecure_key
- login: root, password: no


# How to install and Requirement

## Requirement
 - Virtualbox, version 5.0.10 minimum
 - Vagrant, version 1.7 minimum

For Linux and MacOS, some examples require:
 - Ansible
 - Junos module for Ansible

## Vagrant

(add link to vagrant)

#### How to install Ansible on Linux
```
apt-get install ansible
ansible-galaxy install Juniper.junos
```

## Install Box file locally

In case Vagrant Box are not available on Vagrant Cloud, you'll have to install them manually first

You can install a box with the command
```
vagrant box add {box name} {box path}
```

Both vqfx10k-re and vqfx10k-pfe need to be installed
```
vagrant box add juniper/vqfx10k-re /{path to box file}/vqfx10k-re-virtualbox.box
vagrant box add juniper/vqfx10k-pfe /{path to box file}/vqfx10k-pfe1-virtualbox.box  
```

During Alpha/Beta box files are available on Github [add link]
