# Ubuntu Server Advanced

**Ubuntu 24.04 LTS (Noble Numbat)**

This is a combination student manual lab guide. It is intended to augment the delivery of the  **Ubuntu Server Advanced** training.

All chapters are structured in three sections:
* introduction to the concepts (theory)
* lab exercises to help you understand the theory
* lab solutions for your reference

Please follow along with the class and notify your facilitator if you require assistance.

---

## Table of Contents

1. [Virtualization](#virtualization)
2. [LXD](#lxd)
3. [OpenSSH](#openssh)
4. [System Initialization](#system-initialization)
5. [Storage](#storage)
6. [Advanced Filesystem Concepts](#advanced-filesystem-concepts)
7. [ZFS](#zfs)
8. [Advanced Networking Concepts](#advanced-networking-concepts)
9. [Security](#security)
10. [Snappy](#snappy)

[Appendices](#appendices)

---


# Virtualization

The purpose of this lab is to prepare the ground for the next chapters in this
course. It is mandatory to successully complete this lab in order to continue
the next chapters.

In this section you will learn to:
* Setup your lab QEMU+KVM virtual machine
* Complete an Ubuntu Linux installation process
* Add extra hardware to the VM

What is virtualization?
===


Virtualization refers to the partitioning of one physical server into several
virtual servers or machines. These machines act like real computers with operating
systems and can interact independently with other devices, applications, data, and
users as though they are separate physical resources. The virtual machines or
guests share the resources of the host computer. The machines are isolated from
each other such that, if one crashes, it can't affect the others.

There are two kinds of virtual machines - system and process (as in Java Virtual
Machine). Emulation refers to recreating hardware purely in software whereas
virtualization involves interacting with the host hardware (known as hardware
assist) to accomplish the same thing. **virt-what** - tool in universe which
should detect if running in a VM.

A hypervisor, or virtual machine manager, is the program that creates and runs
virtual machines. The hypervisor controls the host processor and resources, and
allocates them to each virtual machine. Virtualization can also be used to combine
multiple physical resources into a single virtual resource as in storage
virtualization. With storage virtualization multiple network storage resources are
combined into what appears as a single storage device. On Linux, QEMU+KVM are the
leading stack with libvirt for management.

To determine if a machine is virtual:
```bash
sudo dmidecode
sudo dmesg
```

**dmidecode** is a tool for dumping a computer's DMI (also called SMBIOS) tables
contents in a human-readable format. This table contains a description of the
system's hardware components, as well as other useful pieces of information such
as serial numbers and BIOS revision.

**dmesg** is used to examine or control the kernel console ring buffer. The default
action is to display all messages from the kernel console ring buffer.

**systemd-detect-virt** returns *0* if a virtualization technology is detected
(whether container or VM), non-zero otherwise. It prints the identifier of the tech
to standard output (STDOUT). The latter behaviour can be suppressed with the *-q*
option, which is convenient for scripting. It works with nested virtualisation as
well (see the man page).

KVM Virtualization Stack
---

The graphic below portrays a KVM virtualization stack. Each virtual machine (VM)
is a normal Linux process. Qemu is a device emulator. And, libvirt provides the
manageability.

![KVM Virtualization Stack](./images/kvm-stack.png)

### QEMU+KVM

QEMU can be used to emulate another machine although there is a performance penalty.
KVM stands for Kernel-based Virtual Machine and provides access to host machine
hardware via the kernel. It provides near-native performance.

When working alone, QEMU emulates both CPU and hardware. When working together,
KVM arbitrates access to the CPU and memory, and QEMU emulates the hardware resources
(hard disk, video, USB, etc.).

### What is KVM?

**KVM** is a hypervisor which provides near-native speeds by running directly on the
hardware using processor extensions. KVM is a hypervisor that runs inside an
operating system. It requires a processor with hardware virtualization extensions.
KVM can virtualize *x86_64*, *i386* (also known as *x86_32*), *ARM64*, *PowerPC*,
and *S390* on those specific architectures. It is the most commonly used hypervisor
in OpenStack and it is used by many cloud vendors.

Ubuntu uses **KVM** as the back-end virtualization technology primarily for servers and
libvirt as its toolkit or API. Libvirt front ends for managing VMs include *virt-manager*
(GUI) and *virsh* (CLI).

**KVM** a lows a userspace host to set up the guest VM's address space. The host must
also supply a firmware image (usually a custom BIOS when emulating PCs) that the guest
can use to bootstrap into its main OS.

### What is QEMU?

**QEMU** is short for Quick Emulator. It is a userspace host. QEMU uses KVM when
available to virtualize guests at near-native speeds by executing the guest code
directly on the host CPU. Otherwise it falls back to software-only emulation, this
functionality is known in QEMU as the TCG or Tiny Code Generator. It is a generic
software emulator/simulator, capable of simulating a variety of different hardware.

Parts of **QEMU** are also used as the building blocks for many other virtualization
solutions. For example, QEMU in Ubuntu can emulate arm, mips, ppc, sparc, x86, and
more.

**QEMU** is not exclusive to the KVM hypervisor, it can also be used in conjunction
with the  **Xen** hypervisor to provide BIOS emulation for the bootstrap phase of the
guest operating systems.

### Qemu-img

QEMU is a disk image utility that lets you create disk images of many types (RAW,
QCOW2, VMDK) to serve as storage for virtual machines. It lets you convert between
different image types including importing and exporting images from the CEPH
distributed storage system.

It can query images for information with`bash
qemu-img info
```

### libvirt

The **libvirt** suite is used to interface with different virtualization technologies.
It is used to manage QEMU+KVM virtual machines, which it calls domains.

Before getting started with libvirt it is best to make sure your hardware supports
the necessary virtualization extensions for KVM. Running the following command
checks to see if you have the needed processor features active to run KVM VMs:
```bash
sudo apt install cpu-checker
sudo kvm-ok
```

**libvirt** includes the **virsh** command set, allowing the creation and
manipulation of machines, networks, storage pools and devices. These are defined
using XML files which can be exported, imported or edited on the fly.

Virtualization Lab
===

### Installing a Guest VM

In this lab you will setup the virtual machine (VM) you will use for the course:
`LABVM` on `LABHOST`.

 * LABHOST - the assigned lab host that the student logs in to.
 * LABVM - the virtual machine set up by the student and used for the class.

1. From `LABHOST`, begin everything with an update:

```bash
sudo apt update -y && sudo apt upgrade -y
sudo reboot
2. Install the packages needed for VM creation, that are not part of standard Ubuntu.

```bash
sudo apt install -y cpu-checker libvirt-daemon-system qemu-kvm qemu-utils virtinst \
      virt-viewer virt-manager ubuntu-vm-builder bridge-utils
3. See if your LABHOST supports virtualization.

```bash
sudo kvm-ok
4. Install unzip and glib-compile-schemas to get virt-install to work.

```bash
sudo apt install -y unzip libglib2.0-dev
5. Add your user into the two virtualization groups.

```bash
sudo adduser `id -un` libvirt
sudo adduser `id -un` kvm
`You must logout and login to activate the new groups (to use virsh w/o sudo).
> Log out / Log in

6. List the virtual machines already existing (there should be none).`bash
virsh list --all
```

## Setting up storage pools

Libvirt is capable of working with a number of storage back-ends:

 * Directory pool
 * Filesystem pool
 * Network filesystem pool
 * Logical volume pool
 * Disk pool
 * iSCSI pool
 * SCSI pool
 * Multipath pool
 * RBD pool
 * Sheepdog pool
 * Gluster pool
 * ZFS pool
 * Vstorage pool

For now we will be focusing on the `Directory` storage pool. So let's add a new pool.

1. Create a directory that will host our images:

```bash
sudo mkdir -p /var/lib/libvirt/vm-disks
2. Define a pool with this directory as a target:

```bash
virsh pool-define-as --name vm-disks --type dir --target /var/lib/libvirt/vm-disks
3. Let's check that the pool has been created:

```bash
virsh pool-list --all
 Name                 State      Autostart 
-------------------------------------------
 vm-disks             inactive   no        

5. We have a pool, but it's inactive. We need to start it:

```bash
virsh pool-start vm-disks
6. And we need to make sure that it starts automatically on boot:

```bash
virsh pool-autostart vm-disks
7. Let's check again:

```bash
virsh pool-list --all
 Name                 State      Autostart 
-------------------------------------------
 vm-disks             active     yes       
`Great. We have a pool that is active and will start automatically after reboot. We should be able to create disks as needed and fetch information about the pool at will:

```bash
virsh pool-info vm-disks
Name:           vm-disks
UUID:           f4162baa-add5-4b0c-855c-54998cbf11c2
State:          running
Persistent:     yes
Autostart:      yes
Capacity:       418.54 GiB
Allocation:     6.47 GiB
Available:      412.07 GiB
`This should suffice for our needs in this lab.

## Volume operations

Now that we have a storage pool, we can create, delete, resize and get information about volumes in that pool.


1. Create a new volume`bash
virsh vol-create-as vm-disks sample-vol 6G --format qcow2
`This will create a new `qcow2` volume called `sample-vol` with a maximum size of `6GB`. By default libvirt creates sparse files and does not pre-allocate disk space. It is worth mentioning that there is a `vol-create` command as well, that takes a `xml` file as an argument.

2. Let's check that we have the volume:

```bash
virsh vol-list --details vm-disks
 Name        Path                                  Type  Capacity  Allocation
------------------------------------------------------------------------------
 sample-vol  /var/lib/libvirt/vm-disks/sample-vol  file  6.00 GiB  196.00 KiB

3. Great! The volume has been created. We can also get some info about that volume in particular:

```bash
virsh vol-info sample-vol vm-disks
Name:           sample-vol
Type:           file
Capacity:       6.00 GiB
Allocation:     196.00 KiB
4. Resize the volume:

```bash
virsh vol-resize sample-vol --pool vm-disks --capacity 10G
5. Check if the size of the disks has been updated:

```bash
virsh vol-info sample-vol vm-disks 
Name:           sample-vol
Type:           file
Capacity:       10.00 GiB
Allocation:     260.00 KiB
```

### Cloning volumes

1. Cloning volumes`bash
virsh vol-clone sample-vol cloned-vol vm-disks
2. This will create a 1:1 clone of the original volume. So we will get two identical disks:

```bash
virsh vol-list --details vm-disks
 Name        Path                                  Type   Capacity  Allocation
-------------------------------------------------------------------------------
 cloned-vol  /var/lib/libvirt/vm-disks/cloned-vol  file  10.00 GiB  196.00 KiB
 sample-vol  /var/lib/libvirt/vm-disks/sample-vol  file  10.00 GiB  200.00 KiB

3. Alternatively, to save up on space, you can leverage the `qcow2` copy-on-write functionality, and use the original volume as a backing file to create a new volume.`bash
sudo qemu-img create -f qcow2 -b /var/lib/libvirt/vm-disks/sample-vol \
    /var/lib/libvirt/vm-disks/cow-clone
4. Let's check the resulting image:

```bash
sudo qemu-img info --backing-chain /var/lib/libvirt/vm-disks/cow-clone 
image: /var/lib/libvirt/vm-disks/cow-clone
file format: qcow2
virtual size: 10G (10737418240 bytes)
disk size: 196K
cluster_size: 65536
backing file: /var/lib/libvirt/vm-disks/sample-vol
Format specific information:
    compat: 1.1
    lazy refcounts: false
    refcount bits: 16
    corrupt: false

image: /var/lib/libvirt/vm-disks/sample-vol
file format: qcow2
virtual size: 10G (10737418240 bytes)
disk size: 200K
cluster_size: 65536
Format specific information:
    compat: 0.10
    refcount bits: 16

`As we can see, there is now a COW image with a backing file pointing to our original image.

**IMPORTANT NOTE**: In this scenario, booting or modifying the original image will corrupt all other disks that use it as a backing file. Take great care to never attach a backing file to a virtual machine.

5. Let's check out volume pool to see if the new image is registered:

```bash
virsh vol-list --details vm-disks
 Name        Path                                  Type   Capacity  Allocation
-------------------------------------------------------------------------------
 cloned-vol  /var/lib/libvirt/vm-disks/cloned-vol  file  10.00 GiB  196.00 KiB
 sample-vol  /var/lib/libvirt/vm-disks/sample-vol  file  10.00 GiB  200.00 KiB

6. The new image does not show up. This is due to the fact that libvirt does not watch the storage pool for changes that have happened independently of libvirt. So if we manually create a disk, or copy one over, we need to refresh the storage pool:

```bash
virsh pool-refresh vm-disks
`Let's check again:

```bash
virsh vol-list --details vm-disks
 Name        Path                                  Type   Capacity  Allocation
-------------------------------------------------------------------------------
 cloned-vol  /var/lib/libvirt/vm-disks/cloned-vol  file  10.00 GiB  196.00 KiB
 cow-clone   /var/lib/libvirt/vm-disks/cow-clone   file  10.00 GiB  196.00 KiB
 sample-vol  /var/lib/libvirt/vm-disks/sample-vol  file  10.00 GiB  200.00 KiB
```

### Define aditional networks in libvirt

Defining a new network in libvirt is a bit more involved. It requires you to create a XML file that describes the network. Let's create a XML file for a new DHCP enabled NAT network. We already have the default one, but we are creating an additional one to showcase how it's done:

Create `nat-kvm-network.xml` with the following content:

```xml
<network>
    <name>nat-kvm-network</name>
    <forward mode='nat'>
        <nat>
            <port start='1024' end='65535'/>
       </nat>
    </forward>
    <bridge name='nat-virbr2'/>
    <ip address='192.168.101.1' netmask='255.255.255.0'>
    <dhcp>
        <range start='192.168.101.100' end='192.168.101.254'/>
    </dhcp>
    </ip>
</network>

1. Using this file we can create the network:

```bash
virsh net-define nat-kvm-network.xml
2. Start the network:

```bash
virsh net-start nat-kvm-network
3. Set the network to start automatically on boot:

```bash
virsh net-autostart nat-kvm-network
4. Creating a new network will start a new bridge configured with the IP address in the XML:

```bash
ip addr sh nat-virbr2|grep inet
# output
inet 192.168.101.1/24 brd 192.168.101.255 scope global nat-virbr2
```

## Creating our first VM

It's time to put it all together. In the following example we will use `virt-install` to create an Ubuntu 24.04 virtual machine, and we will perform network install, using serial console. We will also show you how to automate that setup using an answer file. We will connect the VM to the `nat-kvm-network` network defined in libvirt.

For simplicity's sake and to be able to reuse this later if need be, in the home directory of the ubuntu user, there is a script called `create_vm.sh`. Open it in an text editor, inspect it, and close the editor afterwards:

```bash
#!/usr/bin/env bash

DISK_FORMAT="qcow2"
DISK_SIZE="15" ##  GB
DISK_BUS="virtio"
EXTRA_ARGS="console=ttyS0,115200n8 serial"
LOCATION="http://archive.ubuntu.com/ubuntu/dists/noble/main/installer-amd64/"
NETWORK="nat-kvm-network"
NETWORK_MODEL="virtio"
OS_TYPE="linux"
OS_VARIANT="ubuntu24.04"
RAM="8192"
VCPUS="4"
VMName="ubuntu"


virt-install \
--name $VMName \
--ram $RAM \
--disk pool=vm-disks,size=$DISK_SIZE,bus=$DISK_BUS \
--vcpus $VCPUS \
--os-type $OS_TYPE \
--os-variant $OS_VARIANT \
--network network:$NETWORK,model=$NETWORK_MODEL \
--graphics none \
--location $LOCATION \
--initrd-inject=/home/ubuntu/preseed.cfg \
--noreboot \
--extra-args "$EXTRA_ARGS"
`The preseed file is also located in `/home/ubuntu/preseed.cfg`, referenced in the script above:

```bash
### Localization
d-i debian-installer/locale string en_US
d-i console-setup/ask_detect boolean false
d-i keyboard-configuration/layoutcode string us

### Network configuration
d-i netcfg/choose_interface select auto
d-i netcfg/get_hostname string unassigned-hostname
d-i netcfg/get_domain string domain.com
d-i netcfg/wireless_wep string

### Mirror settings
d-i mirror/country string manual
d-i mirror/http/directory string /ubuntu
d-i mirror/http/proxy string
d-i mirror/http/hostname string mirrors.archive.ubuntu.com

### Clock and time zone setup
d-i clock-setup/utc boolean true
d-i time/zone string US/Eastern
d-i clock-setup/ntp boolean true

### Partitioning
d-i partman-auto/disk string /dev/vda
d-i partman-auto/method string lvm
d-i partman-lvm/device_remove_lvm boolean true
d-i partman-md/device_remove_md boolean true
d-i partman-lvm/confirm boolean false
d-i partman-lvm/confirm_nooverwrite boolean true
d-i partman-auto-lvm/guided_size string max
# - atomic: all files in one partition
# - home:   separate /home partition
# - multi:  separate /home, /usr, /var, and /tmp partitions
d-i partman-auto/choose_recipe select atomic
d-i partman-partitioning/confirm_write_new_label boolean true
d-i partman/choose_partition select finish
d-i partman/confirm boolean true
d-i partman/confirm_nooverwrite boolean true
d-i partman-md/confirm boolean true
d-i partman-partitioning/confirm_write_new_label boolean true
d-i partman/choose_partition select finish
d-i partman/confirm_nooverwrite boolean true

### Account setup
d-i passwd/root-login boolean false
d-i passwd/user-fullname string ubuntu
d-i passwd/username string ubuntu
d-i passwd/user-password password ubuntu
d-i passwd/user-password-again password ubuntu
# Use mkpasswd -m sha-512 to create an MD5 hash password
#d-i passwd/user-password-crypted password [MD5 hash]
d-i user-setup/allow-password-weak boolean true
d-i user-setup/encrypt-home boolean false

### Apt setup

### Package selection
tasksel tasksel/first multiselect
# Individual additional packages to install
d-i pkgsel/include string git openssh-server python-simplejson sudo
d-i pkgsel/update-policy select none
d-i pkgsel/upgrade select none
popularity-contest popularity-contest/participate boolean false

### Boot loader installation
d-i grub-installer/only_debian boolean true
d-i grub-installer/with_other_os boolean true

### Finishing up the installation
d-i finish-install/reboot_in_progress note
d-i debian-installer/exit/poweroff boolean true

### Preseeding other packages

#### Advanced options
d-i preseed/late_command string \
echo "ubuntu    ALL=(ALL) NOPASSWD:ALL" >> /target/etc/sudoers; \
in-target ln -s /lib/systemd/system/serial-getty@.service \
  /etc/systemd/system/getty.target.wants/serial-getty@ttyS0.service; \
rm -f /target/etc/ssh/ssh_host_*; \
in-target sed -i -e 's|exit 0||' /etc/rc.local; \
in-target sed -i -e 's|.*test -f /etc/ssh/ssh_host_dsa_key.*||' /etc/rc.local; \
in-target bash -c 'echo "test -f /etc/ssh/ssh_host_dsa_key || dpkg-reconfigure \
  openssh-server" >> /etc/rc.local'; \
in-target bash -c 'echo "exit 0" >> /etc/rc.local'; \
cat /dev/null > /target/etc/hostname
`Let's kick things off:

1. Let's kick things off:

```bash
chmod +x ./create_vm.sh
./create_vm.sh
```

### Accessing the VM

Once the installation is complete, the VM will shut down.

1. The above preseed will set the VM to output its console via serial console (see last few lines). This means that we can access this VM using:

```bash
virsh console ubuntu
`If serial console is not enabled during setup, it will be impossible to connect to the machine via  `virsh console`, but you can still connect using the graphical console. But due to the fact that we just deployed the VM using a preseed that has the serial console enabled, that should not be a problem.

Another option is to set a static IP address for this VM in DHCP. Once we boot the VM, we will know to which IP address we can connect. Let's go ahead and do this now.

2. First we need to fetch the MAC address of the new VM:

```bash
$ virsh dumpxml ubuntu | grep "mac address"
      <mac address='52:54:00:91:db:e8'/>
3. In my case, the MAC address is `52:54:00:91:db:e8`. Let's set a static address from the default pool for this MAC:

```bash
virsh net-update nat-kvm-network add ip-dhcp-host \
  "<host mac='<MAC address from 2.>' name='ubuntu' ip='192.168.101.50'/>" \
  --live --config
`Yes...I know it's atrocious, but bare with me. This will set a static mapping for our VMs MAC to IP address  `192.168.101.50`. Once we boot the VM, that is the IP it should receive.

4. Time to boot the VM:

```bash
virsh start ubuntu
5. Wait for a few seconds, and see if you can reach the VM:

```bash
# ping -c3 192.168.101.50
PING 192.168.101.50 (192.168.101.50) 56(84) bytes of data.
64 bytes from 192.168.101.50: icmp_seq=1 ttl=64 time=0.337 ms
64 bytes from 192.168.101.50: icmp_seq=2 ttl=64 time=0.156 ms
64 bytes from 192.168.101.50: icmp_seq=3 ttl=64 time=0.185 ms

--- 192.168.101.50 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 1998ms
rtt min/avg/max/mdev = 0.156/0.226/0.337/0.079 ms
6. Log in the LABVM with the `console` command:

```bash
virsh console ubuntu

sudo /usr/bin/ssh-keygen -A
```

**NOTE**: You may need to hit the `Enter` key once.

**NOTE**: You can exit the serial console by typing:  `ctrl+]`

7. You should now be able to access it using the username and password set in the preseed:
  * Username: ubuntu
  * Password: ubuntu`bash
ssh ubuntu@192.168.101.50
`If you wish to have this VM automatically boot after the host boots, run the following command:

```bash
virsh autostart ubuntu
```

## Attaching extra disks to the VM

Attaching an extra disk can be easily done via `virsh`. Up until this point we have talked about `qcow2` images. In the following example we will create  simple RAW images and attach them to the VM. We will then partition it and format the partition.

1. Creating the disks we're going to attach to LABVM:

```bash
sudo qemu-img create -f raw /var/lib/libvirt/vm-disks/test-vmdiskb.raw 5G
sudo qemu-img create -f raw /var/lib/libvirt/vm-disks/test-vmdiskc.raw 5G
sudo qemu-img create -f raw /var/lib/libvirt/vm-disks/test-vmdiskd.raw 5G
sudo qemu-img create -f raw /var/lib/libvirt/vm-disks/test-vmdiske.raw 5G
sudo qemu-img create -f raw /var/lib/libvirt/vm-disks/test-vmdiskf.raw 5G
2. Now let's refresh the pool:

```bash
virsh pool-refresh vm-disks
virsh vol-list --details vm-disks
3. And we can now attach the disk to the LABVM:

```bash
virsh attach-disk ubuntu /var/lib/libvirt/vm-disks/test-vmdiskb.raw vdb --live --config
virsh attach-disk ubuntu /var/lib/libvirt/vm-disks/test-vmdiskc.raw vdc --live --config
virsh attach-disk ubuntu /var/lib/libvirt/vm-disks/test-vmdiskd.raw vdd --live --config
virsh attach-disk ubuntu /var/lib/libvirt/vm-disks/test-vmdiske.raw vde --live --config
virsh attach-disk ubuntu /var/lib/libvirt/vm-disks/test-vmdiskf.raw vdf --live --config
4. Virsh inspects the disk type automatically and adds the appropriate entry in the VM. In the above command we attached the raw disks we just created inside the VM. Now ssh into the VM and inspect the disks:

```bash
ssh ubuntu@192.168.101.50

sudo ls -l /dev/vd*
5. Go back to the `LABHOST`:

```bash
exit
```

### UVTool Lab

Starting with 14.04 LTS, a tool called `uvtool` greatly facilitates the task of
generating virtual machines (VM) using the cloud images. uvtool provides a simple
mechanism to synchronize cloud-images locally and use them to create new VMs in
minutes.

1. Install the necessary software.`bash
sudo apt install -y uvtool
2. Filter to use one specific image.

```bash
uvt-simplestreams-libvirt sync release=noble arch=amd64
3. List the available images (local).

```bash
uvt-simplestreams-libvirt query
4. Specify a release or image while creating a VM.

```bash
uvt-kvm create uvtool_vm release=noble --password ubuntu
5. List the VMs. (You should have two now. ) Use virsh or uvt-kvm.

```bash
uvt-kvm list
`Wait until the VM is ready. It finds and adds the IP address to the host. 

6. Save the IP address.`bash
uvt-kvm wait uvtool_vm --insecure
7. View the IP address for both VMs.

```bash
uvt-kvm ip uvtool_vm
8. ssh to the node using the IP. (user=ubuntu.)

```bash
export UVTIP=`uvt-kvm ip uvtool_vm`
ssh ubuntu@$UVTIP
9. Modify the kernel parameters in the new VM to allow console access.

```bash
sudo sed --in-place 's/ttyS0/ttyS0,115200/g' /etc/default/grub
10. Apply the change to the boot parameters.

```bash
sudo update-grub
11. Reboot the VM for the change to be applied.

```bash
sudo shutdown -r now
12. Connect to the VM through the console.

```bash
virsh console uvtool_vm
13. Exit the console with `ctrl+]` and delete the virtual machine.`bash
uvt-kvm destroy uvtool_vm
```

# LXD

In this section you will learn to:
* Define LXD
* Setup and manage a container

## Containerization

Containers are isolated userspace instances running on the same kernel effectively providing multiple machines. They share certain portions of the host kernel and operating system instance so they require much less overhead than hypervisor based virtualization. (With full machine virtualization, a virtual machine runs its own full kernel and operating system instance.) A single host can run many more containers than VMs. Containers can be started, stopped and deleted extremely quickly.

There are two broad types:

 * Application containers (Ex. Docker)
 * Full machine containers (LXD)

Containers run on top of LXC. LXC is an operating-system-level virtualization method for running multiple isolated Linux systems (containers) on a control host using a single Linux kernel. LXC is a userspace interface for the Linux kernel containment features. The interface has an application programming interface (API) where Linux users can create and manage system or application containers.

## What is LXD?
LXD is a pure-container hypervisor that runs unmodified Linux operating systems and applications with VM-style operations at incredible speed and density. LXD is an enhancement of the existing LXC Linux container hypervisor with it’s own toolset. The goal is to provide an interface that is similar to a virtual machine. However, it uses Linux containers instead of hardware virtualization. LXC-based containers are easier to use through the addition of a back-end daemon supporting  REST API and a CLI client that works with both the local daemon and remote daemons via the REST API. RESTful API allows communication between LXD and its clients over http which is encapsulated over SSL for remote operations or a Unix socket for local operations. Once LXD is installed, an image is required. The container is based on that image.

LXD is comprised of the following components:

 * A system-wide daemon (lxd) which exports a REST API locally (and if enabled, over the network).
 * A command line client (lxc) which is a simple tool to manage containers: connect multiple container hosts, provide a network overview of all containers, and create and move containers.

LXD is image based. A number of remote image stores are already available. Images will be downloaded to the local LXD store when a container is first launched. At its simplest, LXD is a daemon which provides a REST API to drive LXC containers. Its main goal is to provide a user experience that’s similar to that of virtual machines but using Linux containers rather than hardware virtualization.


## LXD Setup LAB

Traditionally, LXD used to get installed from the default repository, or from a PPA. With the arrival of  `snappy` we can now install LXD as a snap. We must clean any LXD version that may have been pre-installed on the system:

Run the following commands on the `ubuntu` `LABVM`.

1. Change the hostname of the VM:

```bash
ssh ubuntu@192.168.101.50

sudo hostnamectl set-hostname ubuntu
sudo reboot
2. Reconnect back to the VM:

```bash
ssh ubuntu@192.168.101.50
3. Remove the apt install lxd packages, we'll install it another way:

```bash
sudo apt remove --purge lxd lxd-client
4. Install ZFS support:

```bash
sudo apt-get -y install zfsutils-linux
5. Create lxd group and add ourselves to it:

```bash
sudo groupadd --system lxd
sudo usermod -G lxd -a $USER
newgrp lxd
6. And install LXD as a snap:

```bash
sudo apt install -y snapd
sudo snap install lxd
7. Initialize LXD:

```bash
sudo lxd init
`Follow the setup wizard. In most cases defaults should do.

8. Launch a new container:

You must logout and login to activate the new groups (to use lxd w/o sudo).
> Log out / Log in`bash
lxc launch ubuntu:24.04 noble
lxc list
9. Afterwards, rename the new image with `ubuntu` as alias (use `lxc image list` to get the image fingerprint)

```bash
lxc image alias create ubuntu <fingetprint>
`This will download the 24.04 image from the remote `ubuntu:` repository and launch a container using it.

## Using a remote LXD as an image server

1. When using a remote image server, add it as a remote and use it:

```bash
lxc launch images:centos/7/amd64 centos
2. An image list can be obtained with:

```bash
# Colon is required
lxc image list images:
# Check local store for downloaded images
lxc image list local:
```

### Creating and using a container

1. Create the first container with:

```bash
lxc launch ubuntu first
2. That will create and start a new ubuntu container. Confirm it with:

```bash
lxc list
3. The container here is called “first”. Let LXD give it a random name by calling “lxc launch ubuntu” without a name. Once the container is running, get a shell inside it with:

```bash
lxc exec first -- /bin/bash
4. Or run a command directly:

```bash
lxc exec first -- apt-get update
5. To pull a file from the container, use:

```bash
lxc file pull first/etc/hosts .
6. To push one, use:
```bash
lxc file push hosts first/tmp/
7. To stop the container:

```bash
lxc stop first
8. And to remove it entirely:

```bash
lxc delete first
```

### Images

1. A few key image servers come preloaded.`bash
$ lxc remote list

+-----------------+------------------------------------------+---------------+--------+--------+
|      NAME       |                   URL                    |   PROTOCOL    | PUBLIC | STATIC |
+-----------------+------------------------------------------+---------------+--------+--------+
| images          | https://images.linuxcontainers.org       | simplestreams | YES    | NO     |
+-----------------+------------------------------------------+---------------+--------+--------+
| local (default) | unix://                                  | lxd           | NO     | YES    |
+-----------------+------------------------------------------+---------------+--------+--------+
| ubuntu          | https://cloud-images.ubuntu.com/releases | simplestreams | YES    | YES    |
+-----------------+------------------------------------------+---------------+--------+--------+
| ubuntu-daily    | https://cloud-images.ubuntu.com/daily    | simplestreams | YES    | YES    |
+-----------------+------------------------------------------+---------------+--------+--------+
2. The following lists all the images on the `images` server. The colon is required.`bash
sudo lxc image list images:
sudo lxc image list local:
```

## LXD profiles

LXD profiles are used to store pre-defined configurations for containers. You can think of it as a configuration template you can apply to new containers. When creating a container you can associate one or more profiles that will get applied in the order they are specified.

1. To list all profiles:

```bash
lxc profile list
2. To edit a profile:

```bash
lxc profile edit <profile name>
3. To show a profile:

```bash
lxc profile show <profile name>
```


## LXD snapshots

1. Creating a snapshot:

```bash
lxc snapshot noble my-snapshot
`If `ZFS` is used, this operation should be instant. It leverages the `ZFS` copy on write snapshotting functionality to create snapshots that themselves can be used as a base for other containers.

2. Use this to see information about the container, including snapshots:

```bash
lxc info noble
3. Restoring a machine from a previous snapshot:

```bash
lxc restore noble my-snapshot
4. Creating a new container from a snapshot is fast and easy. You simply have to run:

```bash
lxc copy noble/my-snapshot my-new-container-from-snapshot
5. You should now have a new container. We need to apply a profile to it:

```bash
# you can also add multiple profiles by separating them with
# with a comma: default,test-profile

lxc profile assign my-new-container-from-snapshot default
6. And we can now start it:

```bash
lxc start my-new-container-from-snapshot
lxc list
7. Stop and delete all the containers you have created so far. `lxc list`, `lxc stop` and `lxc delete` will help you with this task.

# OpenSSH

In this section you will learn to:
* Describe key OpenSSH features
* Perform OpenSSH server configuration
* Use OpenSSH client tools
* Perform SSH key management

What is OpenSSH?
===

The OpenSSH client is included in Ubuntu by default. Secure Shell (ssh) allows
you to connect to a different computer and execute commands despite the fact
that you are not physically sitting in front of it. SSH grants terminal access
to the computer. It is intended to provide secure encrypted communications
between two untrusted hosts over an insecure network.

SSH connects and logs into the specified hostname (with an optional user name).
The user must prove his or her identity to the remote machine using a password,
a key, or a token.  Using a cryptographic key is the recommended method.

Connecting to a remote host:

```bash
ssh user@<remote_host>
`Installing the OpenSSH server on a host:

```bash
sudo apt install openssh-server
`You can configure it by editing the  **sshd_config** file in the
 **/etc/ssh** directory. **sshd_config** is the configuration file for the
OpenSSH server. **ssh_config** is the configuration file for the OpenSSH client.
*Make sure not to get them mixed-up.*

### Examples

First, make a backup of your **sshd_config** file by copying it to your home
directory, or by making a read-only copy in **/etc/ssh**.

To make the copy:

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.factory-defaults
sudo chmod a-w /etc/ssh/sshd_config.factorydefaults
`Creating a read-only backup in **/etc/ssh** means you'll always be able to find a
known, good configuration when you need it. Once you've backed up your
**sshd_config** file, you can make changes with any text editor, for example:

```bash
sudo gedit /etc/ssh/sshd_config
`Runs the standard graphical text editor in Ubuntu. Once you've made your changes,
you can apply them by saving the file then doing:

```bash
sudo systemctl restart ssh
`Configuring OpenSSH means striking a balance between security and ease-of-use.
Ubuntu's default configuration tries to be as secure as possible without making
it impossible to use in common use cases. Consider the changes you can make,
and how they affect the balance between security and ease-of-use. You should
decide what balance is right for your specific situation.

For a comprehensive list of available options, their meanings and defaults,
please see the **ssh_config(5)** man page:

```bash
man 5 ssh_config
`SSH Keys
---

SSH keys allow authentication between two hosts without the need of a password.
Public key authentication is more secure than password authentication. This is
particularly important if the computer is visible on the internet.

Key-based authentication uses two keys, one "pubic" key that anyone is allowed to
see, and another "private" key that only the owner is allowed to see. Each key is
a large number with special mathematical properties. The private key is kept on the
computer you log in from, while the public key is stored in the **authorized_keys**
file on all the computers you want to log in to. Usually that is located in the
**~/.ssh/** folder.

When you log in to a computer, the SSH server uses the public key to "lock"
messages in a way that can only be "unlocked" by your private key. As an extra
security measure, most SSH programs store the private key in a passphrase-protected
format. If your computer is stolen or broken in to, you should have enough time
to disable your old public key before the thieves break the passphrase and start
using your key.

### Key-Based SSH Logins

Key-based authentication is considered one of the most secure of several modes of
authentication usable with OpenSSH, such as plain password and Kerberos tickets.
Key-based authentication has several advantages over password authentication. For
example, the key values are significantly more difficult to brute-force, or guess
than plain passwords, provided an ample key length. Other authentication methods
are only used in very specific situations. SSH can use either "RSA"
(Rivest-Shamir-Adleman), "DSA" (Digital Signature Algorithm) keys or more recently
Elliptic Curve keys. Both RSA and DSA were considered state-of-the-art algorithms
when SSH was invented, but DSA has come to be seen as less secure in recent years.
While RSA is the recommended choice for new keys, EC keys such as **ECDSA** and
**ED25519** are providing similar security at lesser expense to compute them.

To securely communicate using key-based authentication, one needs to:
* create a key pair
* securely store the private key on the computer one wants to log in from
* store the public key on the computer one wants to log in to

> *Note:* Kerberos is a protocol for SSH Keys authentication that uses tickets to 
> authenticate. It avoids storing passwords locally or sending them over the internet
> and uses a trusted 3rd-party (known as the KDC). It is built on symmetric-key
> cryptography. Your ticket is your proof of identity encrypted with a secret key for
> the particular service requested. As long as the tickt is valid, you can access the
> service within Kerberos.

### Generating Keys

Using key based logins with ssh is generally considered more secure than using
plain password logins. This section of the guide will explain the process of
generating a set of *public/private* RSA keys, and using them for logging into
your Ubuntu computer(s) via OpenSSH.

To generate a set of public/private RSA keys, enter the following at a terminal
prompt:

```bash
ssh-keygen -t rsa
`This will generate the keys using the RSA Algorithm. During the process you will
be prompted for a passphrase. Hit Enter when prompted to create the key for a blank
passphrase.

> *Note:* The recommended approach is to have a password.

By default the public key is saved in the file  **~/.ssh/id_rsa.pub**, while
 **~/.ssh/id_rsa** contains the private key.

Copy the **id_rsa.pub** file to the remote host and append it to 
 **~/.ssh/authorized_keys** by entering:

```bash
ssh-copy-id username@remotehost
`Finally, double check the permissions on the  **authorized_keys** file; only the
authenticated user should have read and write permissions. If the permissions
are not correct change them by entering:

```bash
chmod 600 .ssh/authorized_keys
`You should now be able to SSH to the host without being prompted for a password.

> *Note:* OpenSSH server checks the permissions of the key file and if they are too
> relaxed the server will refuse to authenticate you.

SSH Tools
---

### ssh-agent

**ssh-agent** is a program to hold private keys used for public key authentication.
It is included in the OpenSSH client package. The **ssh-agent** is started in the
beginning of an X-session or a login session, and all other windows or programs are
started as clients to the ssh-agent program. Through use of environment variables
the agent can be located and automatically used for authentication when logging in
to other machines using ssh.

> *Note:* X11 is a network protocol designed for Unix and similar operating systems to
> enable remote graphical access to applications. A machine running an X windowing system
> can launch a program on a remote computer.

The agent initially does not load any private keys. Keys are added using ssh-add.

When executed without arguments, ssh-add loads the files **~/ssh/id_rsa**,
**~/.ssh/id_dsa**, **~/.ssh/id_ecdsa**, **~/.ssh/id_ed25519** and
**~/.ssh/identity**.

If the identity has a passphrase, **ssh-add** asks for the passphrase on the terminal
if it has one or from a small X11 program if running under X11. If neither of these is
the case, then the authentication will fail.
It then sends the identity to the agent. Several identities can be stored in the agent;
the agent can automatically use any of these identities. `ssh-add -l` displays the
identities currently held by the agent.

The idea is that the agent is ran in the user's local PC, laptop, or terminal.
Authentication data need not be stored on any other machine, and authentication
passphrases never go over the network. However, the connection to the agent can be
forwarded over SSH remote logins, and the user can thus use the privileges given by
the identities anywhere in the network in a secure way.

*Example:*
```bash
$ ssh-agent bash
$ ssh-add
Identity added: /home/vagrant/.ssh/id_rsa (/home/vagrant/.ssh/id_rsa)
$ ssh-add -l
1024 SHA256:RaqZqq+CWokFTQ+MrvkuIVzY+jgxaZC+QUxx7pOOtiQ /home/vagrant/.ssh/id_rsa (RSA)
```

### Using rsync with ssh

We can use ssh with rsync for data transfer ensuring your data is being transferred in
a secured manner.

To copy a file from a remote server locally:

```bash
rsync -azvPe ssh user@10.10.10.10:/path-to/file.txt /tmp/
`To copy a file from the local machine to a remote location:

```bash
rsync -azvPe ssh file.txt user@10.10.10.10:/pathto/backups/
`OpenSSH Server Configuration
---

The configuration file **/etc/ssh/sshd_config** is the system configuration file for
the OpenSSH Server daemon. The system wide OpenSSH client configuration file is
**/etc/ssh/ssh_config**.

*Note:*
> **sshd_config** is the configuration file for the OpenSSH *server*. **ssh_config**
> is the configuration file for the OpenSSH *client*. Make sure not to get them mixed up.

* Port 22 is the default SSH port; this can be changed to help fool port sniffers.
* Protocol version 1 contains security vulnerabilities and is disabled by default.
* Protocol version 2 is recommended and the default in Ubuntu.
* Security can be improved by allowing only defined users to login via SSH. Allowing
  or denying SSH access for specific users can significantly improve your security
  if users with poor security practices don't need SSH access. It can also be improved
  by denying defined users or all those not defined in allowed.
* Change **PermitRootLogin** to **no** to deny root logins over SSH.
* Do not allow the use of empty passwords. This is the default in Ubuntu.
* **ListenAddress** restricts which interfaces SSH will listen on. By default it binds
  to all interfaces.
* Setting **PasswordAuthentication** to **no** to disable logging in via user name and
  password and will force the use of SSH keys for authentication.


SSH Lab
===

In this lab you will work with OpenSSH. The following variables, used in the lab, are
defined as follows:

```LABHOST`- the assigned lab host that the student logs in to.`LABVM`- the virtual machine set up by the student and used for the class.


SSH Key Generation
---

1. From the `LABHOST` machine, we will connect to the `LABVM` without password by importing the ssh keys:

```bash
export LABHOSTIP=192.168.101.1
export LABVMIP=192.168.101.50
echo "LABHOSTIP=192.168.101.1" >> ~/.bashrc
echo "LABVMIP=192.168.101.50" >> ~/.bashrc
export USER=ubuntu
2. Install ssh server if needed.

```bash
sudo apt install openssh-server openssh-sftp-server
3. Generate the local keys.

```bash
ssh-keygen -t rsa -b 2048
ls -l ~/.ssh/
`Keys are created in .ssh directory.
Private key - read *(r--------)* access only for the logged in user.
Public key - read access for everyone *(r--r--r---)*


4. Copy the public key on the remote `LABVM` machine with the `ssh-copy-id` command:

```bash
ssh-copy-id ubuntu@$LABVMIP

ssh ubuntu@$LABVMIP
`This can also be done manually by copy/pasting the public key  `~/.ssh/id_pub` from `LABHOST` to `LABVM` in `~/.ssh/authorized_keys`.




sshd, ssh-agent, ssh-add, ssh-import-id and ssh-copy-id
---

**ssh-add** keeps your private keys safe.
**ssh-import-id** imports public keys from specified ssh servers and adds them
to the local **authorized_keys** file.
**ssh-copy-id** does what **ssh-import-id** does but in reverse, it copies a local
public identity to a remote server's **authorized_keys** file.

1. Check if `sshd` is running.`bash
ps -el | grep sshd
2. Check if `ssh-agent` is running, if not, start it by running `ssh-agent` command.`bash
ps -el | grep ssh-agent
3. Run `ssh-add`. This will add all your private keys into `ssh-agent`.
You should see a message similar to the following:
`Identity added: /home/$USER/.ssh/id_rsa (/home/$USER/.ssh/id_rsa).`

```bash
ssh-agent bash
ssh-add
`This starts a separate instance of the ssh-agent and runs bash inside of it


Local port forwarding
---

Specifies that the given port on the local (client) host is to be forwarded to the given
host and port on the remote side.

Imagine you're on a private network which doesn't allow connections to a specific server.
Let's say you're at work and `gnu.org` is being blocked. To get around this, one can
create a tunnel through a server which isn't on his network and thus can access the GNU
website.`bash
ssh -L 9000:gnu.org:80 user@publichost.com
`The key here is `-L` which says you're doing local port forwarding. Then it says you're
forwarding local port 9000 to gnu.org:80 via example.com server. Now, gnu.org would be
accessible on http://localhost:9000.

The local port forwarding command template looks like this:

```bash
sudo ssh -L <localPort>:<endHost>:<endDestinationPort> root@<intermediate host>
```

### Local port forwarding LAB

1. Forward port 443 on your laptop to www.gnu.org port 443 through `LABHOST`.
Some ports, typically up to 1024, will need *sudo*. Thesse are called
reserved ports. Leave this connected, while testing.`bash
# create the tunnel to LABVM
sudo ssh -L 443:www.gnu.org:443 ubuntu@$LABVMIP
# install elinks browser
sudo apt install -y elinks
# check port forward
elinks https://127.0.0.1
`Remote port forwarding
---

Specifies that the given port on the remote (server) host is to be forwarded to the
given host and port on the local side.

Let's take an example. Someone is developing a web application on his local machine,
but he wants to show it to someone outside his local network. He does not have a
public IP and NAT is not a solution for him in this case. The solution here is to do
remote port forwarding on another server that is publicly accessible.`bash
ssh -R 9000:localhost:3000 user@publichost.com
`First he needs to specify the port on which the remote server will listen, which in
this case is `9000`. Next follows `localhost` for the local machine, and the local
port `3000`.


### Remote port forwarding LAB

This exercise should be done after setting up `LABVM`.

You can use this mechanism to bypass restrictions imposed on remote hosts
such as accessing external software repositories if the firewall in front of the
remote host does not allow http traffic but your local computer has access to the
repositories

1. Install apache2 on `LABHOST` and create a `test.txt` file in`/var/www/html`folder with the contents "LABHOST"

```bash
sudo apt install -y apache2
cd /var/www/html
sudo touch test.txt
sudo sh -c 'echo LABHOST > /var/www/html/test.txt'
sudo sed -i 's/80/8080/' /etc/apache2/ports.conf
sudo systemctl restart apache2
2. Open a SSH tunnel from `LABHOST` to `LABVM` and remotely forward port `80`:

```bash
sudo ssh -R 8080:localhost:8080 ubuntu@$LABVMIP
3. From `LABVM` use `curl` to fetch the test page from `http://localhost/test.txt`:

```bash
sudo apt install -y curl net-tools
curl -v http://localhost:8080/test.txt
```

# System Initialization

In this section you will learn:
* Define and explain the GRUB2 bootloader
* Use advanced systemd commands
* Troubleshoot systemd related issues

What is GRUB2?
===

**GRUB2** is the default boot loader and manager for Ubuntu. It loads before any
operating system. Its modular components are loaded on an as-needed basis. Menu
display behavior is generally determined by settings in **/etc/default/grub**.

As the computer starts, **GRUB2** either presents a menu and awaits user input or
automatically transfers control to an operating system kernel. **GRUB2** is a
descendant of GRUB (GRand Unified Bootloader). It has been completely rewritten
to provide the user significantly increased flexibility and performance. **GRUB2**
is free software.

**GRUB2** provides the following features:
* Scripting support including conditional statements and functions
* Dynamic module loading
* Rescue mode
* Custom menus
* Themes
* Graphical boot menu support and improved splash capability
* Boot LiveCD ISO images directly from hard drive
* New configuration file structure
* Non-x86 platform support (such as PowerPC)
* Universal support for UUIDs (not just Ubuntu)

The major **GRUB2** folders include `/etc/grub.d`, which contains the main
**GRUB2** scripts, and `/boot/grub`, which contains the **GRUB2** modules
and menu file (`grub.cfg`). Configuration changes are normally made to
`/etc/default/grub` and to the custom files located in `/etc/grub.d`.
Any changes made directly to the `/boot/grub/grub.cfg` are overwritten
whenever update-grub is executed either by the user or when called
automatically by various system functions.

After editing `/etc/default/grub` or the scripts in the `/etc/grub.d` folder
the user should run `sudo update-grub` to incorporate the changes into the
**GRUB2** menu.

GRUB2 LAB
===

Grub2 Configuration
---

Make sure to run the following exercises from the `ubuntu` `LABVM`.

In this lab you will work with the `GRUB2`  boot loader.

1. See the help at the top of the file `/etc/default/grub`.`bash
info -f grub -n 'Simple configuration'
2. Test the new kernels once using Grub.
Change `GRUB_DEFAULT=0`, to `saved`, this allows a one time test of
the new kernel. After one test, on reboot it reverts to the last
saved kernel.`bash
sudo apt install -y vim
sudo vim /etc/default/grub
`Change `GRUB_DEFAULT=0`

```ini
GRUB_DEFAULT="saved"
GRUB_SAVEDEFAULT="false"
3. After testing a new kernel, you can save it as the default for the future.
Run grub-set-default on the selected menu option (0=first item).

```bash
sudo grub-set-default 0
4. On boot, usually grub menu is either not displayed or displayed for 5
seconds. You can change both settings - to always display for `X` seconds.
Set Grub timeout to 10 seconds.

Edit `/etc/default/grub`:

```ini
GRUB_TIMEOUT=10
`The menu display is controlled by this option. Common values are `menu` or `hidden`.

5. Turn on the grub menu display.`ini
GRUB_TIMEOUT_STYLE='menu'
`Console Connection
---

1. The default connection is local display. Change it to serial.`ini
GRUB_TERMINAL_INPUT="serial"
2. Add this to the boot command line.
*Note:* At beginning of the VM creation we added it to CMD_LINE as shown.

```ini
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash console=ttyS0,115200"
`At end, run sudo `update-grub` to update `/boot/grub/grub.cfg` file.

3. Change the boot menu display size.
Default is `640x480` to match old terminals. You can change this to match
your laptop (or set to auto):

```ini
GRUB_GFXMODE='auto'
4. Reboot and optionally specify the menu item.

```bash
sudo grub-reboot 0
...
sudo update-grub
`Advanced systemd Usage
===

**systemd** is a system and service manager for Linux operating systems.
It provides aggressive parallelization capabilities, uses socket and D-Bus
activation for starting services, offers on-demand starting of daemons,
keeps track of processes using Linux control groups, maintains mount and
automount points, and implements an elaborate transactional dependency-based
service control logic. It also includes a logging daemon, utilities to
control basic system configuration like the hostname, date, locale.

**systemd** maintains a list of logged-in users and running containers and
virtual machines, system accounts, runtime directories and settings, and
daemons to manage the network configuration, network time synchronization,
log forwarding, and name resolution.

**systemd** is the default init process since 15.04 (Vivid), it became the
default for the 16.04 (Xenial) LTS Release. When run as the first process
on boot (as PID 1), it acts as the init system that brings up and maintains
userspace services.

When run as a *system instance*, **systemd** interprets the configuration
file `system.conf` and the files in *system.conf.d* directories. When run
as a user instance, systemd interprets the configuration file *user.conf*
and the files in `user.conf.d` directories.

**systemd** unit files built-in to packages are found in `/lib/systemd/system`, but can be overridden by creating override files
in `/etc/systemd/system/<unitname>.service.d/` directory. The file names
have to end in `.conf` extension.

Another option is:
```shell
sudo systemctl edit <foo>
`Systemd Units
---

Units are the representation of system resources that can be managed by
systemd. They are similar to services but encompass much more. Units can
be used to abstract services, network resources, devices, filesystem mounts,
and isolated resource pools.

Units are separated into component units according to function which makes
it easy to enable, disable, or extend functionality without modifying the
core behavior of a unit.

Example Systemd Unit File
---

The following example demonstrates a service *foo*, in a file
named */lib/systemd/system/foo*. service:
```ini
[Unit]
Description=Unit that runs the foo daemon
Documentation=man:foo(1 )
[Service]
Type=forking
Environment=statedir=/var/cache/foo
ExecStartPre=/usr/bin/mkdir -p ${statedir}
ExecStart=/usr/bin/foo-daemon --arg1 "hello world" --statedir ${statedir}
[Install]
WantedBy=multi-user.target
`Troubleshooting systemd
---

To print a list of all running units, ordered by the time they took to
initialize. This information may be used to optimize boot-up times. Note
that the output might be misleading as the initialization of one service
might be slow simply because it waits for the initialization of another
service to complete.`bash
systemd-analyze blame
`To print an SVG graphic detailing which system services have been started
at what time, highlighting the time they spent on initialization:

```bash
systemd-analyze plot > plot.svg
`To enable permanent storage for logs enter:

```bash
sudo mkdir -p /var/log/journal
sudo killall -USR1 systemd-journald
`systemd LAB
===

In this lab you will use `systemctl` commands to interact with  **systemd**.
This lab will not run on the student VM. It runs only on the student machine `LABHOST`.

1. Stop the libvirt-bin service. (This will disconnect your LABVM.)

```bash
sudo systemctl stop libvirt-bin
2. View the current status of the libvirt-bin service.

```bash
systemctl status libvirt-bin
3. Start the libvirt-bin process.

```bash
sudo systemctl start libvirt-bin
4. Show the current status of the libvirt-bin service.

```bash
systemctl status libvirt-bin
5. Reconnect your VM.

```bash
virsh console ubuntu
6. Which services are CPU hogs on LABVM?

```bash
systemd-analyze blame
7. Create a directory for journalling.

```bash
sudo mkdir -p /var/log/journal
8. Find and kill systemd-journal (it will restart).

```bash
sudo killall -USR1 systemd-journal
9. Flush existing logs, then it will start using new directory.

```bash
sudo journalctl --flush
sudo journalctl --sync
`On Demand Processes
===

There are processes that you know you are going to need in the future.
However, you aren't sure precisely when you will need them, but they
need to be available at that moment. You could have them running all
the time, but you also want to conserve system resources by not having
mostly-idle processes hanging around dormant most of the time. There
are various commands that help manage processes.

**at** and **batch** read commands from standard input or a specified
file which are to be executed at a later time.

> **at** Executes commands at a specified time.
>
> **atq** Lists the user's pending jobs, unless the user is the
>         superuser and all jobs are listed. The format of the
>         output lines (one for each job) is:
>         `Job number, date, hour, queue, and username`.
> 
> **atrm** Deletes jobs, identified by their job number.
> 
> **batch** Executes commands when system load levels permit; in other
>           words, when the load average drops below 0.8, or the
>           valuespecified in the invocation of atd

**at** allows fairly complex time specifications, extending the POSIX.2
standard. Use HH:MM to run a job at a specific time of day and add AM or
PM to specify morning or evening. You can also provide a date in the
form *month-name day* with an optional year (*MMDD[CC]YY*, *MM/DD/[CC]YY*,
*DD.MM*, *[CC]YY* or *[CC]YY-MM-DD*). The date must follow the time of
day. You can also specify times such as *now + count* time-units, where
the time-units can be minutes, hours, days, or weeks.

For example, to run a job at 4pm three days from now, you would enter:

```bash
at 4pm + 3 days
`To run a job at 10:00am on July 31, you would enter:

```bash
at 10am Jul 31
`And to run a job at 1am tomorrow, you would enter:

```bash
at 1am tomorrow
```

**systemd** timers can also be used to schedule tasks. For each *.timer* file,
a matching *.service* file must exist (e.g. foo.timer and foo.service). The
*.timer* file activates and controls the *.service* file. The *.service* does
not require an `[Install]` section as it is the timer units that are enabled.
If necessary, it is possible to control a differently-named unit using the
`Unit=` option in the timer's `[Timer]` section.

### Example SystemD Timer:

The following example runs the `foo.service` weekly.`ini
/etc/systemd/system/foo.timer
[Unit]
Description=Run foo weekly

[Timer]
OnCalendar=weekly
Persistent=true

[Install]
WantedBy=timers.target
`On Demand Processes LAB
===

Make sure to run the following exercises from the `ubuntu` `LABVM`.

Install the required programs:

```bash
sudo apt install at
1. Schedule a job to run in 1 minute from now

```bash
echo touch /tmp/has_ran_from_atd | at now+1minute
2. List all acheduled jobs by `atd`:

```bash
atq
3. List all systemd timers

```bash
systemctl list-timers
4. Schedule a job with systemd to be ran in 1 minute

```bash
sudo systemd-run --on-active=1 /bin/touch /tmp/has_ran_from_systemd
`Hardware Enablement (HWE)
===

Ubuntu Hardware Enablement Stacks (HWE) are a feature aimed at providing an
Ubuntu LTS release with hardware enablement support that has been introduced
and provided in newer Ubuntu releases. These hardware enablement stacks are
incorporated into install media for select Ubuntu LTS (Long Term Support)
point releases.

The hardware enablement stacks themselves are composed of updated kernels
and graphics stacks. Brand new hardware devices are frequently released to
the public. Such hardware should always work on Ubuntu, even if it has been
released after an Ubuntu release. Hardware Enablement (HWE) is about catching
up with the newest hardware technologies.

When a new kernel is released, it is packaged for Ubuntu, tested, and made
available to Ubuntu users. This method has some disadvantages; releasing a
new kernel too quickly may introduce some bugs or issues, and may not be
suitable for the enterprise. Ubuntu has different kernels for different users.
The first is the General Availability (GA) kernel (the most stable kernel)
which does not get updated to point releases. The second is the Hardware
Enablement (HWE) kernel (the most recent kernel released). Thus, there are
both the `linux-generic` and the `linux-hwe-generic` packages.

Hardware Enablement (HWE) LAB
===

In this lab you will work with hardware enablement.
1. Go to www.ubuntu.com.
2. Check the release notes for the LTS 24.04.
   https://wiki.ubuntu.com/NobleNumbat/ReleaseNotes Linux Kernel 6.8.
   These versions are not updated until the next LTS release.
3. Check the latest release for Cosmic 18.10. (once released)
   https://wiki.ubuntu.com/CosmicCuttlefish/ReleaseNotes Review some of the
   updated packages on the page. Linux kernel 4.19 and other updated packages
   These packages are not modified in the LTS; they are available as optional
   updates.

# Storage

In this section you will learn about:
* Partitioning
* Filesystems
* Managing RAID devices with **mdadm**
* Manging the Linux Logical Volume Manager

Partitioning
===

Disk **partitioning** or disk *slicing* is the creation of one or more regions
on a hard disk or other secondary storage, so that an operating system can
manage information in each region separately. These regions are called
partitions. It is typically the first step of preparing a newly manufactured
disk, before any files or directories have been created. The disk stores the
information about the partitions' locations and sizes in an area known as the
partition table that the operating system reads before any other part of the
disk. Each partition then appears in the operating system as a distinct
"logical" disk that uses part of the actual disk. System administrators use a
program called a partition editor to create, resize, delete, and manipulate
the partitions.

Partitioning schemes
---

**MBR** (Master Boot Record) or MS-DOS partitioning scheme has been supported
since early days of MS-DOS to split the disk into maximum 4 primary partitions
and a number of logical partitions inside a single extended partition. This
partitioning scheme has the concept of an active partition which in the PC
architecture houses the system boot loader (i.e. GRUB2).

**UNIX disk slices** are supported by FreeBSD and Solaris and are similar to
the MS-DOS partition table scheme except they are not limited by the number
of partitions they can hold.

**GPT** (GUID Partition Table) is a part of the Unified Extensible Firmware
Interface (UEFI) standard for the layout of the partition table on a physical
hard disk. Linux and many other operating systems now support this standard.

Partitioning Tools
---

* parted - the swiss-army knife of partitioning tools
* fdisk - old-school command line partition manager
* cfdisk - menu dirven alternative to fdisk
* sfdisk - scriptable fdisk
* gdisk - fdisk able to work with GPT

Partitioning LAB
===

1. Use `fdisk` to list the partition tables on `/dev/vdb` on `LABVM`:

```bash
sudo fdisk -l /dev/vdb
3. Use `fdisk` to create a primary partition on `/dev/vdb` on `LABVM`:

```bash
sudo fdisk /dev/vdb
`This will open an interactive shell in fdisk:

Print the partition table with `p`, should be empty:

```bash
Command (m for help): p
Disk /dev/vdb: 192.5 KiB, 197120 bytes, 385 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x18d30887
`Create a new partition with `n`:
```bash
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1):
First sector (1-384, default 1):
Last sector, +sectors or +size{K,M,G,T,P} (1-384, default 384):

Created a new partition 1 of type 'Linux' and of size 192 KiB.
`Print the partition table again:
```bash
Command (m for help): p
Disk /dev/vdb: 192.5 KiB, 197120 bytes, 385 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x18d30887

Device     Boot Start   End Sectors  Size Id Type
/dev/vdb1           1   384     384  192K 83 Linux
`Finally, make the changes persistent with `w`:
```bash
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```
*Note:* fisk will create a partition table by default


4. Clean up `/dev/vdb`:

```bash
sudo dd if=/dev/zero of=/dev/vdb bs=1M count=10
`This will erase the partition table of the device `/dev/vdb`. Make sure you are running this command on the correct machine and the correct device.

RAID
===

RAID (redundant arrays of independent disks) is a method of using multiple
hard drives to act as one. There are many purposes of RAID:
1. Span a filesystem across multiple disk drives.
2. Improve performance by striping data across multiple drives.
3. Prevent data loss in case of drive failure.

RAID allows systems to keep functioning even if some parts fail. A RAID
combines physical disks into one logical disk for the purpose of redundancy.
Data is distributed across the drives. The RAID levels balance reliability,
availability, performance, and capacity in different configurations
depending on need. RAID levels greater than RAID 0 provide protection against
unrecoverable sector read errors, as well as against failures of whole
physical drives.

The most commonly used RAID configurations are:
* **RAID 0** - is also known as a stripe set or striped volume because it
  distributes data evenly across two or more disks without parity information,
  redundancy, or fault tolerance. With this configuration, if one drive fails,
  the entire array fails and the data is lost. This level is used for
  applications that require speed at the expence of reliability.
* **RAID 1** - creates an exact copy or mirror data set on two or more disks.
  Data is mirrored on all disks in the array with no parity, striping, or
  spanning across multiple disks. The capacity of the array is that of the
  smallest disk. The configuration is used when read performance and
  reliability outweigh write performance or capacity.
* **RAID 5** - requires 3 disks and uses block-level striping with distributed
  parity requiring all drives in the array but one to function. If a drive
  fails, reads can be calculated from the distributed parity without lost data.
* **RAID 6** - requires 4 disks and uses block-level striping with distributed
  double parity. This configuration can tollerate the loss of at most two
  drives. If a drive fails, reads can be calculated from the distributed parity
  without lost data or loss of redundancy.

There are three ways to create a RAID:
1. **Hardware RAID**: utilizes a special hardware controller to manage the RAID.
   Hardware RAID is generally faster, and does not place a load on the CPU.
   These controllers typically have a dedicated processor and RAM on the
   controller for processing the RAID algorithms. The on-disk metadata and
   processing is hidden from the operating system by the controller. In order
   to utilize hardware RAID the OS must have support for the hardware RAID
   controller. Additionally controller specific monitoring software may also
   be desired. Ubuntu has drivers for many such controllers in the kernel, and
   has some support for monitoring available in the archives. If monitoring
   software for a controller is not available in the archives users should
   contacts the vendor.
2. **Softare RAID**: Utilizes the main CPU and RAM for all calculations used in
   the RAID array. The metadata used to manage the array is placed on the block
   devices in the array, and is managed by the kernel and **mdadm**. RAID
   monitoring is supported by **mdadm** run with the `--monitor` flag.
3. **FakeRaid** aka *Motherboard RAID* is used because a hardware RAID is
   expensive. It is a version of software RAID that is configured by the
   motherboard bios or firmware. As such, fakeraids can typically be used for
   booting from grub. However, the metadata used in fakeraid is dependent on
   the motherboard manufacturer, and may not be fully supported by the kernel
   and **mdadm**. As such, your mileage may vary when using  *fakeraid* with
   Ubuntu. The recommendation is to use mdadm software raid unless absolutely
   necessary.

Managing a software RAID
---

### mdadm

**mdadm** is the standard RAID management tool. It is available in the Ubuntu
distribution.  **mdadm** has 7 major modes of operation:

#### Assemble

Assemble the parts of a previously created array into an active array.
Components can be specified or searched for. mdadm checks that the components
do form a true array, and can, on request, adjust superblock information so
as to assemble a faul ty array.

#### Build

Build a legacy array without per-device superblocks.

#### Create

Create a new array with pre-device superblocks.

#### Manage

Used for doing things to specific components of an array such as adding new
spares and removing faulty devices.

#### Misc

Allows operations on independent devices such as examine MD superblocks,
erasing old superblocks and stopping active arrays.

#### Follow or Monitor

Monitor one or more md devices and configure event notification and actions
for the array. This is only meaningful for RAID 1, 4, 5, 6 or multipath
arrays. RAID 0 or linear never have missing, spare, or failed drives, so
there is nothing to monitor.

#### Grow

Grow (or shrink) an array, or otherwise reshape it in someway. Currently
supported growth options including changing the active size of component
devices in RAID level 1/4/5/6 and changing the number of active devices in
RAID1.

Software RAID Drive Failures
---

There are two kinds of failure with RAID systems: failures that reduce the
resilience and failures that prevent the RAID device from operating. If a
single disk fails, the RAID will continue operating. When enough component
devices fail the RAID device will stop working.

To recover a broken RAID:

Preserve the information in the RAID superblocks.`bash
mdadm --examine /dev/vd[*]1 >> raid.status
```
`raid.status` will contain a listing of the RAID devices.

If the assemble fails, check the logs using dmesg.`bash
dmesg | grep vd
`Use `mdadm --examine` on all devices and check their event count.`bash
mdadm --examine /dev/vd[a-z] | egrep 'Event| /dev/sd'
`One reason for failure is that the event count of the devices does not
match so `mdadm` won't assemble the array automatically. If the count
differs by less than 50, the data is most likely OK.

Try to assemble using `--force.`

```bash
mdadm --assemble --force /dev/mdX <list of devices>
`The `/proc/mdstat` file shows a snapshot of the kernel's RAID/md state.`bash
$ cat /proc/mdstat
Personalities : [raid1] [raid6] [raid5] [raid4]
md_d0 : active raid5 vde1[0] vdf1[4] vdb1[5] vdd1[2] vdc1[1]
  1250241792 blocks super 1.2 level 5, 64k chunk, algorithm 2 [5/5] [UUUUU]
  bitmap: 0/10 pages [0KB], 1 6384KB chunk

unused devices: <none>
```

> *Note:* Visit https://raid.wiki.kernel.org/index.php/Mdstat for examples and
> explanations.


**Personalities**: what RAID level the kernel currently supports (changed by
either changing the raid modules or recompiling the kernel). Possible
personalities include:
*[raid0] [raid1] [raid4] [raid5] [raid6] [linear] [multipath] [faulty]*.
*[faulty]* is a diagnostic personality; it does not mean there is a problem
with the array.

**md_d0**: means we're looking at the device `/dev/md_d0`. It is active or
'started'. An inactive array is usually faulty. Stopped arrays aren't visible
here. It is a raid5 array and the component devices are: `/dev/vde1` is device
*0*, `/dev/vdf1` is device *4*, `/dev/vdb1` is device *5*, `/dev/vdd1` is device
*2*, `/dev/vdc1` is device *1*. The order in which the devices appear in this
line means nothing. The raid role numbers [#] following each device indicates
its role, or function, within the raid set. Devices with "n" or higher are spare
disks. 0,1, . . ,n-1 are for the working array.

> *Note:* There is no device 3. That device will have failed and been replaced by
> device 5.

To identify spare devices, first look for the #/# value on a line. The first number
is the number of a complete raid device such as "n". A failed device will be marked
with (F) after the [#]. The spare that replaces this device will be the device with
the lowest role number *n* or higher that is not marked (F). Once the resync operation
is complete, the device's role numbers are swapped.

**[n/m]** means that ideally the array would have  *n* devices however, currently,  *m*
devices are in use. Obviously when m >= n then things are good.

**[UUUUU]** represents the status of each device, either *U* for up or *_* for down.

**Bitmap**: describes the state. There are 0 of 10 pages allocated in the in-memory
bitmap.

RAID LAB
===

In this lab we will create a RAID on `LABVM` in the following format:
Linux SW RAID 5 on drives vda-e (5 drives, 4 used, 1 is spare/hot standby.)
The boot/os is installed on sda1.

1. Install software needed for this lab.`bash
sudo apt install mdadm parted
2. Format all disks as gpt.

```bash
sudo parted /dev/vdb mklabel gpt
sudo parted /dev/vdc mklabel gpt
sudo parted /dev/vdd mklabel gpt
sudo parted /dev/vde mklabel gpt
sudo parted /dev/vdf mklabel gpt
3. Create the partition on drive vdb and then clone it to the other 4 drives.
   Cloning saves partition information on all of the drives.

```bash
sudo parted -a optimal /dev/vdb mkpart primary ext4 1 100%
sudo parted -a optimal /dev/vdc mkpart primary ext4 1 100%
sudo parted -a optimal /dev/vdd mkpart primary ext4 1 100%
sudo parted -a optimal /dev/vde mkpart primary ext4 1 100%
sudo parted -a optimal /dev/vdf mkpart primary ext4 1 100%
5. Create the array with 5 drives. One drive is treated as a spare.

```bash
sudo mdadm --create --verbose /dev/md0 --level=5 \
--raid-devices=5 /dev/vd[bcdef]1
6. Watch and wait for the RAID creation status. The creation might take a some time.

```bash
watch cat /proc/mdstat
7. On completion check the RAID status.

```bash
cat /proc/mdstat
8. Examine the RAID disk with the mdadm command and look for events.

```bash
sudo mdadm --examine /dev/vd[b-f]1
9. Check for errors in RAID creation with dmesg.

```bash
dmesg | grep vd
10. More tuning may be needed to complete the RAID. Lookup other RAID commands using mdadm --help:

```bash
man pages
`Removing a RAID
---

Removing a RAID is difficult, but useful to know. To remove a RAID:

1. Stop the RAID.`bash
sudo mdadm --stop /dev/md0
2. Zero out the superblocks on the drives that were used.

```bash
sudo mdadm --zero-superblock /dev/vdb1
sudo mdadm --zero-superblock /dev/vdc1
sudo mdadm --zero-superblock /dev/vdd1
sudo mdadm --zero-superblock /dev/vde1
sudo mdadm --zero-superblock /dev/vdf1
3. Remove the volume definition from `mdadm.conf`.`bash
sudo rm /etc/mdadm/mdadm.conf
`Advanced LVM
===

![Linux Logical Volume  Manager](./images/lvm.png)

LVM stands for Logical Volume Manager. It is more flexible than the traditional
method of partitioning a disk into one or more segments and formatting each
partition with a filesystem. At least one volume group is required to use LVM.
There must be one or more physical volumes (PVs) or hard disks which host a
volume group.

Logical volumes are similar to partitions on disks. They have names rather
than numbers. They can span across multiple disks and they do not have to be
physically contiguous.

> *Note:* It is a good practice to leave some space unused in the volume groups.
> If needed you can enlarge one or more logical volumes later, it also helps
> with moving data around if you need to reorganize the volume groups later.

LVM can work on top of MD to benefit from the maturity of the MD layer though
they do overlap in certain areas.

When configuring LVM on a host, the first thing you have to do is allocate
storage and dedicate it to LVM. It is a best practice to configure PVs directly
on the physical disks:

```bash
pvcreate /dev/vdx
`Once you have one ore more PVs created you can group them into a volume group.`bash
vgcreate my_vg /dev/vdx
`You can then create a logical volume by specifying its size in either storage
units (bytes, KB, MB, GB...), extents or percentage (20%FREE). You can specify
volume type (linear, striped, mirror, raid, cache) and additional parameters
for each specific type during volume creation.`bash
lvcreate --name my_lv --size 10GB my_vg
`When you get close to running out of space in a filesystem you need to allocate
it more space, you do that by first increasing the underlying logical volume.
If you don't have enough free extents in the current volume group you might
also need to add additional PVs.`bash
vgextend my_vg /dev/vdy
lvextend -L +20GB /dev/my_vg/my_lv
`Once you have enough PVs (2 or more) in your volume group you might decide to
increase data protection by mirroring your volume.`bash
lvconvert -m +1 my_vg/my_lv (from linear to mirror)
lvconvert -m 0 my_vg/my_lv (from mirror back to linear)
`You might need to move data from one device to another such as when replacing
an old disk array. In that situation you can use the mv command and create
another filesystem or perform the change online from the LVM layer using the
**pvmove** command.`bash
pvmove /dev/vdy
pvmove -n my_vg/my_lv
`Advanced LVM LAB
===

Install lvm:
```bash
sudo apt install lvm2
1. Create two PVs on `/dev/vdb` and `/dev/vdc`

```bash
sudo pvcreate /dev/vd[bc]1
2. Create a volume group called `my_vg` on top of the two volumes`bash
sudo vgcreate my_vg /dev/vd[bc]1
3. Create a volume called `my_lv`, 1GB in size in the new VG`bash
sudo lvcreate -n my_lv -L 1GB my_vg
4. Convert the volume into a mirrored volume

```bash
sudo lvconvert -m +1 my_vg/my_lv
5. Convert the volume back into a regular volume

```bash
sudo lvconvert -m 0 my_vg/my_lv
6. Check on which PV the volume resides:s

> Multiple methods

```bash
sudo lvs -o +devices
sudo pvdisplay -m
sudo lsblk
sudo lvs --segments /dev/my_vg/my_lv
7. Move the volume to the other PV

```bash
sudo pvmove -n my_lv /dev/vdb1 /dev/vdc1
8. Remove the newly freed PV from the volume group

```bash
sudo vgreduce my_vg /dev/vdb1
9. Remove the logical volume and the volume group

```bash
sudo vgchange -an my_vg
sudo lvremove my_vg/my_lv
sudo vgremove my_vg
sudo pvremove /dev/vd[bc]1
`Device Mapper Multipathing
===

Device mapper multipathing (**DM-Multipath**) allows you to configure multiple
I/O paths between server nodes and storage arrays into a single device. These
I/O paths are connections that can include separate cables, switches, and
controllers. Multipathing aggregates the I/O paths, creating a new device that
consists of the aggregated paths.

**DM-Multipath** can be used to provide:
1. Redundancy
   > **DM-Multipath** can provide failover in an active/passive configuration.
   > In an active/passive configuration, only half the paths are used at any
   > time for I/O. If any element of an I/O path (the cable, switch, or
   > controller) fails, DM-Multipath switches to an alternate path.
2. Improved Performance
   > **DM-Multipath** can be configured in active/active mode, where I/O is
   > spread over the paths in a round-robin fashion. In some configurations,
   > **DM-Multipath** can detect loading on the I/O paths and dynamically
   > re-balance the load.

By default, **DM-Multipath** includes support for the most common storage arrays
that support  **DM-Multipath**. The supported devices can be found in the
`multipath.conf.defaults` file. If your storage array supports  **DM-Multipath** and
is not configured by default in this file, you may need to add them to the
**DM-Multipath** configuration file, `multipath.conf`.

Without **DM-Multipath**, each path from a server node to a storage controller
is treated by the system as a separate device, even when the I/O path connects
the same server node to the same storage controller. **DM-Multipath** provides
a way of organizing the I/O paths logically, by creating a single multipath
device on top of the underlying devices.

Device Mapper Multipathing LAB
===

1. Install the multipath package`bash
sudo apt install multipath-tools
2. Run multipathd to discover all paths and aggregate them

```bash
sudo multipath -r
3. List the discovered devices.

```bash
sudo multipath -ll
```

# Advanced Filesystem Concepts

In this section you will learn to:
* Use setuid, setgid, sticky bits
* Explain the difference between file attributes and extended attributes
* Explain inodes and superblocks
* Implement an XFS filesystem
* Rescue a filesystem using fsck
* Resize of filesystem using specific tools

The SETUID and SETGID Bits
===

A process has two different ways to classify its UID: the "real " UID and the
"effective" UID. The real UID belongs to the user account that starts the
process. The effective UID is that of the user account whose privileges attach
to the process. For the most part, the real and effective UIDs are the same.
The setuid and setgid permissions allow you to alter that behavior.

**setuid** (set user ID) and **setgid** (set group ID) are settings that enable
users to run an executable with the permissions of that executable's owner or
group. They allow users to run programs with elevated permissions
in order to perform a specific task.

**setuid** sets the effective user ID of the calling process. The setuid is a
permission bit, that allows the users to execute a program with the permissions
of its owner. The bit is not set by default.

**setgid** is similar to *setuid*. It sets the effective group ID of the calling
process. Also, when the *setgid* bit is set on a directory, new subfolders and
files within that directory will inherit the group of the owner of the directory.

When set on a folder, new files and folders inherit the parent directory group id.
The *setuid* and *setgid* are set with the **chmod** command. When the *setuid*
bit is set as part of a directory's permissions in Ubuntu, it has no effect.

To view whether a file has setuid and setgid execute the following command:
```bash
ls -l
-rwSrw-r-- 1 michelle michelle 0 Oct 1 0 1 4:30 file1
-rw-rwsr-- 1 michelle michelle 0 Oct 1 0 1 4:30 file2
`In the output above the `S` in the user permissions for file1 represents the
*setuid* and the `s` in the group permision field for file2 represents the
*setgid*. There is a difference between `S` which indicates just the setuid
bit and `s` which indicates *setgid* bit and execute `x` (for that position)
in the permissions set.

Other files that might have these bits set: *at*, *chage*, *chsh*, *crontab*,
*sudo*, *ping*, *mount*.

To set the setuid:
```bash
chmod u+s /path/filename
`To set the setgid:
```bash
chmod g+s /path/filename
`To remove the permissions:
```bash
chmod u-s /path/filename
chmod g-s /path/filename
`In octal mode:
---

To set the setuid in the octal form, place a `4` in front of the three permission bits.`bash
chmod 4777 file1
`To set the setgid bit, place `2` in front of the permission bits:
```bash
chmod 2764 file2
`To set both, place a `6` in front of the permission bits:
```bash
chmod 6764 file3
`To find all files on the system with *suid* or *sgid* bits set:
```bash
sudo find / -type f -perm /6000 -exec ls -l {} \;
`Use of setuid/setgid in the VM - LAB
---

1. Check current permissions of passwd command.`bash
ls -lt /usr/bin/passwd
2. Remove setuid from passwd.

```bash
sudo chmod u-s /usr/bin/passwd
3. Check permissions again.

```bash
ls -lt /usr/bin/passwd
4. Run passwd command.

```bash
passwd
```
> **passwd** will fail because */etc/passwd* and */etc/shadow* (the files
> the command tries to acccess) are protected by different UNIX filesystem
> permissions

5. Restore setuid permissions and try again`bash
sudo chmod u+s /usr/bin/passwd
ls -lt /usr/bin/passwd
passwd
`Sticky Bits
===

The sticky bit applies only to directories. It is typically used on publicly
writeable directories (i.e. /tmp). When the sticky bit is applied to a directory,
users are prevented from deleting or renaming any files that they do not own.

To add or remove the sticky bit, use chmod with the `t` flag:
```bash
chmod +t <directoryname>
chmod -t <directoryname>
`The status of the sticky bit is shown in the  **other** execute field, when
viewing the long output of the `ls` command. `t` or `T` in the **other**
execute field indicates the sticky bit is set, anything else indicates it
is not.`bash
drwxrwxr-t 2 michelle michelle 4096 Oct 7 1 8:26 mydir2
`The sticky bit is useful on directories that are world-writable, such as `/tmp`.
In these directories, anyone can create a file, so the directory needs to be
world-writable. But that would mean anyone could delete files, including those
that don' t belong to them. When a directory has the sticky bit, only the
owner of a file has the permission to delete it.

In a directory with permissions `rwxrwx---`, all members of the group can
create and delete files. Any member of the group can delete any file even if
it belongs to another user. If the permissions are `rwxrwx--T` (`t` means that
the `x` bit is *set* and `T` means that the `x` bit is *clear*), then any
member of the group can create a file, and members of the group can delete
files but only their own files.

To set the sticky bit concurrently with suid and sgid use:
```bash
chmod 7755 directory_name
`Here is a table of the things you learned so far:

pattern | &nbsp; | description
--- | --- | ---
-S-- | &nbsp; | SUID is set, but user (owner) execute permission is not set.
-s-- | &nbsp; | SUID and user execute persmission are set both.
--S- | &nbsp; | SGID is set, but group execute permission is not set.
--s- | &nbsp; | SGID and group execute permission are set both.
---T | &nbsp; | Sticky bit is set, bot other execute permission is not set.
---t | &nbsp; | Sticky bit and other execute permission are both set.

Sticky Bit Lab
---

Run the following commands on the `LABVM` machine.

1. Make a directory and set world-writable permissions (777). Next steps will be
   executed inside that directory.`bash
mkdir teststick
chmod 777 teststick
```
> In this directory anyone can add/delete any file

2. Login as the superuser (ubuntu) and create 3 files called  *sbFile1*, *sbFile2*,
   and *sbFile3*.`bash
for ((i=1;i<=3;i++)) ; do
  touch teststick/sbFile${i}
done
3. Create a normal user called **cm**.

```bash
sudo adduser cm
4. Login as that user and create 3 files called  *sbUserFile1*, *sbUserFile2*, and
   *sbUserFile3*.

```bash
su cm
for ((i=1;i<=3;i++)) ; do
  touch teststick/sbUserFile${i}
done
5. As the current user, delete the **ubuntu** user's file.

```bash
rm teststick/sbFile1
6. As **ubuntu**, remove the current user's file.

```bash
exit
rm teststick/sbUserFile1
7. Now set sticky bit on the directory.

```bash
chmod +t teststick
8. Still as **ubuntu**, try to delete or move **sbUserFile2**.

```bash
rm teststick/sbUserFile2
mv teststick/sbUserFile3 teststick/sbUserFile4
```
> Works since *ubuntu* is superuser (the owner of the directory)

9. As the  **cm** user try delete or move one of the  **ubuntu** user files.`bash
su cm
rm teststick/sbFile2
mv teststick/sbFile3 teststick/sbFile4
exit
`SETUID/SETGID and Sticky Bits Exercises
===

> In this lab you will experiment with  **setuid**, **setgid**, and **sticky** bits.

1. Create a file called `file1` using the touch command.`bash
touch file1
2. Change the setuid bit for `file1`.`bash
chmod 4555 file1
3. Check the permissions to verify that the change was made.

```bash
ls -l
4. Is the SUID bit upper case or lower case and why?
> Lower case because the execute bit is also set (5 = `u=r-x`)

5. Set the setgid bit to 2.`bash
chmod 2555 file1
6. Change the sticky bit to 1.

```bash
chmod 1555 file1
`Filesystem Basics
===

Inodes
---

The Linux filesystem is comprised of a pool of data blocks for storing data and
the database that manages the data pool.

An **inode** (index node) is a data structure found in many Unix file systems.
Each  **inode** stores all the information about a file system object (type,
permissions, and where the data pool file resides), except data content and
file name. Inodes are numbered and listed in the inode table.

Both files and directories are implemented as inodes. A directory inode contains
a list of file names and the links to their corresponding inodes. The figure below
illustrates a directory structure.
![Directory Structure](./images/folders.png)

The next figure shows that structure represented using inodes.
![Inodes](./images/inodes.png)

*To read the drawing:*
> Start at inode `2` which is the root inode. It points to `dir_1` in the data
> pool which corresponds to inode `10`. In the inode table you would find `10`
> - a directory inode - which points to the second block in the diagram. `File_a`
> corresponds to inode `12`. In the inode table `12` corresponds to a file inode
> which points to the content of `File_a`. The kernel can open the file by
> folowing the link.
>
> All directory inodes (except the root) have entries for the current directory
> (`.`) and the parent directory (`..`).
>
> *Links*=link count which is the total number of directory entries across all
> directories that point to an inode. (Ex. `10` is listed twice.)
>
> The inode bitmap records which entries in the inode table are in use.

To display a list of every directory on the filesystem prefixed with the number
of files (and subdirectories) in that directory (the directory with the largest
number of files will be at the bottom):
```bash
find / -xdev -printf '%h\n' | sort | uniq -c | sort -k 1 -n
`Superblock
---

The superblock is essentially file system metadata and defines the file system
type, size, status, and information about other metadata structures (metadata of
metadata).

The superblock is so critical to the filesystem that multiple backups are created
when the filesystem is created. Without the superblock, the filesystem cannot be
mounted.

The superblock is a very "high level" metadata structure for the file system
which records information such as block counts, inode counts, supported features,
maintenance information, location of the filesystem journal, when the filesystem
was created, and more.

If the superblock of a partition, /var, becomes corrupt then the file system
(/var) cannot be mounted by the operating system. You would run fsck to select
a backup copy of the superblock and attempt to recover the file system. The first
backup is stored at a 1 block offset from the start of the partition in case a
manual recovery is necessary.

Exercises
---

> *Note:* You will run these exercisese as part of the lab so this is here just
> for reference.

Type the following to view information about the superblock backups:
```bash
sudo dumpe2fs /dev/<partition> | grep -i superblock
`tune2fs
---

**tune2fs** allows the system administrator to adjust various tunable filesystem
parameters on Linux ext2, ext3, or ext4 filesystems. The current values of these
options can be displayed by using the `-l` option to *tune2fs* program, or by
using the *dumpe2fs* program.

Use the `-l` parameter to dipslay the name of the filesystem superblock:
```bash
sudo tune2fs -l /dev/vdb1 | grep volume
`Use the capital `-L` parameter to change the volume-label name:
```bash
sudo tune2fs -L myhome /dev/vdb1
`Use **tune2s** to display the filesystem superblock information:
```bash
sudo tune2fs -l /dev/vdc1
`Filesystem Lab
===

Run the following commands on the `LABVM` machine.

> In this lab you will work with `tune2fs to` view the superblock.

> *Note:* You need to use **sudo** to run **tune2fs** and some **virt-install** commands.

1. Run the command to view the inode info.`bash
df -i
2. Set the mount count to 2 max. This specifies how many mounts will execute
   before running  **fsck**.

```bash
sudo dd if=/dev/zero of=/dev/vdb bs=1M count=10
sudo dd if=/dev/zero of=/dev/vdc bs=1M count=10
sudo dd if=/dev/zero of=/dev/vdd bs=1M count=10
sudo dd if=/dev/zero of=/dev/vde bs=1M count=10
sudo dd if=/dev/zero of=/dev/vdf bs=1M count=10
sudo parted /dev/vdb mklabel gpt
sudo parted -a optimal /dev/vdb mkpart primary ext4 1 100%
sudo mkfs.ext4 /dev/vdb1
sudo tune2fs -c 2 /dev/vdb1
3. Run `tune2fs -i`
   > This command specifies how often to run it, (after days/weeks/months).`bash
sudo tune2fs -i 2 /dev/vdb1
4. Run `tune2fs -l`

```bash
sudo tune2fs -l /dev/vdb1
```
> This allows you to see all information on a drive, including superblock
> information. An example is shown below:
> ```
> Inode count: 524288
> Free blocks: 1725279
> Free inodes: 463362
> Mount count: 2
> Maximum mount count: -1
> Last checked: Sun Nov 20 23:12:52 2016
> First inode: 11
> Inode size: 256
> `5. The default mount options can be also set in the filesystem superblock using
   the  **tune2fs** utility.`bash
sudo tune2fs -o acl /dev/vdb1
dumpe2fs -h /dev/vdb1 | grep Default
```
> This enables POSIX ACLs, check that `acl` shows up in the `Default mount options`

XFS
===

XFS is a journaled 64-bit file system. An XFS filesystem can reside on a regular
disk partition or on a logical volume. An XFS filesystem has up to three parts:

* A *data section* - contains the filesystem metadata (inodes, directories,
  indirect blocks), the user file data for ordinary files, and the log area (if
  it is internal to the data section). The data section is divided into a number
  of al location groups which controls parallelism in file and block allocation.
  Each allocation group contains several data structures. The first sector
  contains the **superblock**. Other allocation groups contain information for
  block and inode allocation within the allocation group and data structures
  to locate free blocks and inodes.
* A *log section* - used to store changes to filesystem metadata while the
  filesystem is running until those changes are committed to the data section.
  It is written sequentially during normal operation and is accessed read only
  during mount. When mounting a filesystem after a crash, the log is read to
  complete operations that were in progress at the time of the crash.
* A *real-time section* - used to store the data of real-time files. It is
  divided into a number of extents of fixed size (specified at mkfs.xfs time).
  Each file in the real-time section has an extent size that is a multiple of
  the real-time section extent size.

Each XFS filesystem is labeled with an Universal Unique Identifier (UUID). The
UUID is stored in every allocation group header and is used to help distinguish
one XFS filesystem from another.

`xfsdump` and `xfsrestore` are used for making copies of XFS filesystems.

An extent is a contiguous area of storage reserved for a file and is represented
as a range. Files can consist of zero or more extents; one file fragment requires
one extent. Extent allocation results in much less file fragmentation. Extent
based file systems eliminate most of the metadata overhead of large files and
reduce exposure to filesystem corruption.

Dependencies
---

The **xfs** module must be loaded, to mount xfs volumes. This happens automatically
in Ubuntu. The following packages should be installed:
* **ubuntu-minimal** - a minimal install disk for Ubuntu. It will fetch all
  packages from an external repository (such as the official Ubuntu mirrors)
  during installation.
* **xfsprogs** - Utilities to create, repair and manage xfs-filesystems
* **xfsdump** - Utilities for backing up and restoring xfs filesystem
* **xfs_fsr** - The defragmentation and optimization application
* Newer GRUB (as shipped with Ubuntu) with XFS Stage1.5 or LILO as bootloader.

XFS Features
---

* Supports extended attributes and quotas
* Can do direct (non-cached) filesystem I/O using DMA (Direct Memory Access)
* Allows applications to reserve bandwidth to disk for a specified time due
  to guaranteed-rate I/O
* Can freeze filesystem I/O during LVM snapshot using `xfs_freeze`
* Provides online defragmentation (`xfs_fsr`) and online growth (`xfs_growfs`)
* Provides native backup/restore using `xfsdump` and `xfsrestore`

Tuning XFS
---

* Most RAID controllers supply striping information that XFS can use to
  self-optimize. For those that don't, it can be set using `mkfs` options *-sw/-su*.
* The amount of journal information in RAM can improve performance. Add  *logbufs=8*
  to fstab or mount.xfs.
* Use variable block size feature: _4KB_ for a few million directory entries,
  _16KB_ or _64KB_ for larger (>10 million).
* Distribute large numbers of files in different directories to maximize concurrency
  between allocation groups

XFS Lab
===

Run the following commands on the `LABVM` machine.

1. Install the needed xfs software.`bash
sudo apt install -y xfsprogs xfsdump
2. Create an ext4 filesystem and mount point and mount them.

```bash
sudo dd if=/dev/zero of=/dev/vdb bs=1M count=10
sudo parted /dev/vdb mklabel gpt
sudo parted -a optimal /dev/vdb mkpart primary ext4 1 100%
sudo mkfs -t ext4 /dev/vdb1
sudo mkdir /media/ext4mnt
sudo mount /dev/vdb1 /media/ext4mnt
3. Create an xfs filesystem and mount point and mount them. Use -f to overwrite:

```bash
sudo dd if=/dev/zero of=/dev/vdc bs=1M count=10
sudo parted /dev/vdc mklabel gpt
sudo parted -a optimal /dev/vdc mkpart primary ext4 1 100%
sudo mkfs.xfs -L "datavol " -f /dev/vdc1
sudo mkdir /media/xfsmnt
sudo mount -t xfs /dev/vdc1 /media/xfsmnt
4. View the mounted drives.

```bash
mount | grep vd
```

5. Add partitions to /etc/fstab for automount as shown:

   ```fstab
   /dev/vdc1 /media/xfsmnt xfs rw,relatime,attr2,inode64,noquota 0 0
   ```
```bash
sudo vi /etc/fstab
```
> Add the lines.

6. Unmount the already mounted partitions and verify that they are unmounted.
   View the mounts again. What do you see and why? *____________*

```bash
sudo umount /media/ext4mnt
sudo umount /media/xfsmnt
mount | grep vd
7. Remount the disks using fstab and verify that they are mounted.

```bash
sudo mount -a
mount | grep vdc
`Other XFS Commands
---

> *Note:* you need to run the following commands as `root` or using `sudo`

1. Freeze the filesystem IO when taking a snapshot.`bash
# freeze
sudo xfs_freeze -f /media/xfsmnt
# unfreeze
sudo xfs_freeze -u /media/xfsmnt
2. Defrag the drive /dev/vdc1.

```bash
sudo xfs_fsr /dev/vdc1
3. Your filesystem may be full. Grow the filesystem.

```bash
sudo xfs_growfs /media/xfsmnt
4. Backup and restore the XFS filesystem.

```bash
sudo xfsdump - /media/xfsmnt > /tmp/xfs.dump
sudo xfsrestore - /media/xfsmnt < /tmp/xfs.dump
`Tuning commands
---

> *Note:* you need to run the following commands as `root` or using `sudo`

1. Use the man pages to search for `mkfs.xfs` and the options *-sunit*, *-su*
   and *-sw*.`bash
mkfs.xfs -h
man 5 xfs
2. Specify stripe unit and stripe width in terms of 512 byte blocks

```bash
umount /dev/vdc1
sudo mkfs.xfs -d sunit=128 -d swidth=384 /dev/vdc1 -f
3. Improve logging with *logbufs=8*
```bash
sudo vim /etc/fstab
```
> Add `logbufs=8` to the xfs mount options and remount:
```bash
sudo mount -o remount /mnt/xfsmnt
```

**Cleanup**

```bash
sudo umount /media/xfsmnt
sudo umount /media/ext4mnt
```

# ZFS

In this section you will learn to:
* Define ZFS
* Explain ZFS virtual devices
* Configure and tune ZFS

ZFS Overview
===

ZFS file system is a combination volume manager (like LVM) and a filesystem
(like ext4, xfs, or btrfs). The storage capacity makes it useful for large
servers. It supports 128-bit block addresses. ZFS is the preferred filesystem
for LXD containers because of its flexibility and fault tolerant features.
Multiple drives can be pooled into a larger drive. ZFS is only supported
on 64 bit architectures.

To install ZFS, use:
```bash
sudo apt install zfsutils-linux
`Features
* Simplified administration with `zpool` and `zfs` commands
* Snapshots (retains a data copy as of a specific point in time)
* Copy-on-write (COW) cloning (write-able copies of snapshots that store only
  changes from the original)
* Continuous integrity checking against data corruption
* Automatic repair
* Efficient data compression
* Deduplication (eliminates duplicate copies of repeating data)
* Partitioning is replaced by ZFS storage pools that can span multiple disks

ZFS Architecture
===

![ZFS Architecture](./images/zfs-architecture.png)

ZFS Virtual Devices (ZFS VDEVs)
---

A **VDEV** is a _meta-device_ that can represent one or more devices. ZFS supports 7
different types of  _VDEV_:
* File - a pre-allocated file
* Physical Drive (HDD, SDD, PCIe NVME, etc)
* Mirror - a standard RAID1 mirror
* ZFS software raidz1, raidz2, raidz3 'distributed' parity based RAID
* Hot Spare - hot spare for ZFS software raid.
* Cache - a device for level 2 adaptive read cache (ZFS L2ARC)
* Log - ZFS Intent Log (ZFS ZIL)
* VDEVS are dynamically striped by ZFS. A device can be added to a VDEV, but cannot
  be removed from it.

ZFS Pools
---

ZFS filesystems are built on top of virtual storage pools called  **zpools**. A *zpool*
is constructed of virtual devices (*VDEVs*), which are themselves constructed of block
devices: files, hard drive partitions, or entire drives, with the last being the
recommended usage. Block devices within a *vdev* may be configured in different ways,
depending on needs and space available: non-redundantly (similar to RAID0), as a mirror
(RAID1) of two or more devices, and others. Besides standard storage, devices can be
designated as volatile read cache (ARC), nonvolatile write cache, or as a spare disk
for use only in the case of a failure. Finally, when mirroring, block devices can be
grouped according to physical chassis, so that the filesystem can continue to work in
the face of a failure of an entire chassis. One or more ZFS file systems can be created
from a ZFS pool.

In the following example, a pool named  **pool-test** is created from 3 physical drives:
```bash
sudo zpool create pool-test /dev/vdb /dev/vdc /dev/vdd
`Striping is performed dynamically, so this creates a zero redundancy  _RAID0_ pool.
> *Note:* To help when managing many devices, refer to them using `/dev/disk/by-id/`
> names such as the serial numbers of the drives.

To see the status of the pool execute the following command:
```bash
sudo zpool status pool-test
`To destroy a zpool:
```bash
sudo zpool destroy pool-test
`A 2 x 2 Mirrored Zpool Example
---
1. Create a zpool containing a VDEV of 2 drives in a mirror.`bash
   sudo zpool create mypool mirror /dev/vdc /dev/vdd
   `2. Add another VDEV of 2 drives in a mirror to the pool.`bash
   sudo zpool add mypool mirror /dev/vde /dev/vdf -f
   `3. Check the status.`bash
   sudo zpool status
   `The output is shown below.
   `pool: mypool
    state: ONLINE
     scan: none requested
   config:
           NAME        STATE   READ WRITE CKSUM
           mypool      ONLINE     0     0     0
             mirror-0  ONLINE     0     0     0
               sdc     ONLINE     0     0     0
               sdd     ONLINE     0     0     0
             mirror-1  ONLINE     0     0     0
               sde     ONLINE     0     0     0
               sdf     ONLINE     0     0     0
   `In this example:
* /dev/vdc, /dev/vdd, /dev/vde, /dev/vdf are the physical devices
* *mirror-0*, *mirror-1* are the VDEVs
* *mypool* is the pool

There are plenty of other ways to arrange VDEVs to create a zpool.

A Single File based Zpool Example
---

In the following example, a single 2GB file is used as a *VDEV* and zpool is
made from just this one VDEV. `/home/user/example.img` is a file based VDEV
and `pool-test` is the pool.`bash
dd if=/dev/zero of=example.img bs=1M count=2048
sudo zpool create pool-test /home/user/example.img
sudo zpool status
`ZFS Scrubbing
===

Scrubbing happens automatically when errors are detected. Recovery will be
attempted automatically. Scrubbing can be triggered manually:
```bash
sudo zpool scrub pool-test
`This can be scripted and scheduled via cron. Check the status of the scrub
using zpool status:
```bash
sudo zpool status -v pool-test
`Configuring and Tuning ZFS
===

Each ZFS dataset or pool can have properties set.

**quota** - Limits the amount of space a dataset and its descendents can
consume. This property enforces a hard limit on the amount of space used.
This includes all space consumed by descendents, including file systems
and snapshots. Setting a quota on a descendent of a dataset that already has
a quota does not override the ancestor's quota, but rather imposes an additional
limit. Quotas cannot be set on volumes, as the `volsize` property acts as an
implicit quota.

To set a maximum quota of 10GB on a dataset:
```bash
sudo zfs set quota=10G pool-test/mystuff
`To see a list of all options that can be set:
```bash
zfs get all pool-test
`Two of the most common options to set are `compression` and `atime`.

**atime** - Controls whether the access time for files is updated when they are
read. Turning this property  *off* avoids producing write traffic when reading
files and can result in significant performance gains, though it might confuse
mailers and other similar utilities. The default value is *on*.

**compression** - Controls the compression algorithm used for this dataset and
is described in the next section.

ZFS Compression
===

Data can be compressed automatically with ZFS. With the speed of modern CPUs
this is a useful option as reduced data size means less data to physically read
and write and hence faster I/O. ZFS offers a comprehensive range of compression
methods. **lz4** offers faster compression/decompression to lzjb and slightly
higher compression too.

The **compression** option controls the compression algorithm used for this
dataset. The **lzjb** compression algorithm is optimized for performance while
providing decent data compression. Setting compression to *on* uses the lzjb
compression algorithm. The gzip compression algorithm uses the same compression
as the gzip command. You can specify the gzip level by using the value *gzip-N*
where *N* is an integer from *1* (fastest) to *9* (best compression ratio).
Currently, *gzip* is equivalent to *gzip-6* (which is also the default for gzip).

To turn on compression:
```bash
sudo zfs set compression=on pool-test
`To set the compression type:
```bash
sudo zfs set compression=lz4 pool-test
`To check the compression level:
```bash
sudo zfs get compressratio
`ZFS Snapshots
===

A ZFS snapshot is a read-only copy of a ZFS file system or volume. It can be used
to save the state of a ZFS file system at a point of time, and one can rollback
to this state at a later date. One can even extract files from a snapshot without
performing a complete rollback.

To snapshot the mypool/projects file system:
```bash
sudo zfs snapshot -r mypool/projects@snap1
`To view the collection of snapshots:
```bash
sudo zfs list -t snapshot
`Simulate an accident and destroy all the files and then rollback:
```bash
rm -rf /mypool/projects
sudo zfs rollback mypool/projects@snap1
`Snapshots of file systems are accessible in the `.zfs/snapshot` directory within
the root of the file system. For example, if *mypool/projects* is mounted on
*/mypool/projects*, then the *mypool/projects@snap1* snapshot data is accessible
in the */mypool/projects/.zfs/snapshot/snap1* directory. This is a very useful
hidden feature.

To remove a snapshot:
```bash
sudo zfs destroy mypool/projects@snap1
`ZFS Clones
---

A ZFS clone is a writeable copy of a filesystem with the initial content of the
clone being identical to the original filesystem. A ZFS clone can only be created
from a ZFS snapshot and the snapshot cannot be destroyed until all the clones
created from it are also destroyed.

To clone mypool/projects, first make a snapshot and then clone:

```bash
sudo zfs snapshot -r mypool/projects@snap1
sudo zfs clone mypool/projects@snap1 mypool/projects-clone
`ZFS Lab
===

Run the following commands on the `LABVM` machine.

In this lab you will create a ZFS filesystem.

1. Install the `zfsutils-linux` package.`bash
sudo apt install zfsutils-linux
2. View the drives available. You will install ZFS on the first drive - vdc.

```bash
sudo parted -l | grep vd
3. Check the drive. If there are partitions, delete them.

```bash
sudo parted -s /dev/vdc rm 1
4. Init the drives if not recognized (not required for ZFS). Set the disk as a
   GPT partition type in parted. `-s` (script) option prevents interaction
> Use lsblk to see devices and:
```bash
sudo parted -s /dev/vdc mklabel gpt
sudo parted -s /dev/vdd mklabel gpt
sudo parted -s /dev/vde mklabel gpt
5. Create the storage pool

```bash
sudo parted -s /dev/vdc mkpart primary 1 2G
sudo parted -s /dev/vdd mkpart primary 1 2G
sudo zpool create zfspool /dev/vdc1
sudo zpool create -f zfspool /dev/vdc1
sudo zpool add -f zfspool /dev/vde
sudo zpool add zfspool /dev/vdd1
6. See status and whats included

```bash
sudo zpool status zfspool
sudo zpool status
7. Create a dataset (filesystem). ZFS allows one to create a maximum of `2^64`
   file systems per pool. Each ZFS file system can have properties set`bash
sudo zfs create zfspool/mystuff
sudo zfs create zfspool/myFs2
8. View the created datasets/filesystems

```bash
sudo zfs list -H -o name
9. ZFS rpools get automounted. Check status with the `mount` command`mount
   zfspool on /zfspool type zfs (rw,relatime,xattr,noacl)
   zfspool/mystuff on /zfspool/mystuff type zfs (rw,relatime,xattr,noacl)
   ```
```bash
mount | grep zfspool
10. Check mount status with the zfs mount command:
    `zfspool/zfspool
    zfspool/mystuff /zfspool/mystuff
    ```
```bash
sudo zfs mount
11. Manual mounting of ZFS

```bash
sudo mkdir /mnt/myzfs
sudo zfs set mountpoint=legacy zfspool/mystuff
sudo mount -F -t zfs zfspool/mystuff /mnt/myzfs
```
> You can also use legacy mode mounting via `/etc/fstab`.

Tuning
---

1. ZFS compression. As mentioned earlier, one can compress data automatically
   with ZFS. With the speed of modern CPUs this is a useful option as reduced
   data size means less data to physically read and write and hence faster I/O.
   ZFS offers a comprehensive range of compression methods. The default is *lz4*
   (a high performance replacement of *lzjb*) that offers faster
   compression/decompression to *lzjb* and slightly higher compression too.
   1. Compression. Default compression is *off*. Change to *on* for pool or
      part of it
   2. Default compression type is *lz4*. Set it explicitly or change it
   3. Use `df`/`du` to see compressed size. (`ls` won't show it). *lz4* is
      significantly faster than the other options while still performing well;
      *lz4* is the safest choice.
   4. check on compression leve`bash
sudo zfs set compression=on zfspool/mystuff
sudo zfs set compression=on zfspool
sudo zfs set compression=lz4 zfspool
sudo zfs set compression=gzip-9 zfspool
sudo zfs get compressratio
2. See all zfs pools options

```bash
sudo zfs get all zfspool
3. Write a file to ZFS

```bash
sudo cp ~/.bashrc /mnt/myzfs
`ZFS Snapshots
---

1. Take a snapshot.`bash
sudo zfs snapshot -r zfspool/mystuff@snap1
2. View all snapshots.
   `NAME USED AVAIL REFER MOUNTPOINT
   zfspool /mystuff@snap1 8.80G - 8.80G -
   ```
```bash
sudo zfs list -t snapshot
3. Create another file

```bash
cd /mnt/myzfs
sudo touch postsnap1
ls -alt
4. Rollback and see that file2 is lost and file1 is still there

```bash
sudo zfs rollback zfspool/mystuff@snap1
ls -alt
`ZFS Clones
---

A ZFS clone is a writeable copy of a file system with the initial content of
the clone being identical to the original file system. A ZFS clone can only be
created from a ZFS snapshot and the snapshot cannot be destroyed until all the
clones created from it are also destroyed.

1. Clone a pool (at a snapshot point). Note: snap uses `@`, clone uses `/`
```bash
sudo zfs clone zfspool/mystuff@snap1 zfspool/mystuff/snap1clone
sudo zfs list
2. Set a maximum quota of 10 gigabytes for the filesystem or subdirectory.

```bash
sudo zfs set quota=10G zfspool/mystuff
sudo zfs get all zfspool/mystuff
`ZFS Send and Receive
---

ZFS `send` sends a snapshot of a filesystem that can be streamed to a file or
to another machine. ZFS receive takes this stream and will write out the copy
of the snapshot back as a ZFS filesystem. This is great for backups or sending
copies over the network (e.g. using `ssh`) to copy a file system.

1. For `zfspool/mystuff`, make a snapshot and save it to a file, then send
   and receive it back`bash
sudo zfs snapshot -r zfspool/mystuff@snap2
sudo zfs send zfspool/mystuff@snap2 > ~/mystuff-snap.zfs
sudo zfs receive -F zfspool/mystuff-copy < ~/mystuff-snap.zfs
`ZFS Ditto Blocks
---

Ditto blocks create more redundant copies of data to copy, just for more added
redundancy. With a storage pool of just one device, ditto blocks are spread
across the device, trying to place the blocks at least 1/8 of the disk apart.
With multiple devices in a pool, ZFS tries to spread ditto blocks across
separate VDEVs. 1 to 3 copies can be set.

1. For `zfspool/mystuff`, setting 3 copies`bash
sudo zfs set copies=3 zfspool/mystuff
sudo zfs get all zfspool/mystuff | grep copies
`ZFS Deduplication
---

ZFS dedup will discard blocks that are identical to existing blocks and will
instead use a reference to the existing block. This saves space on the device
but comes at a large cost to memory. The dedup in-memory table uses ~320 bytes
per block. The greater the table is in size, the slower write performance
becomes.

1. Enable dedup on `zfspool/mystuff`.
   > For more pros/cons of deduping, refer to
   > http://constantin.glez.de/blog/2011/07/zfs-dedupeor-not-dedupe.
   > Deduplication is almost never worth the performance penalty.`bash
sudo zfs set dedup=on zfspool/mystuff
sudo zfs get all zfspool/mystuff | grep dedup
`ZFS Pool Scrubbing
---

Pools are scrubbed for errors automatically, but can also be run manual ly.

1. To initiate an explicit data integrity check on a pool use the `zfs scrub` command.`bash
sudo zpool scrub zfspool
sudo zpool status -v zfspool
2. In this example you will destroy some data and then recover it.
   1. Use `dd` to populate zfs with some data.
   2. Calculate the checksum on the data.
   3. Use `dd` write zeroes to the `vdf` drive.
      > Check the disk size before overwri ting (count = xxxx)<br/>
      > 5G Devices = 5x1024 = 5120 1M blocks
   4. Initiate the scrub.
   5. Check the status.
   6. Remove the drive from the pool.
   7. Swap it out and add a new one back.
   8. Initiate a scrub to repair.`bash
sudo dd if=/dev/urandom of=/zfspool/random.dat bs=1M count=20
md5sum /zfspool/random.dat
sudo dd if=/dev/zero of=/dev/vde bs=1M count=10
sudo zpool scrub zfspool
sudo zpool status
# after it finished scrubbing, verify the md5 is the same
md5sum /zfspool/random.dat
`Destroy a ZFS Pool
---

1. Destroy clones, then snapshots, then data, finally the pool
   1. Destroy all clones
      > *Note:* Cannot destroy a snapshot till all clones are destroyed.
   2. Remove all snapshots
   3. Destroy the file systems, (can use -r for recursion)
      > *Note:* You cannot destroy data untill all snapshots are destroyed.<br/>
      > You can recursively destroy all datasets in the pool
   4. Destroy the pool itself. (This takes time!)
```bash
sudo zfs destroy zfspool/mystuff/snap1clone
sudo zfs destroy zfspool/mystuff@snap1
sudo zfs destroy zfspool/myFs2
sudo zfs destroy zfspool/mystuff
sudo zfs destroy -r zfspool
sudo zpool destroy zfspool
`ZFS RAID Lab
===

Run the following commands on the `LABVM` machine.

In this lab you will create a ZFS RAID.

1. File based ZFS
   1. Create a single file based zpool zfsraidtest.
   2. Destroy the pool`bash
sudo dd if=/dev/zero of=zfsraidtest.qcow2 bs=1M count=2048
sudo zpool create zfsraidpool /home/$USER/zfsraidtest.qcow2
sudo zpool status
sudo zpool destroy zfsraidpool
2. Striped VDEVs RAID0
   1. Create a striped pool using 4 VDEVs:
   2. Destroy it

```bash
sudo zpool create -f zfsraid0 /dev/vdb /dev/vdc /dev/vdd /dev/vde
sudo zpool destroy zfsraid0
3. Mirrored VDEVs
   1. Create a mirrored pool with 2 VDEVs and destroy it

```bash
sudo zpool create zfsraid1 mirror /dev/vdd /dev/vde
sudo zpool destroy zfsraid1
4. Striped Mirrored VDEVs create striped *2x2* mirrored pool and destory it

```bash
sudo zpool create zfsraid10 mirror /dev/vdb /dev/vdc mirror /dev/vdd /dev/vde -f
sudo zpool status
sudo zpool destroy zfsraid10
5. RAIDZ
   1. Create a 4 VDEV RAIDZ and destroy it

```bash
sudo zpool create zfsraid5 raidz /dev/vdb /dev/vdc /dev/vdd /dev/vde
sudo zpool destroy zfsraid5
6. RAIDZ2
   1. Create a 2 parity 5 VDEV pool and destroy it

```bash
sudo zpool create zfsraid6 raidz2 /dev/vdb /dev/vdc /dev/vdd /dev/vde /dev/vdf
sudo zpool destroy zfsraid6
7. Nested RAIDZ
   1. Create and destory a 2 x RAIDZ

```bash
sudo zpool create zfsraid60 raidz /dev/vdd /dev/vde
sudo zpool add zfsraid60 raidz /dev/vdb /dev/vdc
sudo zpool destroy zfsraid60
8. Create a 2 x 2 mirrored zpool.
   1. Create a zpool containing a VDEV of 2 drives in a mirror.
      > *Note:* not destroyed, used below

```bash
sudo zpool create zfsmirror mirror /dev/vdd /dev/vde
9. ZFS Cache Drives and ZFS Intent Logs. Add a cache drive /dev/vdg to the pool
   'zfsmirror'. Destroy the pool afterwards.

```bash
sudo zpool add zfsmirror log  /dev/vdb -f
sudo zpool add zfsmirror cache /dev/vdc -f
sudo zpool status
sudo zpool destroy zfsmirror
```

# Advanced Networking Concepts

In this section you will learn to:
* Explain and configure netplan
* Explain and configure a linux bridge
* Explain and configure a bond
* Explain and configure trunking

Netplan
===

Netplan is a utility for easily configuring networking on an Ubuntu Linux
system. You simply create a YAML description of the required network interfaces
and what each should be configured to do. From this description Netplan will
generate all the necessary configuration for your chosen renderer tool.

Netplan was added to Ubuntu in the 17.10 (Artful) release and became the
default network configuration renderer for Ubuntu 18.04 LTS (Bionic) and continues
to be the default in Ubuntu 24.04 LTS (Noble) and all subsequent releases.

Netplan reads network configuration from `/etc/netplan/*.yaml` which are
written by administrators, installers, cloud image instantiations, or other
OS deployments. All installers only generate such a file, no
**/etc/network/interfaces** any more. There is also a `netplan` command line tool
to drive some operations.

During early boot, Netplan generates backend specific configuration files in
 **/run** to hand off control of devices to a particular networking daemon.

Netplan currently works with these supported networking daemons:
* NetworkManager 
* systemd-networkd

> Wifi and WWAN get managed by NetworkManager.<br/>
> Any other configured devices get handled by networkd by default, unless explicitly
> marked as managed by a specific manager (NetworkManager).<br/>
> Devices that are not covered by the network config do not get touched at all.

![Netplan Rendering](./images/netplan.png)

General Configuration Structure
---

The top-level node is a network: mapping that contains **version: 2** (the YAML
currently being used by *curtin*, *MAAS*, etc. is version **1**), and then device
definitions grouped by their type, such as  **ethernets:**, **wifis:**, or **bridges:**.
These are the types that our renderer can understand and are supported by the backends.

Each type block contains device definitions as a map where the keys (called  *configuration
IDs*) are defined in the following section.

### Device Configuration IDs

The key names below the per-device-type definition maps (like  **ethernets:**) are called
**ID**s. They must be unique throughout the entire set of configuration files. Their
primary purpose is to serve as anchor names for composite devices, for example to enumerate
the members of a bridge that is currently being defined.

There are two physically/structurally different classes of device definitions, and the  *ID*
field has a different interpretation for each:

### Physical devices

> Examples: ethernet, wifi

These can dynamically come and go between reboots and even during runtime (hotplugging).
In the generic case, they can be selected by `match:` rules on desired properties, such as
name/name pattern, MAC address, driver, or device paths. In general these will match any
number of devices (unless they refer to properties which are unique such as the full path
or MAC address), so without further knowledge about the hardware these will always be
considered as a group.

With specific knowledge (taken from the admin, a gadget snap, etc.), or by using unique
properties such as  *path* or *MAC*, match rules can be written so that they only match
one device. Then the **set-name:** property can be used to give that device a more
specific/desirable/nicer name than the default from udev's ifnames. Any additional device
that satisfies the match rules will then fail to get renamed and keep the original kernel
name (and dmesg will show an error).

It is valid to specify no match rules at all, in which case the ID field is simply the
interface name to be matched. This is mostly useful if you want to keep simple cases
simple, and it's how we have done network device configuration since the mists of time.
If there are *match:* rules, then the ID field is a purely opaque name which is only
being used for references from definitions of compound devices in the config.

### Virtual devices

> Examples: veth, bridge, bond

These are fully under the control of the config file(s) and the network stack. I. e. these
devices are being  _created_ instead of _matched_. Thus **match:** and **set-name:** are not
applicable for these, and the ID field is the name of the created virtual device.

Netplan Commands
---

* `generate` Runs during early boot and will read config, and write files 
* `apply` Kicks the various backends to realize network config 
* `try` Similar to *generate* but asks for confirmation otherwise rolls back configuration

Linux Bridges
===

A bridge is a means to connect two Ethernet networks together through software to create
one larger Ethernet network. The bridge is more powerful than a pure hardware bridge
because it can also filter and direct traffic. Bridging is a way to share a network
connection between two (or more) computers. One reason to bridge Ethernet connections
is to monitor traffic flowing across an Ethernet cable. Another reason is to provide
a redundant network.

Packets are forwarded based on Ethernet address (like a switch), rather than IP address
(like a router). Since forwarding is done at Layer 2, all protocols can go transparently
through a bridge. The job of the bridge is to examine the destination of the data packets
one at a time and decide whether or not to pass the packets to the other side of the
Ethernet segment. This results in a faster, quieter network with fewer collisions.

To create the bridge, plug one computer into another computer that has a connection
to a larger network, such as the Internet. Let the bridged computer use the networked
computer's connection. The networked computer must have two ethernet ports, one for
the big network, and one for the bridged computer.

Sample Bridge Configuration
---`yaml
network:
    version: 2
    renderer: networkd
    bridges:
        br0:
            addresses: [1.2.3.4/24]
            gateway4: 1.2.3.1
            nameservers:
                addresses: [8.8.8.8]
            interfaces: [eth0, eth1]
`This *netplan* bridge configuration sets up a Layer 2 bridge between  *eth0* and *eth1*
Ethernet iterfaces. It uses the *networkd* renderer and configures an IPv4 address on
the bridge.

You can manage bridges using the `brctl` command part of the **bridge-utils** packages.

Network Interface Bonding
===

Bonding, or link aggregation, means combining several network interfaces (NICs) into a
single link. This provides either high-availability, load-balancing, better throughput,
or a combination of there of.

A system with multiple interfaces may be configured to use bonding. It is an L2
aggregation of multiple interfaces (physical or virtual ) into a single logical
network interface.

Depending on the mode selected, when interfaces are aggregated into a bond, they may
provide redundancy for each other, allow increased total network throughput (by
sending data from different connections over multiple interfaces at once), or both.

Example, configuring the aggregate in  *active-backup* and setting the link monitor
to *100ms*:

```bash
sudo ip link add bond0 type bond mode active-backup miimon 100
`Sample Bonding Configuration
---`yaml
network:
    version: 2
    renderer: networkd
    bonds:
        bond0:
            dhcp4: yes
            interfaces: [eth0, eth1]
            parameters:
                mode: active-backup
                primary: eth0
`This *netplan* bond configuration creates a Linux bond named  **bond0** from Ethernet
interfaces *eth0* and *eth1*. The primary interface is configured to be *eth0* and
the aggregation mode is set to  *active-backup*.

The IP address of the *bond0* virtual interface is configured dynamically by DHCP.

> *Note:* you can use the `ip link` command to manage the bonds or the `ifenslave`
> command from the **ifenslave** package.

> *Warning:* installing the **ifenslave** package on Ubuntu (including 24.04) will
> also bring in the  **ifupdown** package which is the old network manager from
> previous Ubuntu releases. As a result, you will find the **/etc/network/interfaces**
> file showing up but empty. It's recommended to use Netplan for network configuration
> instead of ifupdown on modern Ubuntu systems.

VLANs
===

Virtual Local Area Networks are used to divide a physical network into several
broadcast domains. The reason to use VLANs is to divide a network and separate
hosts that shouldn't be able to access each other directly or at all.

Packets sent on a VLAN are tagged. This refers to the VLAN identification data
(called a "tag" ) which is added to the Ethernet header of a packet. A tag
contains two important pieces of data:

1. The **VLAN ID** (VID) a number in the range *1 - 4095*. Some VIDs (notably
   1 and 1002-1005) are generally unusable due to widespread preallocation on
   many switches. VLAN *1* is most commonly used for management so this should
   not be used.
2. **Priority information** (a priority value) is used for traffic classification
   purposes - including expedited routing.

There are two types of packets on a VLAN: tagged and untagged packets. The
untagged packet, the most common type, is a regular packet. The switch decides
to which VLAN an untagged packet belongs. A switch can be configured to assign
specific ports to specific VLANs. The switch can also be configured to receive
tagged packets.

If the switch receives a tagged packet and the port on which it receives the packet
is configured to allow tagged packets, the switch knows to which ports it can send
the packet. This could be used to make a VLAN span more than one switch or to make
use of a VLAN aware NIC (Network Interface Card) on a router, firewall, server or
workstation.

The following are required to make use of a VLAN:
* One or more switches that support the IEEE **802.1q** standard.
* A NIC (Network Interface Card) that can transmit and receive tagged packets.
* A userspace tool to create the VLAN-aware interfaces.

### Trunking

There are two ways to connect a machine to a switch that carries 802.1Q VLANs:
* Via an untagged port, where VLAN support is handled by the switch (so the
  attached machine sees ordinary Ethernet frames);
* Via a tagged (trunk) port, where VLAN support is handled by the attached machine
  (which sees 802.1Q-encapsulated Ethernet frames).

The advantage of a tagged port is that it allows multiple VLANs to be carried by
a single physical bearer. The disadvantage is that the machine in question must
support *801.q* and be configured to use it. Typical practice is to use tagged
ports for machines that need to talk to multiple VLANs and untagged ports for
everything else.

You need to know whether the port being presented to a machine is tagged or
untagged. These instructions apply if and only if it is a tagged port.

VLANs are numbered from *1* to *4094* inclusive (the values *0* and *4095* are
reserved). Some manufacturers (including Cisco) additionally recommend that
VLAN *1* be reserved for management purposes.

Sample VLAN Trunking Configuration
---`yaml
network:
    version: 2
    renderer: networkd
    vlans:
        vlan42:
            id: 42
            link: eth0
            addresses: [ "10.107.0.42/24" ]
`This *netplan* configuration sets up a virtual interface called `vlan42` on top
of the Ethernet interface  *eth0* with VLAN ID *42* and a static IPv4 address.

VLAN Lab
===

Run the following commands on the `LABVM` machine.

Install `net-tools` package:
```bash
sudo apt install net-tools
`In this lab we will create a VLAN.
1. Create a VLAN interface on ens2 for VLAN 42.`bash
sudo ip link add link ens2 name ens2.42 type vlan id 42
2. To check the VLAN interface was created, and what VLAN ID it will use, run
   `ip link show` with the `-d` switch (for 'details').`bash
ip -d link show ens2.42
3. Assign an IP address to the VLAN interface.

```bash
sudo ip addr add 192.168.42.42/24 brd 192.168.42.255 dev ens2.42
4. Check the IP address was correctly assigned with `ip addr show`.`bash
ip addr show dev ens2.42
5. Bring up the VLAN interface.

```bash
sudo ip link set dev ens2.42 up
6. Check the VLAN interface is up.

```bash
ip addr show ens2.42
7. In Ubuntu, 8021q module is shipped with the kernel. Check this module is there.
   > *linux-image-$(uname -r): /lib/modules/$(uname -r)/kernel/net/8021q/8021q.ko*.

   Check that running the first `ip` command automatically loaded the 8021q kernel
   module:
   > ```
   > 8021q                  32768  0
   > garp                   16384  1 8021q
   > mrp                    20480  1 8021q
   > ```

```bash
dpkg -S /lib/modules/$(uname -r)/kernel/net/8021q/8021q.ko
lsmod | grep 802
modinfo 8021q
8. Trunking is when you add more than 1 VLAN on the same underlying path
    1. Create a VLAN interface on ens4 for VLAN 100
    2. Display info on the new VLAN
> You will add a secondary VLAN to the ens4 interface using `ip link add`
```bash
sudo ip link add link ens2 name ens2.100 type vlan id 100
sudo ip link set dev ens2.100 up
ip -d link show ens2.100
sudo ip addr add 192.168.100.42/24 brd + dev ens2.100
ip addr show dev ens2.100
`Bonding/Bridging Lab
===

In this lab you will learn to add a dummy NIC for use with boding and bridging.

1. Make sure that you have the dummy kernel module loaded. If the dummy module is not
   loaded, force it to load.`bash
sudo lsmod | grep dummy
sudo modprobe dummy
sudo lsmod | grep dummy
2. With the driver now loaded you can create whatever dummy network interfaces you
   like. And confirm it.

```bash
sudo ip link add dummy0 type dummy
sudo ip link set name ens10 dev dummy0
ip link show ens10
3. Changing the MAC - You can change the MAC address.

```bash
sudo ifconfig ens10 hw ether 00:22:22:ff:ff:ff
4. Creating an alias.
    1. You can create aliases on top of ens10.
    2. Confirm them.
    3. Create aliases using ip.

```bash
ip link show ens10
sudo ip addr add 192.168.100.199/24 brd + dev ens10 label ens10:0
sudo ifconfig -a ens10
sudo ifconfig -a ens10:0
ip a | grep -w inet
5. Remove the test interface.

```bash
sudo ip addr del 192.168.100.199/24 brd + dev ens10 label ens10:0
sudo ip link delete ens10 type dummy
sudo rmmod dummy
`Bonding
---

1. Create two dummy interfaces to be used in the bond`bash
sudo ip link add ens10 type dummy
sudo ip link add ens11 type dummy
2. Create a bond interface and set its mode to `active-backup`
```bash
sudo ip link add bond0 type bond mode active-backup miimon 100
3. Enslave the two interfaces to the bond

```bash
sudo ip link set dev ens10 master bond0 state up
sudo ip link set dev ens11 master bond0 state up
4. Set an IP address on the bond

```bash
sudo ip link set dev bond0 state up
sudo ip addr add dev bond0 172.16.0.14/24
```
> To see the changes:
```bash
ip -d addr show
5. Remove the bond interface.
   > *Note:* do not remoe the dummy interfaces as you will need them in the next
   > exercise

```bash
sudo ip link del bond0
`Bridges
---

1. Install the `bridge-utils` package.`bash
sudo apt install bridge-utils
2. Create a Linux bridge.

```bash
sudo brctl addbr br0
sudo brctl show
sudo ip link show
3. Add the two interfaces from the bonding exercise to the bridge.

```bash
sudo brctl addif br0 ens10
sudo brctl addif br0 ens11
sudo brctl show
sudo ip link show
4. Set an IP address on the bridge.

```bash
sudo ip addr add dev br0 10.255.0.4/24
sudo ip -d addr show
5. Clean up and remove the interfaces.

```bash
sudo brctl delif br0 ens11
sudo brctl delif br0 ens10
sudo brctl delbr br0
sudo ip link del ens11
sudo ip link del ens10
```

# Security

In this section you will learn to:
* Define and explain PAM
* Define and use ACLs
* Define and use AppArmor

> **Exercise Caution:**
> Each of the technologies introduced in this section can, if misused, result
> in being unable to log in to a machine. Take great care when testing or using
> the information here on any machine with important data.

Pluggable Authentication Modules (PAM)
===

Pluggable Authentication Module (PAM) is a system of libraries that handles all
authentication in Ubuntu by default. The library provides a stable general
interface (Application Programming Interface - API) that privilege granting
programs (such as login and su) defer to in order to perform standard
authentication and authorization tasks.

The principal feature of the PAM approach is that the nature of the authentication
is dynamically configurable. In other words, the system administrator is free to
choose how individual service-providing applications will authenticate users. This
dynamic configuration is set by the contents of the single Linux-PAM configuration
file `/etc/pam.conf`.

Alternatively, the configuration can be set by individual configuration files
located in the `/etc/pam.d/` directory. The presence of this directory will cause
Linux-PAM to ignore `/etc/pam.conf`. Each module represents a particular
authentication mechanism, and is named *pam_xxxxx*.so.

**pam_unix.so** handles basic Linux authentication using the `/etc/passwd`,
`/etc/group` and `/etc/shadow` files. **pam_ldap.so** handles authentication using
an LDAP database.

Linux-PAM separates the tasks of authentication into four independent management
groups:
* **Account management** - check the account status and whether the user is
  authorized to perform a task.
* **Authentication management** - ensure users are who they say they are.
* **Password management** - change a user's password or other credentials.
* **Session management** - perform something only during the user's current
  session (maintenance of audit trails or mounting of the user's home directory).

PAM Configuration
---

Each configuration line has 3 fields:
* **Function type** - The function that the user application asks PAM to perform.
* **Control argument** - This detemines what PAM does if the action of the current
  line is successful or fails.
* **Module** - The module that runs for the line determining what the line does.
An example usage for `/etc/pam.d/login` would be:

```ini
# Authenticate the user
auth required pam_unix.so

# Ensure users account and password are still active
account required pam_unix.so

# Change the users password, but at first check the strength with pam_cracklib
password required pam_cracklib.so retry=3 minlen=6 difok=3
password required pam_unix.so use_authtok nullok md5
session required pam_unix.so
`In the example above, in the first line, `auth` means the application is asking
PAM to authenticate the user. Required indicates that if the rule succeeds, PAM
proceeds to additional rules. If the rule fails, PAM proceeds to additional rules
but will always return an unsuccessful authentication regardless of the end
result of the additional rules.

**pam_unix.so** is the module that PAM executes to authenticate the user.

Common Settings
---

PAM supports reusing *common* configuration files between services. The **common
authentication settings** config file, for example, defines the different
authentication methods used by default in all services like this:

```ini
#
# /etc/pam.d/common-auth - authentication settings common to all services
#
# This file is included from other service-specific PAM config files,
# and should contain a list of the authentication modules that define
# the central authentication scheme for use on the system
# (e.g., /etc/shadow, LDAP, Kerberos, etc.).  The default is to use the
# traditional Unix authentication mechanisms.
#
# As of pam 1.0.1-6, this file is managed by pam-auth-update by default.
# To take advantage of this, it is recommended that you configure any
# local modules either before or after the default block, and use
# pam-auth-update to manage selection of other modules.  See
# pam-auth-update(8) for details.

# here are the per-package modules (the "Primary" block)
auth    [success=1 default=ignore]      pam_unix.so nullok_secure
# here's the fallback if no module succeeds
auth    requisite                       pam_deny.so
# prime the stack with a positive return value if there isn't one already;
# this avoids us returning an error just because nothing sets a success code
# since the modules above will each just jump around
auth    required                        pam_permit.so
# and here are more per-package modules (the "Additional" block)
auth    optional                        pam_cap.so
# end of pam-auth-update config
`These *common* files can be referenced in a service file using @<common-file>
format (a simple example for the `chsh` command):

```ini
#
# The PAM configuration file for the Shadow `chsh' service
#

# This will not allow a user to change their bash unless
# their current one is listed in /etc/bashs. This keeps
# accounts with special bashs from changing them.
auth       required   pam_bashs.so

# This allows root to change user bash without being
# prompted for a password
auth            sufficient      pam_rootok.so

# The standard Unix authentication modules, used with
# NIS (man nsswitch) as well as normal /etc/passwd and
# /etc/shadow entries.
@include common-auth
@include common-account
@include common-session
`PAM Architecture
---

![PAM Architecture](./images/pam.png)

libpam Packages
---

Applications have to be written with *PAM* library support. Application
programmers use the *PAM* API (**libpam**) to authenticate applications.
Applications interact with PAM modules based on the PAM configuration in
**/etc/pam.d**.

**libpam-cap**: POSIX 1003.1e capabilities (PAM module)<br/>
**libpam-cracklib**: PAM module to enable cracklib support<br/>
**libpam-doc**: Documentation of PAM<br/>
**libpam-modules**: Pluggable Authentication Modules for PAM<br/>
**libpam-modules-bin**: Pluggable Authentication Modules for PAM - helper
binaries<br/>
**libpam-runtime**: Runtime support for the PAM library<br/>
**libpam-systemd**: system and service manager - PAM module<br/>
**libpam0g**: Pluggable Authentication Modules library<br/>

PAM Modules
---

To get a list of applications on a system that can use PAM in some way, you
can run the following small script:
```bash
for i in /usr/{bin,sbin}/* ; do
  ldd $i | grep -q libpam
  if [ $? == 0 ] ; then
    echo $i
  fi
done
`To check a specific application for PAM functionality:
```bash
ldd $(which prog_name) | grep libpam
`PAM configuration files are stored in **/etc/pam.d**. To see the files:
```bash
ls /etc/pam.d
`Generally **/etc/pam.d** has a configuration file for each application that
requests PAM authentication. If an application calls PAM, but there is no
associated configuration file, the **other** configuration file is applied.

The configuration files usually have calls to include the *common-* files.
These are general configuration files whose rules should be applied in most
situations.

The modules that are referenced in the configuration files can be located
with this command:
```bash
ls /lib/*/security
`PAM LAB
===

In this lab you will review and explain a PAM configuration file. Use the
following configuration and explain the lines. Don't forget to use the manual
pages to help.`ini
auth [success=1 default=ignore] pam_unix.so nullok_secure
auth requisite pam_deny.so
auth required pam_permit.so
auth optional pam_ecryptfs.so unwrap
```

**Line 1**: This line provides standard unix authentication configured through
the */etc/nsswitch.conf* file. This means checking the */etc/passwd* and
*/etc/shadow* files. The `nullok_secure` argument passed to the *unix* module
specifies that accounts with no password are okay, as long as login information
checks out with the `/etc/securetty` file. The control field, which has
`[success=1 default=ignore]` replaces the simplified *required* , *sufficient*
etc. parameters and allows for more fine-grained control. In this instance, if
the module returns success, it skips the next *1* line. The default case, which
handles every other return value of the module, results in the line being
ignored and moving on.

**Line 2**: This line has the control value of  *requisite* meaning that if it
fails, the entire configuration returns a failure immediately. It also calls on
the `pam_deny` module, which returns a failure for every call . This means that
this will always fail. The only exception is when this line is skipped, which
happens when the first line returns successfully.

**Line 3**: This line is required and calls the `pam_permit` module, which
returns success every time. This simply resets the current *pass/fail* record
at this point to ensure that there aren't some strange values from earlier.

**Line 4**: This line is listed as optional, and calls the `pam_ecryptfs`
module with the `unwrap` option. This is used to unwrap a passphrase using the
supplied password, which will then be used for mounting a private directory.
This is only relevant when you use this technology, which is why it is optional.

1. man pam_ for help.  `man pam_unix`, shows this example file:
```ini
# Authenticate the user
auth required pam_unix.so
# Ensure users account and password are still active
account required pam_unix.so
# Change the users password, but at first check the strength
# with pam_cracklib(8)
password required pam_crackl ib.so retry=3 minlen=6 difok=3
password required pam_unix.so use_authtok nullok md5
session required pam_unix.so
2. Find modules in /bin, /sbin, /usr/bin, /usr/sbin that use PAM

```bash
for i in /{bin,sbin}/* /usr/{bin,sbin}/* ; do
  ldd $i | grep -q libpam
  if [ $? == 0 ] ; then
    echo $i
  fi
done
3. Check a specific program for PAM functionality.

```bash
ldd /bin/login | grep pam
4. See where the security modules are located, and what modules are available.

```bash
ls /lib/*/security
5. Modify PAM to restrict a minimum password length. Open 
`/etc/pam.d/common-password` and locate the line containing
`password        [success=1 default=ignore]      pam_unix.so obscure sha512`.
Add `minlen=8` to set a minimum of 8 characters passwords.


6. Run pam-auth-update to change the settings`bash
pam-auth-update
`Access Control Lists (ACLs)
===

POSIX Access Control Lists (ACLs) are fine-grained access rights for files and
directories. An ACL consists of entries specifying access permissions on an
associated object. ACLs can be configured per user, per group or via the effective
rights mask. They use the same permissions as `rwx` found in regular permissions.
ACLs are enabled with the default mount options for the ext4 filesystem as of
Ubuntu 14.04.

ACL Entries
---

ACL entries consist of a user (u), group (g), other (o) and an effective rights
mask (m). An effective rights mask defines the most restrictive level of
permissions. `setfacl` sets the permissions for a given file or directory.
`getfacl` shows the permissions for a given file or directory.

Defaults for a given object can be defined.

ACLs can be applied to users or groups but it is easier to manage groups.
Groups scale better than continuously adding or subtracting users.

The utility getfacl lists the ACLs for a given file or directory.`bash
getfacl /var/www
`The utility `setfacl` is used to add the groups *blue* and *green* to the ACL
for the directory /var/www.`bash
sudo setfacl -m g:green:rwx /var/www/
sudo setfacl -m g:blue:rwx /var/www/
sudo getfacl /var/www/
`The option `-x` removes groups or users from a given ACL. Below, the group
*green* is removed from the directory /var/www.`bash
setfacl -x g:green /var/www
`Transfer of ACL Attributes from a Specification File
---

The transfer of ACL attributes from a specification file takes two steps.
In this example, the specification file is called  `acl`.
1. Create a file containing the ACL to be used.`bash
echo "g:green:rwx" > acl
2. Read the contents of the file into `setfacl` to set the ACL for directory
`/path/to/dir`
```bash
setfacl -M acl /path/to/dir
`Output from `getfacl` is accepted, when reading from files using -M which
tells `setfacl` to modify the acl list for the specified directory.

Copying ACLs from One File or Directory to Another
---

To copy an ACL from *dir1* to *dir2* use the `-M` option. Output from `getfacl`
is accepted as input for `setfacl` when using `-M`.`bash
getfacl dir1 | setfacl -b -n -M - dir2
```
`-b` clears the ACLs<br/>
`-n` do not recalculate effective rights mask<br/>
`-` to read from stdin

Or it can be done like this:
```bash
getfacl file1 | setfacl --set-file=- file2
`Copying an ACL into the Default ACL
---

Once the ACLs are the way they need to be, they can be set as the default.
Defaults are inherited, so a new directory will inherit the defaults of the
parent directory.`bash
getfacl -a /path/to/dir | setfacl -d -M- /path/to/dir
`Adding and Removing a User
---

ACLs are not restricted to the user, group, other model. You can add multiple
users and groups with permissions specific to each.

Add user *ubuntu* with `rwx` to the ACL for the file named `coolcode`:
```bash
setfacl -m u:ubuntu:rwx coolcode
`To reset to defaults:
```bash
setfacl -x u:ubuntu: coolcode
`ACL with Groups and Other
---

Use `g` for *groups* and use `o` for *other*.`bash
setfacl -m g:devops:rwx coolcode
setfacl -x o:r coolcode
`ACL Masking
---

Use `m` for *mask*, list only available permissions:
```bash
setfacl -m m:r-x coolcode
`This limits everyone. The group may have rwx, the mask permits only `r-x`. For
example, the group with `rw-`, with this mask, ends up with `r--`.

ACL Lab
===

In this lab use will apply your knowledge of ACLs.
1. Install the acl package.`bash
sudo apt install acl
2. Look at the example and determine what the output indicates from the `getfacl`
   command for a file name `program`. Which groups have what access?
```
# file:program
# owner: ubuntu
# group: students
user::rw-
group::rw-
other::r--
group:qa:rwx
group:uat:rwx
mask::rwx
3. The utility `getfacl` lists the ACLs for a given file or directory. Check
   current acl settings (`getfacl`) on a directory and file.`bash
mkdir ~/testpermissions
touch ~/testpermissions/coolcode
getfacl ~/testpermissions/
getfacl ~/.bashrc
4. Adding and Removing ACLs for group and other - two models. Add groups called
   *green*, *blue* and devops. Also add a normal user (*cm*).

```bash
sudo addgroup devops
sudo addgroup blue
sudo addgroup green
sudo adduser cm
`Model 1: ACL with Groups and Other
---

5. Set(add) permissions for group called devops.
`cd testpermissions
setfacl -m g:devops:rwx coolcode
6. Add the groups *blue* and *green* to the ACL for the directory *testpermissions*.

```bash
setfacl -m g:green:rwx ~/testpermissions
setfacl -m g:blue:rwx ~/testpermissions
setfacl -m g:green:rwx coolcode
setfacl -x g:green ~/testpermissions
7. The option `-x` removes groups or users from a given ACL. Remove the group 
    *green* from the directory  *testpermissions*.`bash
cd ~/testpermissions
setfacl -m o:rwx coolcode
getfacl coolcode
8. Remove permissions for *other*
```bash
setfacl -m o:--- coolcode
`Model 2: ACLs are not restricted to the (user, group, other)
---

10. You can add multiple users and groups with permissions specific to each.
    1. Add user ubuntu with `rwx` to the ACL for file named *coolcode*
    2. Reset all permissions to default`bash
cd ~/testpermissions
getfacl coolcode
setfacl -m u:ubuntu:rwx coolcode
# (Alternative method)
setfacl -x u:ubuntu: coolcode
setfacl -b coolcode
11. Removing acl permissions selectively. Remove higher level group permissions,
    but individual groups can still keep permissions
    1. Remove permissions for *groups* (but check that *green* still has
       permissions)
    2. Remove permissions for *green*
```bash
cd ~/testpermissions
getfacl coolcode
setfacl -m g::--- coolcode
getfacl coolcode
setfacl -m g:green:--- coolcode
getfacl coolcode
12. Transfer of ACL attributes. Transfer takes two steps. In this example, the
    specification file is called  *acl*.
    1. Create a file containing the ACL to be used.
    2. Read the contents of the file into `setfacl` to set the ACL for a directory
       `-M` tells `setfacl` to modify the acl list for the specified directory.`bash
echo "g:green:rwx" > acl
mkdir dirtwo
setfacl -M acl dirtwo
13. Copying ACLs from One File or Directory to Another
    1. Copy an ACL from one directory to another with the `-M` option.
    2. See that permissions changed (*green* is gone, *blue* is added)<br/>
       **-b** clear ACLs<br/>
       **-n** do not recalculate effective rights mask<br/>
       **-** read from stdin`bash
getfacl ../testpermissions | setfacl -b -n -M - dirtwo
getfacl dirtwo
14. Transfer ACLs for a file
    1. Create a new file with any content check permissions (as user  *ubuntu*)
    2. See the `+` at the end, indicating acl is setup on this file`bash
touch coolcode
getfacl coolcode
ls -lt coolcode
15. Copying an ACL into the Default ACL. Once the ACLs are the way they need
    to be, they can be set as the default. Defaults are inherited, so a new
    directory will inherit the defaults of the parent directory.

```bash
touch coolcode2
getfacl coolcode | setfacl --set-file=- coolcode2
getfacl coolcode
getfacl coolcode2
16. Recursively reset ACLs for directory

```bash
getfacl -a dirtwo | setfacl -d -M- /home/ubuntu
cd ..
setfacl -R -b testpermissions
getfacl testpermissions
17. ACL Masking. Use `m` for *mask*, list available permissions. This limits
    everyone. The *group* may have `rwx`, the *mask* permits `r-x`. For example,
    the group with `rw-`, with this mask, ends up with `r--`.`bash
setfacl -m m:r-x coolcode
18. Multiple users. Grant an additional user *cm* `rwx` access and verify that
    the permission is applied. Change to user cm, edit and add data to file, then
    logout. As user *ubuntu*, remove/reset the access permissions. Change to user
    *cm*, edit and add data to file`bash
setfacl -m u:cm:rwx coolcode
su cm
echo "I am testing" > coolcode
cat coolcode
Output: I am testing
exit
setfacl -m u:cm:--- coolcode
su cm
echo "test2" >> coolcode
bash: coolcode: Permission denied
exit
`AppArmor
===

AppArmor is a Mandatory Access Control (MAC) system which is a kernel Linux
security module (LSM) enhancement to confine programs to a limited set of
resources. AppArmor's security model is to bind access control attributes to
programs rather than to users. AppArmor confinement is provided via profiles
loaded into the kernel, typically on boot. AppArmor profiles can be in one of
two modes: *enforcement* and *complain*. Profiles loaded in enforcement mode
will result in enforcement of the policy defined in the profile as well as
reporting policy violation attempts (either via syslog or auditd). Profiles
in *complain* mode will not enforce policy but instead report policy violation
attempts. *Complain* mode creates entries in */var/log/messages*. This mode is
convenient for developing profiles.

AppArmor differs from some other MAC systems on Linux: it is path-based, it
allows mixing of  *enforcement* and *complain* mode profiles, it uses include
files to ease development, and it has a far lower barrier to entry than other
popular MAC systems.

Core AppArmor functionality is in the mainline Linux kernel from 2.6.36 onwards;
work is ongoing by AppArmor, Ubuntu and other developers to merge additional
AppArmor functionality into the mainline kernel.

AppArmor confinement in Ubuntu is application specific with profiles available
for specific binaries. With each release, more and more profiles are shipped
by default, with more planned.

If a profile is not available for an application, users may create a profile
and add it to **/etc/apparmor.d*. If a profile is not defined for a particular
binary, the binary is not confined.

To list protocols installed on the system execute:
```bash
sudo apparmor_status
`AppArmor Parser
---
`apparmor_parser` is used as a general tool to compile, and manage AppArmor
policy, including loading new  *apparmor.d* profiles into the Linux kernel.

AppArmor profiles restrict the operations available to processes.

The profiles are loaded into the Linux kernel by the `apparmor_parser`
program. The profiles may be specified by file name or a directory name
containing a set of profiles. If a directory is specified then the
`apparmor_parser` will try to do a profile load for each file in the
directory that is not a *dot file*, or explicitly blacklisted (*.dpkg-new,
*.dpkg-old, *.dpkg-dist, *-dpkg-bak, *.rpmnew, *.rpmsave, *orig, *.rej,
*~). The `apparmor_parser` will fall back to taking input from standard
input if a profile or directory is not supplied.

AppArmor Profiles
---

Profiles are stored in `/etc/apparmor.d/`. They are named after the full
path to the executable they profile, replacing `/` with `.` . For example
`/etc/apparmor.d/bin.ping` is the profile for `ping` in `/bin`.

There are two main types of entries used in profiles:
1. Path Entries determine what files an application can access.
2. Capability entries determine what privileges a process can use.

The following example is the profile for ping, located in
`/etc/apparmor.d/bin.ping`.`c
#include <tunables/global>
  /bin/ping flags=(complain) {
  #include <abstractions/base>
  #include <abstractions/consoles>
  #include <abstractions/nameservice>

  capability net_raw,
  capability setuid,
  network inet raw,

  /bin/ping mixr,
  /etc/modules.conf r,
}
```

`#include <tunables/global>` Includes the file `global` in the directory
tunables. This allows statements pertaining to multiple applications to
be placed in a common file.

`/bin/ping flags=(complain)` sets the path to the profiled program and
sets the mode to  *complain*.

`capability net_raw` allows the application access to the CAP_NET_RAW
POSIX.1e capability.

`/bin/ping mixr` allows the application read and execute access to the file.

`/etc/modules.conf r`. The `r` gives the application read privileges for
`/etc/modules.conf`.

Properties of AppArmor profiles:
* Profiles are simple text files.
* Comments are supported and introduced with `#`.
* Absolute paths as well as file globbing can be used when specifying file
  access.
* Various access controls for files are present. From the profile we see `r`
  (read), `w` (write), `m` (memory map as executable), `k` (file locking),
  and `l` (creation hard links). There are others not demonstrated in this
  profile, including `ix` (execute and inherit this profile), `Px` (execute
  under another profile, after cleaning the environment), and `Ux` (execute
  unconfined, after cleaning the environment), along with others.
* Access controls for capabilities are present.
* Access controls for networking are present.
* Specificity in rule matching, i.e. the most specific rule matches (ex.
  access to @{HOME}/bin/bad.sh is denied with auditing due to
  `audit deny @{HOME}/bin/** mrwkl,` even though general access to
  @{HOME} is permitted with `@{HOME}/** rw,`).
* Include files will include the contents of a file inline to the policy.
  They are supported to ease development and simplify profiles (i .e.
  `#include <abstractions/base>`, `#include <abstractions/nameservice>`,
  `#include <abstractions/user-tmp>`).
* Variables can be defined and manipulated outside the profile
  (`#include <tunables/global>` with *@{PROC}* and *@{HOME}*).
* AppArmor profiles are easy to read and audit.

AppArmor Status
---

To check AppArmor:
```bash
sudo apparmor_status
`The command shows whether AppArmor is loaded and lists all profiles in each mode.

To reload a profile:
```bash
cat /etc/apparmor.d/profile.name | sudo apparmor_parser -a
`AppArmor Changing Mode
---

Enforce mode:
```bash
sudo aa-enforce /path/to/bin
`Return to complain (un-enforced) mode:
```bash
sudo aa-complain /path/to/bin
`The commands are in the `apparmor-utils` package which must be installed.

AppArmor Changing Mode for All
---

Enforce mode for all configured applications:
```bash
sudo aa-enforce /etc/apparmor.d/*
`Return to complain mode:
```bash
sudo aa-complain /etc/apparmor.d/*
`AppArmor Lab
===

In this lab we will enable AppArmor for Firefox. We will use `LABVM` throughout the lab.

1. Run apparmor status. There are profiles for many commands.`bash
sudo aa-status
# or
sudo apparmor_status
2. How many profiles are in complain mode?<br/>
   How many profiles are in enforce mode?
```bash
$ sudo aa-status | grep ^[0-9]
51 profiles are loaded.
40 profiles are in enforce mode.
11 profiles are in complain mode.
32 processes have profiles defined.
32 processes are in enforce mode.
0 processes are in complain mode.
0 processes are unconfined but have a profile defined.
```
> There are 11 profiles in `complain` mode<br/>
> There are 40 profiles in `enforce` mode
3. Which processes have profiles defined?
> See output from `aa-status` above and identify processes such as `/sbin/dhclient`
> `/usr/bin/man` and `/usr/sbin/tcpdump` which have profiles defined
4. Install the apparmor utilities.`bash
sudo apt install apparmor-utils
5. Enable the Firefox profile and confine Firefox with AppArmor.
   You will need to remove the Firefox profile from `apparmor.d/disable`.`bash
sudo apt update
sudo apt install -y firefox
sudo rm /etc/apparmor.d/disable/usr.bin.firefox
cat /etc/apparmor.d/usr.bin.firefox | sudo apparmor_parser -a
6. Put Firefox in enforce mode.

```bash
sudo aa-enforce /etc/apparmor.d/usr.bin.firefox
7. Run apparmor status and verify that Firefox is in enforce mode.

```bash
sudo apparmor_status
8. Disable the Firefox profile.

```bash
sudo ln -s /etc/apparmor.d/usr.bin.firefox /etc/apparmor.d/disable/
sudo apparmor_parser -R /etc/apparmor.d/usr.bin.firefox
9. Run apparmor status and verify that Firefox is no longer enforced.

```bash
sudo apparmor_status
```

# Snappy

In this section you will learn to:
* Define snaps
* Use snap commands
* Define Ubuntu core
* Explain Canonical LivePatch service

Snaps
===

Ubuntu has a new package format called **snap**. This is not a replacement for
**deb** packages and traditional package management, but rather an additional
option. They can be provided by any vendor and integrate with any other snaps
through secure, *well-defined* interfaces.

Snaps are intended to be used by application developers to deliver software
updates directly to users. They also have several use case advantages.

A **snap** is a universal Linux package. Snaps work on most distributions.
Snaps are faster to install, easier to create, safer to run, and they update
automatically and transactionally so your app is always fresh and never
broken.

The public collection of snaps includes the latest and best apps from GitHub
and the whole world of Linux apps.

Snaps help developers with:
* A simple packaging format
* Streamlined process
* Including all dependencies in the package rather than by linking to shared
  libraries and other packages that may change
* Transactional updates, just the delta needs to be downloaded

Snaps on the filesystem are immutable and almost impossible to hack because
they are read-only and digitally signed. Their integrity can be verified any
time and your system will be secure, from startup to shutdown.

Contents of Snaps
---

A snap package consists of the software and all of its dependencies. This
provides isolation between apps, enables an app store model for finding and
managing software, and eliminates the packaging bottleneck that exists with
traditional software repositories.

Snap Commands
---

To install a snap package, use:
```shell
snap install packagename
`To find what Snaps are available, use:
```shell
snap find
`To narrow the list down, use:
```shell
snap find searchterm
`To learn what Snaps are currently instal led, use:
```shell
snap list
`To update to the latest release of a Snap, use:
```shell
snap refresh packagename
`To remove an installed Snap, use:
```shell
snap remove packagename
`Exercise
---

Take the Snappy tour at https://snapcraft.io. If you have an application and
you are interested in creating snaps, the information is provided at this link.

Ubuntu Core
===

Ubuntu Core is a tiny, transactional version of Ubuntu for IoT devices and
large container deployments. It runs a new breed of super-secure, remotely
upgradeable Linux app packages known as **snaps**. And, it's trusted by
leading Internet of Things (IoT) players, from chipset vendors to device
makers and system integrators.

Smart IoT applications include digital signage, robotics, and industrial
gateways.

Why use Ubuntu Core?
---

Ubuntu Core uses the same kernel, libraries and system software as classic
Ubuntu. You can develop snaps on your Ubuntu PC just like any other
application. The difference is that it has been built for the Internet of
Things.

Automatic updates ensure that critical security issues are addressed in the
field, even if a device is unattended.

Ubuntu Core is free. It can be distributed at no cost, with a custom kernel,
broad support package (BSP), and suite of apps to suit your device.

Transactional over-the-air updates **with full rollback features** cut the
costs of managing devices in the field.

You can easily deploy your own app store and curate a suite of certified apps
from an open ecosystem.

![Ubuntu Core Architecture](./images/ubuntu-core.png)

Ubuntu Core is different from classic Ubuntu distributions. It is a purposely
lightweight and transactionally updated system, with security at its heart.
The fundamental unit is the **snap** - a self contained, isolated and protected
bit of code that performs a well-defined set of functions. Even the kernel and
core are snaps.

Ubuntu Core is small because it is a base filesystem. Apps are delivered as
snaps, alongside a free choice of container runtimes and coordination systems.
And because it has a smaller attack surface, it is much more secure.

Canonical Livepatch Service
===

Starting with Ubuntu 16.04 LTS's, the Ubuntu Linux kernel includes an important
new security capability -- the ability to modify the running Linux kernel code,
without rebooting, through a mechanism called kernel livepatch.

Livepatch is an authenticated, encrypted, signed stream of Linux livepatches that
apply to the  **64-bit Intel/AMD** and **ARM64** architectures of Ubuntu LTS releases
(including Focal 20.04, Jammy 22.04, and Noble 24.04) Linux kernel, addressing the 
highest and most critical security vulnerabilities, without requiring a reboot in 
order to take effect. This is particularly useful for Container hosts -- Docker, 
LXD, etc. -- as all of the containers share the same kernel and, thus, all instances 
benefit from this capability.

To install Livepatch on a fully up-to-date 64-bit Ubuntu LTS system (20.04, 22.04, or 24.04):
1. Go to https://ubuntu.com/livepatch and retrieve your livepatch token.<br/>
   You will need to create a Launchpad login first at https://login.launchpad.net.
   Using this login you can sign-up for the Livepatch service. We will be using `LABVM` throughout this lab.
2. Install the canonical-livepatch snap.`shell
sudo snap install canonical-livepatch
3. Enable the service with your token.

```shell
sudo canonical-livepatch enable [TOKEN]
4. To check the status at any time use:
```
~# canonical-livepatch status --verbose
client-version: 8.0.3
machine-id: <GUID>
machine-token: [TOKEN]
architecture: x86_64
cpu-model: Intel(R) Core(TM) i7-6700HQ CPU @ 2.60GHz
last-check: 2018-09-12T02:03:21.94799735-07:00
boot-time: 2018-09-11T23:49:20-07:00
uptime: 2h14m13s
status:
- kernel: 4.15.0-34.37-generic
  running: true
  livepatch:
    checkState: checked
    patchState: nothing-to-apply
    version: ""
    fixes: ""
```

# APPENDICES

Time Synchronization
===

Network Time Protocol (NTP)
---

Why should time be synchronized? If you have communicating programs running on
different computers, time still should advance if you switch from one computer
to another. Obviously, if one system is ahead of the others, the others are
behind that particular one. From the perspective of an external observer,
switching between these systems would cause time to jump forward and back, a
non-desirable effect. As a consequence, isolated networks may run their own
wrong local time, but as soon as you connect to the Internet, effects will be
visible. Just imagine some email message arrived five minutes before it was
sent and there even was a reply two minutes before the message was sent.

Even on a single computer, some applications have trouble when the time jumps
backwards. For example, database systems using transactions and crash recovery
like to know the time of the last good state. On most systems, time will slowly
drift.

TCP/IP protocol is used for synchronising time over a network. NTP uses UDP,
Port 123. The client requests the current time from a server to set its own
clock. With multiple NTP servers, the tier one NTP servers are connected to
atomic clocks. Tier-two and tier-three servers spread the load of actually
handling requests across the Internet.

**NTP** needs some reference clock that defines the true time to operate. All
clocks are set towards that true time. (It will not just make all systems
agree on some time, but will make them agree upon the true time as defined by
some standard.) It uses *UTC* as reference time. It is a faulttolerant
protocol that will automatically select the best of several available time
sources to synchronize to.

Multiple candidates can be combined to minimize the accumulated error. It is
highly scalable. A synchronization network may consist of several reference
clocks. Each node of such a network can exchange time information either
bidirectional or unidirectional. Propagating time from one node to another
forms a hierarchical graph with reference clocks at the top.

**NTP** can select the best candidates to build its estimate of the current
time. The protocol is highly accurate, using a resolution of less than a
nanosecond (about 2^-32 seconds). It can use measurements from the past to
estimate current time and error. It maintains estimates for the accuracy of
the local time.

Timedatectl
---

In recent Ubuntu releases, `timedatectl` replaces `ntpdate` and `ntpd` (ntp).
By default, `timedatectl` syncs the time once on boot. It uses socket activation
to recheck once network connections become active. If `ntpdate` / `ntp` is
installed, `timedatectl` steps back to let you keep your old setup. This ensures
that no two time syncing services are fighting and retains any kind of old
behaviour/config that you had through an upgrade. But it also implies that on an
upgrade from a former release, ntp/ntpdate might still be installed and
therefore renders the new systemd based services disabled.

Timesyncd
---

In recent Ubuntu releases, `timesyncd` replaces the client portion of `ntpd`.
By default, `timesyncd` regularly checks and keeps the time in sync.

NTP Status
---

The NTP server to fetch time for `timedatectl` and `timesyncd` can be specified
in `/etc/systemd/timesyncd.conf` and with flexible additional config files in
`/etc/systemd/timesyncd.conf.d/`.

The current status of NTP time configuration via `timedatectl` and `timesyncd`
can be checked with `timedatectl status`.`shell
# timedatectl status
                      Local time: Wed 2018-09-12 05:05:00 PDT
                  Universal time: Wed 2018-09-12 12:05:00 UTC
                        RTC time: Wed 2018-09-12 12:05:00
                       Time zone: US/Pacific (PDT, -0700)
       System clock synchronized: yes
systemd-timesyncd.service active: yes
                 RTC in local TZ: no
`If NTP is installed, the line "NTP synchronized" is set to yes.

Time Synchronization Lab
===

In this lab you will learn to troubleshoot NTP.

1. Check to see if NTP is installed. If not present install it`shell
dpkg-query --list ntp\*
sudo apt install ntp
```
> and look for ntp
2. Write a command to figure out which servers you're trying to use.`shell
grep ^server /etc/ntp.conf
# or 
grep ^server /etc/ntp.conf.dhcp
3. Write a command using the numeric option to determine what NTP is doing.

```shell
ntpq --numeric --peers
```
> `--numeric` removes the DNS lookups.
4. Check the list of NTP peers with which you are communicating.`shell
ntpq -c lpeer
`rsyslog
===

**rsyslog** is a continuation of the syslog (and accompanying sysklogd daemon)
log manager found in most Linux distributions. It is backwards compatible with
syslog's configuration file, syslog.conf, and adds additional functionality
such as extensibility with modules and rules for discrete log filtering for
different applications and sources. It also provides some additional
functionality over *syslog*, such as native email alert, SSL encryption, and
multiple configuration file support.

Configure rsyslog
---

**rsyslog** is installed by default by all Ubuntu images. If for any reason
it is not installed, it can be installed with:
```shell
sudo apt-get install rsyslog
`An additional documentation package can be installed using:
```shell
sudo apt install rsyslog-doc
`Check that it's running with:
```shell
$ sudo service rsyslog status
* rsyslog.service - System Logging Service
   Loaded: loaded (/lib/systemd/system/rsyslog.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2018-09-11 23:49:29 PDT; 5h 25min ago
     Docs: man:rsyslogd(8)
           http://www.rsyslog.com/doc/
 Main PID: 1111 (rsyslogd)
    Tasks: 4 (limit: 4633)
   CGroup: /system.slice/rsyslog.service
           `-1111 /usr/sbin/rsyslogd -n
`The rsyslogd process can be seen with ps:
```shell
$ ps aux | grep rsyslogd
syslog     1111  0.0  0.1 263036  4444 ?        Ssl  Sep11   0:01 /usr/sbin/rsyslogd -n
`After any configuration changes, a restart of the rsyslog service is required:
```shell
sudo service rsyslog restart
`The main configuration file for rsyslog by default is `/etc/rsyslog.conf`.
Additional configuration can be made in files placed in the `/etc/rsyslog.d`
directory. The rsyslog configuration file is usually structured in order of
the following sections: *modules*, *global directives*, *filter rules*, and
comments for explanation (#). Modules and global directives are specified
in the same way: one line at a time, starting with a dollar sign ($).

Modules
---

The Modules section specifies the modules and plugins to be loaded. These
affect the operation of the rsyslogd daemon and the sources it reads from
and logs to.

For example, the `imuxsock` and `imklog` modules are here by default:
* the `imuxsock` module handles logging on local system processes
* the `imklog` module handles kernel logging.`rsyslog
module(load="imuxsock") # provides support for local system logging
module(load="imklog" permitnonkernelfacility="on")
`Other available modules and plugins can be found in the rsyslog
documentation and provide support for different input and output formats
including different databases like MySQL and PostgreSQL.

Global Directives
---

Global directives are configuration options for the rsyslogd daemon. These
are specified in the same way as modules, on individual lines with a dollar
sign ($). These are outside of the scope of this documentation, but here are
some examples from a default configuration:
```rsyslog
# Use traditional timestamp format.
$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat
$RepeatedMsgReduction on

# Set the default permissions for all log files.
$FileOwner syslog
$FileGroup adm
$FileCreateMode 0640
$DirCreateMode 0755
$Umask 0022
$PrivDropToUser syslog
$PrivDropToGroup syslog

# Where to place spool and state files
$WorkDirectory /var/spool/rsyslog

# Include all config files in /etc/rsyslog.d/
$IncludeConfig /etc/rsyslog.d/*.conf
`Filter Rules
---

A filtering rule is made up of two main sections known as a selector and
action, separated by spaces or tabs. For example:
```rsyslog
daemon.* /var/log/daemon. log
`The selector section on the left side is further broken down into another
two keywords on either side of a period (`.`). The first is called the
facility and refers to different parts of the the operation system defined
by the traditional Syslog protocol. The second keyword is the priority,
from a list ranging from `debug` to `panic` also specified in the syslog
protocol. Use an asterisk for `wildcard` allowing any facility or priority.

Multiple selectors can be specified in one rule using a semicolon to separate
them. Each of them will be subject to the same action. The right-hand section
of the rule is the action and determines what will be done with the messages
filtered by the selector criteria. The available actions are partly determined
by the output modules explained earlier, but most commonly, the action simply
contains the output destination of the messages caught by the filter, i.e. a
file in `/var/log`.

Examples of different facilities that can be specified as part of a filter rule:

Keyword | Description
---     | ----
kern    | Kernel messages
user    | User-level messages
daemon  | System daemon messages
auth    | Security/authorization messages

Examples of priorities:

Keyword | Description
---     | ---
emerg   | Emergency: system is unusable
alert   | Alert: action must be taken immediately
warning | Warning: warning conditions
notice  | Notice: normal but significant condition
info    | Information: information messages

rsyslog Lab
===

In this lab you will work with rsyslog. rsyslog adds to the functionality of
syslog.

1. View the logfiles on the system.<br/>
   Ex. kern. log, syslog, auth. log
> Most logs are under `/var/log`.`shell
cd /var/log
ls -lt
2. Some applications with logging (audit-trail/security) requirements may have
   their own subdirectries.<br/>
> Ex. apparmor apt and installer
3. Two files with error are kern and dpkg (install and boot). Look at these errors.

```shell
grep -rli "error" *.log
grep -rli "failed" *.log
4. Install rsyslog.

```shell
sudo apt install rsyslog
5. Get additional documentation.

```shell
sudo apt install rsyslog-doc
6. Make sure it's running.

```shell
sudo service rsyslog status
7. Check the rsyslogd process.

```shell
ps aux | grep rsyslogd
8. Restart rsyslog service (required for changes).

```shell
sudo service rsyslog restart
ps -ef | grep rsyslogd
`How to collect logs and data for a bug validation
===

Ubuntu has both **apport** and **sosreport** way of saving log files. The
recommended way is using  **apport** because it is more Ubuntu specific and
has an integration with **Launchpad**.

**apport-bug** reports problems to your distribution's bug tracking system.
Use apport to collect local information about your system to help the
developers fix problems and avoid unnecessary question/answer turnarounds.

You should always start with running `apport-bug` without arguments, which
will present a list of known symptoms. This will generate the most useful
bug reports. If there is no matching symptom, you need to determine the
affected program or package yourself. You can provide a package name or
program name to `apport-bug`, ex.:
```shell
apport-bug linux
`apport
---

Debugging program crashes without any automated tools is time consuming and
hard for both developers and users. Many program crashes remain unreported or
unfixed because:
* Many crashes are not easily reproducible.
* End users do not know how to prepare a report that is really useful for
  developers, like building a package with debug symbols, operating gdb, etc.
* A considerable part of bug triage is spent with collecting relevant
  information about the crash itself, package versions, hardware architecture,
  operating system version, etc.
* There is no easy frontend which allow users to submit detailed problem reports.
* Existing solutions like `bug-buddy` or `krash` are specific to a particular
  desktop environment, are non-trivial to adapt to the needs of a distribution
  developer, do not work for crashes of background servers (like a database or
  an email server), and do not integrate well with existing debug packages that
  a distribution might provide.

Apport is a system which
* intercepts crashes right when they happen the first time,
* gathers potentially useful information about the crash and the OS environment,
* can be automatically invoked for unhandled exceptions in other programming
  languages (ex. in Ubuntu this is done for Python),
* can be automatically invoked for other problems that can be automatically
  detected (ex. Ubuntu automatically detects and reports package installation/
  upgrade failures from update-manager),
* presents a UI that informs the user about the crash and instructs them on how
  to proceed,
* and is able to file non-crash bug reports about software, so that developers
  still get information about package versions, OS version etc.

Starting with Ubuntu 12.04 LTS, **apport** itself is running at all times because
it collects crash data for  **whoopsie** (see ErrorTracker). However, the crash
interception component is still disabled. To enable it permanently:
```shell
sudo nano /etc/apport/crashdb.conf
```
> and add a hash symbol `#` in the beginning of the following line:

```python
'problem_types' : ['Bug' , 'Package'],
`To disable crash reporting just remove the hash symbol.

sosreport
---

**sosreport** generates a compressed tar archive of diagnostic information
from the running system. The archive may be stored locally or centrally for
recording or tracking purposes or may be sent to technical support representatives,
developers or system administrators to assist with technical fault-finding and
debugging.

**sos** is modular in design and is able to collect data from a wide range of
subsystems and packages that may be installed. An XML or HTML report summarizing
the collected information is optionally generated and stored within the archive.

Apport and sosreport Lab
---

1. Check to see if sosreport is installed. If not, install it.`shell
dpkg -l | grep sosreport
sudo apt install sosreport
2. Run sosreport and view the results.

```shell
$ sudo sosreport

sosreport (version 3.5)

This command will collect system configuration and diagnostic
information from this Ubuntu system. An archive containing the collected
information will be generated in /tmp/sos.4ra4xrmo.

For more information on Ubuntu visit:

  http://www.ubuntu.com/

The generated archive may contain data considered sensitive and its
content should be reviewed by the originating organization before being
passed to any third party.

No changes will be made to system configuration.

Press ENTER to continue, or CTRL-C to quit.

Please enter your first initial and last name [ubuntu-juju]:
Please enter the case id that you are generating this report for []:

 Setting up archive ...
 Setting up plugins ...
Not all environment variables set. Source the environment file for the user intended to connect to the OpenStack environment.
 Running plugins. Please wait ...

  Running 60/60: zfs...              .
Creating compressed archive...

Your sosreport has been generated and saved in:
  /tmp/sosreport-ubuntu-juju-20180912055513.tar.xz

The checksum is: f987b877aac55d03579057a64f7d9681

Please send this file to your support representative.
```
> Review the file in `/tmp/`
3. Review some of the configuration fi les in the apport directory.`shell
ls -l /etc/apport
cat /etc/apport/crashdb.conf
```
