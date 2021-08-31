## 1. Hypervisor & Virtualization

![](./img/virtual1.jpg)

- Virtualization is technology that allows you to create multiple simulated environments or dedicated resources from a single, physical hardware system. Software called a hypervisor connects directly to that hardware and allows you to split 1 system into separate, distinct, and secure environments known as virtual machines (VMs).

- Hypervisor software that creates and run virtual machines.A hypervisor, sometimes called a virtual machine monitor (VMM), isolates the hypervisor operating system and resources from the virtual machines and enables the creation and management of those VMs. Some Hypervisor like KVM, Virtual box, VMware, ... support to make virtualization possible.

![x86 structure](./img/ring.png)
![kernel](./img/kernel.png)

> - ring 0: hardware resource
> - ring 1: virtualization software
> - ring 2: Virtual machine
> - ring 3: OS

- In `x86` structure basically `Ring` have 2 mode work:

  - **User mode**: work on `ring 3`. From `ring3` to go to `ring 2`, `ring 1`, `ring 0` it need to use `system call`

  - **Kernel mode**: run in `ring 0` and it's have highest privilege, it's can access to hardware and have all privilege with the system.

- Type of Virtualization:
  - `RAM` Virtualization
  - `CPU` Virtualization
  - `Network` Virtualization
  - `Device I/O` Virtualization

- 3 main `CPU - Virtualization` : `full-virtualization`,`para-virtualization`, `Hardware Assisted Virtualization`
  - `full-virtualization`: Guest OS not modify OS to compatible with hardware. It's convert their assignment to binary then give it to VMM -> Hardware

   ![full-virtualization](./img/fullvirt.png)
  
  - `para-virtualization`: Guest OS has been modified to stay in `ring 0` and it's have privilege access directly to hardware.
  
   ![para-virtualization](./img/paravirt.png)

  - `Hardware Assisted Virtualization`: combine `full-virtualization` & `Para-virtualization`. It's have all advantage of 2 virtualization, OS `not` be modify, compatible with hardware, run in `ring 0`
  
   ![Hardware Assisted Virtualization](./img/hardwareassist.png)

- 2 Type of `Hypervisor` : `Native (bare metal) hypervisor` & `hosted hypervisor`
![](./img/hypervisor.png)

  - `Bare-metal`: run directly on the hardware between `the hardware` and `OS`. It's start  before `OS`  and interact directly with `kernel`. Some hypervisor : VMware ESXi, Apple BootCamp, Microsoft Hyper-V,...
  
  - `Hosted hypervisor`: A hosted hypervisor have installed in a computer like a application. It's can be manage and run many VMs at the same time. Some `hosted-hypervisor`: VMware workstation, VirtualBox ...

## 2. KVM - QEMU

### 2.1 What is KVM ?

- Kernel-based Virtual Machine (KVM) is an open source virtualization technology built into Linux x86 hardware and support virtualization (Intel VT-x or AMD-V)

- KVM lets you turn Linux kernel into a hypervisor (bare-metal hypervisor)that allows a host machine to run multiple, isolated virtual environment called guest or virtual machines

***KVM Architecture***
![](./img/kvmstructure.png)

***KVM Stack***
![](./img/kvmstack.png)

- User-facing tools: tools to manage VM support KVM

- Management layer: provide API to manage tools interact with KVM to manage virtual resource

- VM: virtual machine which user create. Normally, if don't use manage tools like virt-manager or virsh, KVM combine with QEMU hypervisor

- Kernel support: provide a module kernel for virtual infrastructure (kvm.ko), a module kernel support VT-x or AMD-V processor(kvm-intel.ko or kvm-amd.ko)

### 2.2 KVM feature

> KVM is part of Linux. Linux is part of KVM. Everything Linux has, KVM has too. But there are specific features that make KVM an enterprise’s preferred hypervisor.

