# Openstack Minimal Deployment

This project includes automation scripts for a minimal deployment for OpenStack (Yoga release)

  => You can see the full installation in this demo: https://www.youtube.com/watch?v=bKgNCKeTBiQ&t=26s

## Introduction
OpenStack consists of several independent parts, named the OpenStack services, all those services should be installed in the order specified below:
- Identity service
- Image service
- Placement service
- Nova Compute service
- Neutron Networking service
- Horizon Dashboard service

The example architecture requires at least two nodes (hosts), the following minimum requirements should support a proof-of-concept environment with core services and several CirrOS instances:

    - Controller Node: 1 processor, 4 GB memory, and 5 GB storage

    - Compute Node: 1 processor, 2 GB memory, and 10 GB storage
(Note: for first-time installation and testing purposes, many users select to build each host as a virtual machine (VM). The primary benefit of VMs is using One physical server that can support multiple nodes, each with almost any number of network interfaces.)

I- At Controller Node we install:

- host environment (DNS, NTP, DB, etc.) setup
- keystone
- glance
- nova
- neutron

II- At Compute Node we install:

- host environment (DNS, NTP, DB, etc.) setup
- nova-compute
- neutron


https://www.golinuxcloud.com/etc-network-interfaces-missing-ubuntu/
```
sudo sed -i -e 's/GRUB_CMDLINE_LINUX=\"\"/GRUB_CMDLINE_LINUX=\"net.ifnames=0 biosdevname=0\"/g' /etc/default/grub
sudo update-grub
sudo reboot
```
Edit the /etc/network/interfaces file to contain the following:
```
# The provider network interface
auto INTERFACE_NAME
iface  INTERFACE_NAME inet manual
up ip link set dev $IFACE up
down ip link set dev $IFACE down
```
Edit the /etc/hosts file to contain the following:
```
# controller
10.0.0.11       controller

# compute1
10.0.0.31       compute1
```




## Installation

Controller Node
```sh
$ chmod 733 openstack-install.sh
$ ./openstack-install.sh
```

Compute Node
```sh
$ chmod 733 compute-install.sh
$ ./compute-install.sh compute_ip controller_ip
```
## Errors


error while instance creation
```
Exceeds number of retries exhausted all host available for retreying build failures for instance ...
```
solution for this go to '/etc/nova/nova-compute' file and change in compute node
```
#virt_type=kvm
virt_type=qemu
```
then restart the service
```
service nova-compute restart
```
