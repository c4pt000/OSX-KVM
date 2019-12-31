### clover-r5070.iso.imac131.svga.iso (intel q35 accel + VMsvga + imac13,1 store login)

https://drive.google.com/file/d/1VeiY7Dt_uk_8KwVNQ-b2riPaUZcih9Tu/view

### Requirements

* A modern Linux distribution. E.g. Ubuntu 18.04 LTS 64-bit.

* QEMU > 2.11.1

* A CPU with Intel VT-x / AMD SVM support is required

* A CPU with SSE4.1 support is required for macOS Sierra

* A CPU with AVX2 support is required for macOS Mojave

Note: Older AMD CPU(s) are known to be problematic. AMD FX-8350 works but
Phenom II X3 720 does not. Ryzen processors work just fine.


### Installation Preparation

<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/raw/master/KVM-modprobe.png" width="800"></p>

ignore_msrs and unsafe_interrupts required to bypass serial console loading for verbose boot testing

* KVM may need the following tweak on the host machine to work.

  ```
  # echo 1 > /sys/module/kvm/parameters/ignore_msrs
  ```

  To make this change permanent, you may use the following command.

  ```
  $ sudo cp kvm.conf /etc/modprobe.d/kvm.conf
  ```
* editing virsh machine settings with nano opposed to vi standard


```
echo "export EDITOR=nano" >> /root/.bashrc

#where user is your username
echo "export EDITOR=nano" >> /home/user/.bashrc

```
* Install QEMU and other packages.

  ```
  sudo apt-get install qemu uml-utilities virt-manager dmg2img git wget
  ```

  This step may need to be adapted for your Linux distribution.




### * respawned fork from kholia to reflect updates for Catalina

### * WIP implement bash script to deploy virsh based virt-manager install package management driven install qemu
<br>
<br>
<p align="center"><img src="https://raw.githubusercontent.com/c4pt000/OSX-KVM/master/fetch-download-catalina-1024x345.png" width="600"></p>

```
qemu-img convert BaseSystem.dmg -O raw Catalina-installer.iso
```
<br>

<br>
<br>
<br>
<br>

### * clover-r5070.iso.imac131.svga.iso (partial q35 acceleration mount as SATA qcow2 not as a CDROM iso "files encapsulated in .iso based container")


https://drive.google.com/file/d/1VeiY7Dt_uk_8KwVNQ-b2riPaUZcih9Tu/view

<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/blob/master/Screenshot%20at%202019-12-30%2021-35-35.png" width="800"></p>



(iso modified from nick sherlock's iso again from kholia's patch -> resoloution fixed with this iso ^ for kholia's OVMF patch from https://www.nicksherlock.com/2019/10/installing-macos-catalina-10-15-on-proxmox-6/ )

### * to mount to edit
### * where nbd5 is choosen mount point (where qemu-nbd is installed, where kernel is built with NBD as a block device)


```
modprobe nbd max_part=8
qemu-nbd --connect=/dev/nbd5 clover-r5070.iso.catalina.iso
mkdir /mnt/catalina-5070
mount /dev/nbd5p1 /mnt/catalina-5070
cd /mnt/catalina-5070
ls EFI/CLOVER/kexts/Other                     # <-- for custom kexts
```
<br>
<br>
<br>
<br>

Fedora 31 noted

```
/etc/default/grub -> here
grub.conf
 intel_iommu=on iommu=pt cgroup_enable=memory,namespace systemd.unified_cgroup_hierarchy=0 pcie_acs_override=downstream"

```
* at first run during verbose boot the serial output is scrambled, after about 15 seconds the display auto corrects to the proper wrapped resoloution,
<br>
<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/raw/master/scrambled.png" width="800"></p>
<br>
<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/raw/master/scrambled-auto-correct.png" width="800"></p>
## default for display might help when graphics extert visual artifacts, and changing from full screen to windowed otherwise expect graphical instability, without physical graphics card, (other solutions are teamviewer installed in the guest and teamviewer installed in the host, then connecting from teamviewer host side to teamviewer guest side using LAN only)
<br>
<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/raw/master/1920x1080.png" width="800"></p>

<br>
<br>
<br>
<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/raw/master/1920-1080-system-pref-display.png" width="800"></p>

EFI/CLOVER/config.plist

<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/raw/master/YOUR-CLOVER-config-plist-EFI.png" width="800"></p>
virt-manager settings
<br>
<br>

### penryn cpu type required
<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/raw/master/Screenshot%20at%202019-12-30%2021-34-38.png" width="800"></p>


# VGA required type not vmware, not cirrus, not virtio, not QXL
<br>

<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/raw/master/Screenshot%20at%202019-12-30%2021-34-27.png" width="800"></p>

### boot options enabled "esc at load" OVMF platform settings? resoloution internal

<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/raw/master/OVMF-CONFIG-RESO.png" width="800"></p>
<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/blob/master/networking-mojave.png" width="800"></p>

<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/raw/master/Screenshot%20at%202019-12-30%2021-34-57.png" width="800"></p>