- **Security**: KVM uses a combination of security-enhanced Linux (SELinux) and secure virtualization (sVirt) for enhanced VM security and isolation. SELinux establishes security boundaries around VMs. sVirt extends SELinux’s capabilities, allowing Mandatory Access Control (MAC) security to be applied to guest VMs and preventing manual labeling errors.

- **Storage**: KVM is able to use any storage supported by Linux, including some local disks and network-attached storage (NAS). Multipath I/O may be used to improve storage and provide redundancy. KVM also supports shared file systems so VM images may be shared by multiple hosts. Disk images support thin provisioning, allocating storage on demand rather than all up front.

- **Hardware support**: KVM can use a wide variety of certified Linux-supported hardware platforms. Because hardware vendors regularly contribute to kernel development, the latest hardware features are often rapidly adopted in the Linux kernel.

- **Memory management**: KVM inherits the memory management features of Linux, including non-uniform memory access and kernel same-page merging. The memory of a VM can be swapped, backed by large volumes for better performance, and shared or backed by a disk file.

- **Live migration**: KVM supports live migration, which is the ability to move a running VM between physical hosts with no service interruption. The VM remains powered on, network connections remain active, and applications continue to run while the VM is relocated. KVM also saves a VM's current state so it can be stored and resumed later.

- **Performance and scalability**: KVM inherits the performance of Linux, scaling to match demand load if the number of guest machines and requests increases. KVM allows the most demanding application workloads to be virtualized and is the basis for many enterprise virtualization setups, such as datacenters and private clouds (via OpenStack).

- **Scheduling and resource control**: In the KVM model, a VM is a Linux process, scheduled and managed by the kernel. The Linux scheduler allows fine-grained control of the resources allocated to a Linux process and guarantees a quality of service for a particular process. In KVM, this includes the completely fair scheduler, control groups, network name spaces, and real-time extensions.

- **Lower latency and higher prioritization** : The Linux kernel features real-time extensions that allow VM-based apps to run at lower latency with better prioritization (compared to bare metal). The kernel also divides processes that require long computing times into smaller components, which are then scheduled and processed accordingly.

### 2.3 KVM - QEMU

- QEMU like a `hypervisor type 2` , QEMU can be emulate hardware resource, which includes a virtual CPU. `QEMU` frive all OS instruction to `vCPU` through a translator (TCG-Tiny Core Generator) but TCG have low performance.

- But KVM support **mapping** `physical CPU` to `vCPU` => increase performance for hardware so QEMU can use KVM to be a accelerator.

- KVM-QEMU is combine QEMU with KVM => `Full Virtualization`

***KVM-QEMU structure***

![](./img/kvmqemu.png)

## REFERENCE

- [https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/VMware_paravirtualization.pdf](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/VMware_paravirtualization.pdf)

- [https://www.redhat.com/en/topics/virtualization/what-is-a-hypervisor](https://www.redhat.com/en/topics/virtualization/what-is-a-hypervisor)

- [https://www.redhat.com/en/topics/virtualization/what-is-KVM](https://www.redhat.com/en/topics/virtualization/what-is-KVM)

- [https://github.com/thanh474/thuc-tap/blob/master/KVM/tim-hieu-vitualization-va-hypervior.md#2](https://github.com/thanh474/thuc-tap/blob/master/KVM/tim-hieu-vitualization-va-hypervior.md#2)

- [https://github.com/hocchudong/thuctap012017/blob/master/TamNT/Virtualization/docs/KVM/1.Tim_hieu_KVM.md#1.3](https://github.com/hocchudong/thuctap012017/blob/master/TamNT/Virtualization/docs/KVM/1.Tim_hieu_KVM.md#1.3)

- [https://github.com/hocchudong/KVM-QEMU](https://github.com/hocchudong/KVM-QEMU)

- [https://news.cloud365.vn/kvm-tong-quan-ve-virtualization-va-hypervisor/](https://news.cloud365.vn/kvm-tong-quan-ve-virtualization-va-hypervisor/)

- [https://www.youtube.com/watch?v=CLR0pq9dy4g](https://www.youtube.com/watch?v=CLR0pq9dy4g)
