# * respawned fork from kholia to reflect updates for Catalina

# * WIP implement bash script to deploy virsh based virt-manager install package management driven install qemu
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

# * clover-r5070.iso.catalina.svga.iso (partial q35 acceleration mount as SATA qcow2 not as a CDROM iso "files encapsulated in .iso based container")



https://drive.google.com/file/d/1vcA6ti6SyEBYZ48DRXSFvpsJPZkQ2d5-/view


(iso modified from nick sherlock's iso again from kholia's patch -> resoloution fixed with this iso ^ for kholia's OVMF patch from https://www.nicksherlock.com/2019/10/installing-macos-catalina-10-15-on-proxmox-6/ )

# * to mount to edit
<br>
* where nbd5 is choosen mount point (where qemu-nbd is installed, where kernel is built with NBD as a block device)
<br>

modprobe nbd max_part=8
<br>
qemu-nbd --connect=/dev/nbd5 clover-r5070.iso.catalina.iso
<br>
mkdir /mnt/catalina-5070
<br>
mount /dev/nbd5p1 /mnt/catalina-5070
<br>
cd /mnt/catalina-5070
<br>
EFI/CLOVER/kexts/Other <-- for custom kexts
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
(q35-2.11)
<br>
penryn cpu type required
<br>
manual cirrus typed in video card model 
<br>
change from spice to vnc
<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/raw/master/Screenshot%20at%202019-12-30%2021-34-27.png" width="800"></p>

boot options enabled "esc at load" OVMF platform settings? resoloution internal

<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/raw/master/OVMF-CONFIG-RESO.png" width="800"></p>
<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/raw/master/Screenshot%20at%202019-12-30%2021-34-57.png" width="800"></p>
ignore_msrs and unsafe_interrupts required to bypass serial console loading for verbose boot testing

<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/raw/master/KVM-modprobe.png" width="800"></p>


<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/blob/master/networking-qemu-virt-manager.png" width="800"></p>
<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/blob/master/networking-mojave.png" width="800"></p>



### Note

to add q35-speed-patch for libvirt, (4200 and 4920)


# Speedpatch for q35 libvirt to remove slow performance with KVM

svn co -r 4920 svn://svn.code.sf.net/p/cloverefiboot/code Clover


http://s3.nicksherlock.com/forumposts/2016/clover-r4061-qemu-cpu-speed-patch.diff

Download this patch to “edk2/Clover”. Change into that directory and run:

svn patch clover-r4061-qemu-cpu-speed-patch.diff


Change into the “edk2/Clover” directory, and run:

./ebuild.sh

The default options, which use XCode to build an X64 bootloader, are perfect for us.

After that completes, run “cd CloverPackage; ./makepkg”. This will produce an installable package for us in “edk2/Clover/CloverPackage/sym/Clover_v2.4k_r4920.pkg.”.


# ** FORKED THIS OUT OF IT DRIVING ME TO BRINK of,.q35?

<br>
todo: move q35-dsdt.aml ->
<br>

q35-acpi-dsdt.aml /Volumes/ESP/EFI/CLOVER/ACPI/origin/
<br>


(these qcow2's are built for KVM? q35 model in libvirt)
<br>

(q35-acpi-dsdt.aml appears missing?)
<br>

@q35-acpi-dsdt.aml EFI/CLOVER/ACPI/origin/
<br>

(in the clover image)'
# * Clover bootloader settings based off Mojave Olarila raw intel image distro install
<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/blob/master/clover-settings.png" width="800"></p>
<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/blob/master/patch-apic.png" width="800"></p>

<p align="center"><img src="https://github.com/c4pt000/OSX-KVM/blob/master/patch-apic.png.apic-ext.png" width="800"></p>

This `README` documents the new method to install macOS. The older `README` is
available [here](README-OLD.md).

This new method does *not* require an existing physical/virtual macOS
installation. However, this `new method` requires internet access during the
macOS installation process. This limitation may be addressed in a future
commit.

Note: All blobs and resources included in this repository are re-derivable (all
instructions are included!).

Note: Checkout [ideas.md](ideas.md). This project can always use your help,
time and attention.


### Requirements

* A modern Linux distribution. E.g. Ubuntu 18.04 LTS 64-bit.

* QEMU > 2.11.1

* A CPU with Intel VT-x / AMD SVM support is required

* A CPU with SSE4.1 support is required for macOS Sierra

* A CPU with AVX2 support is required for macOS Mojave

Note: Older AMD CPU(s) are known to be problematic. AMD FX-8350 works but
Phenom II X3 720 does not. Ryzen processors work just fine.


### Installation Preparation

* KVM may need the following tweak on the host machine to work.

  ```
  # echo 1 > /sys/module/kvm/parameters/ignore_msrs
  ```

  To make this change permanent, you may use the following command.

  ```
  $ sudo cp kvm.conf /etc/modprobe.d/kvm.conf
  ```

* Install QEMU and other packages.

  ```
  sudo apt-get install qemu uml-utilities virt-manager dmg2img git wget
  ```

  This step may need to be adapted for your Linux distribution.

