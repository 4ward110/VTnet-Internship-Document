## Draft docs

- Hot plugging vCPUs/RAM : You can hot plug vCPUs or RAM. Hot plugging mean enabling or disabling devices while a VM is running. and OS automatically recognize the change.

> Note: Hot unpluging a vCPU is only supported if the vCPU was previously hotplugged. A VM's vCPUs cannot be hot unplugged to less vCPUs than it was originally created with.

### Did KVM/QEMU support hot-plug

- CPU/RAM hotplug is a controverse feature. It exist on qemu-kvm for some time now and according to some BZ movement, we known there are users for it

- Very few physical machine support it

- `PCI`( Peripheral COmponent Interconnect): is any piece of computer hardware that plugs directly into a PCI slot on a computer's motherboard

### vCPU, Memory hotplug QEMU

- Hot plugging of memory is supported since version 2.1 of QEMU and is an alternative to memory ballooning.

- Removing memory support since QEMU 2.4

- Configure changes:

- To tell `libvirt` we want to have memory hotplug there has to be a maxMemory element and a NUMA node declaration

- maxMemory specifies the maximum amount of memory that is allowed to be plugged in and behaves the same way as the memory attribute

- Second limiteation is the amount of modules that can be plugged

- `libvirt` currently enforces a specified NUMA node for memory hotplug. id specifies the node number and cpus which vcpus belong to this node. Even if not all vcpus are active, the maximum amount of vcpus shoud be specified here.

### Hotplug with `virsh`

- `virsh` requires an `XML` file with a memory device. To add 128 MB, the file look:

```
<memory model='dimm'>
  <target>
    <size unit='MiB'>128</size>
    <node>0</node>
  </target>
</memory>
```

- To add it to the running guest adn also add it to the config use following cmd:

```
virsh attach-device <vm-name> <xml file name> --config --live
```

-
