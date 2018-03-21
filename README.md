# RDO Stack - RDO OpenStack Queens setup

[Packstack: Create a proof of concept cloud](https://www.rdoproject.org/install/packstack/)

## Cinder, Swift volumes
```
# pvcreate /dev/sdb
# vgcreate cinder-volumes /dev/sdb

# pvcreate /dev/sdc
# vgcreate swift-volumes /dev/sdc
# lvcreate -n swift-lvs -l 100%FREE swift-volumes
# mkfs.ext4 /dev/swift-volumes/swift-lvs

# vgs
# lvs
# lsblk
```

## Locale
If non-English local `/etc/environment`
```
LANG=en_US.utf-8
LC_ALL=en_US.utf-8
```

## hosts
`/etc/hosts`
```
xxx.xxx.xxx.xxx        <hostname>
```

## fixed IP address
`/etc/sysconfig/network-scripts/ifcfg-xxxxxx`
```
BOOTPROTO=none
ONBOOT=yes
IPADDR=xxx.xxx.xxx.xxx
NETMASK=255.255.255.0
BROADCAST=xxx.xxx.xxx.255
```

## Network
```
$ sudo systemctl disable firewalld
$ sudo systemctl stop firewalld
$ sudo systemctl disable NetworkManager
$ sudo systemctl stop NetworkManager
$ sudo systemctl enable network
$ sudo systemctl start network
```
## Software repository
```
$ sudo yum install -y centos-release-openstack-queens
$ sudo yum-config-manager --enable openstack-queens
$ sudo yum update -y
```

## Install Packstack
```
$ sudo yum install -y openstack-packstack
```

## https://bugzilla.redhat.com/show_bug.cfi?id=1526064
```
$ sudo yum install -y python-setuptools
```

## Run Packstack to install OpenStack
```
$ sudo packstack --allinone --gen-answer-file=answer-file.cfg
```

### Edit answer-file.cfg
```
CONFIG_CINDER_VOLUMES_SIZE=30G
CONFIG_NEUTRON_OVS_BRIDGE_IFACES=br-ex:enp0s3
CONFIG_SWIFT_STORAGES=/dev/swift-volumes/swift-lvs
CONFIG_SWIFT_STORAGE_SIZE=20G
CONFIG_PROVISION_DEMO=n
```

### Install OpenStack
```
$ sudo packstack --answer-file=./answer-file.cfg
```
