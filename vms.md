# Virtual Machines

Notes about the preparation, configuration and use of virtual machines.

## Download

Useful sites for downloading qcow2 images:

- [Fedora Cloud. Cloud Base Images](https://alt.fedoraproject.org/cloud/)
- [Fedora CoreOS. Bare Metal & Virtualized](https://getfedora.org/en/coreos/download?tab=metal_virtualized&stream=stable)
- [OpenStack: Get images](https://docs.openstack.org/image-guide/obtain-images.html)

## Preparation of VM

### Creating a new virtual network for public access

1. Create a <code>bridged-network.xml</code> configuration file:

```
<network>
    <name>bridged-network</name>
    <forward mode="bridge" />
    <bridge name="br0" />
</network>
```

2. Define as virtual network:

```
# virsh net-define bridged-network.xml
```

3. Activate and set as auto-started:

```
# virsh net-start bridged-network
# virsh net-autostart bridged-network
```

4. Verify

```
# virsh net-list
Name              State    Autostart   Persistent
----------------------------------------------------
bridged-network   active   yes         yes
default           active   yes         yes
```

5. Create/Import VM with <code>bridged-network</code> as network:

```
# virt-install --name vmlab --memory 4096 --vcpus 2 --disk /var/lib/libvirt/images/vmlab.qcow2 \
--import --os-variant centos8 --network network=bridged-network --noautoconsole
```

### Resize a qcow2 image

- Check image info

```
# qemu-img info image.qcow2
```

- Resize image

```
# qemu-img resize image.qcow2 +20G
```

- To resize the underlying filesystems, we need to make a copy of the image, since virt-resize does not resize disk images in-place

```
# cp image.qcow2 image-orig.qcow2
```

**NOTE: In this example /dev/vda3 is not the /boot partition. So be careful you are growing the correct partitions on your qcow.**

- Check the partitions to ensure the filesystem to be grown

```
# virt-filesystems --long -h --all -a image.qcow2
```

- Grow <code>/dev/vda3</code>

```
# virt-resize --expand /dev/vda3 image-orig.qcow2 image.qcow2
```

- Inspect new disk

```
# qemu-img info image.qcow2
```

- Verify the filesystems

```
# virt-filesystems --long -h --all -a image.qcow2
```

### Customize a qcow2 image

- Setting the qcow2 image

```
# virt-customize -a /var/lib/libvirt/images/image.qcow2 --hostname vm01.sedena.lab \
--root-password password:rootpw --ssh-inject 'root:file:labkey.pub' --uninstall cloud-init \
--selinux-relabel
```

## Configuring VM

### Loading the VM

- Installing the VM

```
# virt-install --name vm01 --memory 1024 --vcpus 1 --disk /var/lib/libvirt/images/image.qcow2 --import \
--os-type linux --os-variant generic --noautoconsole
```

**Note: If you want to set the exact os-variant, check with the osinfo-query command:**

```
# osinfo-query os
```

## Using the VM

### Access

- Using console

```
# virsh console vm01
```

**NOTE: To escape from console use the key combination _ctrl+5_.**


