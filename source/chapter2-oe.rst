.. SPDX-License-Identifier: CC-BY-SA-4.0

************
OpenEmbedded
************

This chapter describes specific OpenEmbedded LEDGE build and run.

Supported platfroms
===================
- armv7/ledge-multi-armv7 (qemu, ti-am572x, stm32mp157c-dk2);
- armv8/ledge-multi-armv8 (qemu, synquacer)
- x86-64 (qemu)

Build steps
===========

Download sources:
-----------------

.. code-block:: bash

   repo init --no-clone-bundle --depth=1 --no-tags -u https://github.com/Linaro/ledge-oe-manifest.git -b master
   repo sync

Setup environment and run build:
--------------------------------
armv7 family:
-------------

.. code-block:: bash

   MACHINE=ledge-multi-armv7 DISTRO=rpb source ./setup-environment build-rpb
   bitbake mc:qemuarm:ledge-iot mc:qemuarm:ledge-gateway ${FIRMWARE}

Image files will apper under: armhf-glibc/deploy/images directory.

Generated output will be:

.. code-block:: bash

	├── ledge-qemuarm
	│   ├── arm-trusted-firmware
	│   │   ├── bl1.bin
	│   │   ├── bl1.elf
	│   │   ├── bl2.bin
	│   │   └── bl2.elf
	│   ├── bl1.bin -> arm-trusted-firmware/bl1.bin
	│   ├── bl2.bin -> arm-trusted-firmware/bl2.bin
	│   ├── bl32.bin -> optee/tee-header_v2.bin
	│   ├── bl32_extra1.bin -> optee/tee-pager_v2.bin
	│   ├── bl32_extra2.bin -> optee/tee-pageable_v2.bin
	│   ├── bl33.bin -> u-boot-ledge-qemuarm.bin
	│   ├── dtb
	│   ├── kernel-devicetrees.tgz
	│   ├── ledge-gateway.env
	│   ├── ledge-gateway-ledge-kernel-uefi.wks
	│   ├── ledge-gateway-ledge-qemuarm-20200218104425.bootfs.vfat
	│   ├── ledge-gateway-ledge-qemuarm-20200218104425.bootfs.vfat.gz
	│   ├── ledge-gateway-ledge-qemuarm-20200218104425.qemuboot.conf
	│   ├── ledge-gateway-ledge-qemuarm-20200218104425.rootfs.manifest
	│   ├── ledge-gateway-ledge-qemuarm-20200218104425.rootfs.wic
	│   ├── ledge-gateway-ledge-qemuarm-20200218104425.testdata.json
	│   ├── ledge-gateway-ledge-qemuarm.bootfs.vfat -> ledge-gateway-ledge-qemuarm-20200218104425.bootfs.vfat
	│   ├── ledge-gateway-ledge-qemuarm.bootfs.vfat.gz
	│   ├── ledge-gateway-ledge-qemuarm.manifest -> ledge-gateway-ledge-qemuarm-20200218104425.rootfs.manifest
	│   ├── ledge-gateway-ledge-qemuarm.qemuboot.conf -> ledge-gateway-ledge-qemuarm-20200218104425.qemuboot.conf
	│   ├── ledge-gateway-ledge-qemuarm.testdata.json -> ledge-gateway-ledge-qemuarm-20200218104425.testdata.json
	│   ├── ledge-gateway-ledge-qemuarm.wic -> ledge-gateway-ledge-qemuarm-20200218104425.rootfs.wic
	│   ├── ledge-initramfs-ledge-qemuarm.cpio.gz -> ledge-initramfs.rootfs.cpio.gz
	│   ├── ledge-initramfs-ledge-qemuarm.manifest -> ledge-initramfs.rootfs.manifest
	│   ├── ledge-initramfs-ledge-qemuarm.qemuboot.conf -> ledge-initramfs.qemuboot.conf
	│   ├── ledge-initramfs-ledge-qemuarm.testdata.json -> ledge-initramfs.testdata.json
	│   ├── ledge-initramfs.qemuboot.conf
	│   ├── ledge-initramfs.rootfs.cpio.gz
	│   ├── ledge-initramfs.rootfs.manifest
	│   ├── ledge-initramfs.testdata.json
	│   ├── ledge-iot.env
	│   ├── ledge-iot-ledge-kernel-uefi.wks
	│   ├── ledge-iot-ledge-qemuarm-20200218104425.bootfs.vfat
	│   ├── ledge-iot-ledge-qemuarm-20200218104425.bootfs.vfat.gz
	│   ├── ledge-iot-ledge-qemuarm-20200218104425.qemuboot.conf
	│   ├── ledge-iot-ledge-qemuarm-20200218104425.rootfs.manifest
	│   ├── ledge-iot-ledge-qemuarm-20200218104425.rootfs.wic
	│   ├── ledge-iot-ledge-qemuarm-20200218104425.testdata.json
	│   ├── ledge-iot-ledge-qemuarm.bootfs.vfat -> ledge-iot-ledge-qemuarm-20200218104425.bootfs.vfat
	│   ├── ledge-iot-ledge-qemuarm.bootfs.vfat.gz
	│   ├── ledge-iot-ledge-qemuarm.manifest -> ledge-iot-ledge-qemuarm-20200218104425.rootfs.manifest
	│   ├── ledge-iot-ledge-qemuarm.qemuboot.conf -> ledge-iot-ledge-qemuarm-20200218104425.qemuboot.conf
	│   ├── ledge-iot-ledge-qemuarm.testdata.json -> ledge-iot-ledge-qemuarm-20200218104425.testdata.json
	│   ├── ledge-iot-ledge-qemuarm.wic -> ledge-iot-ledge-qemuarm-20200218104425.rootfs.wic
	│   ├── ledge-kernel-uefi-certs.ext4.img
	│   ├── ledge-qemuarm.dtb
	│   ├── modules-ledge-qemuarm.tgz -> modules--mainline-5.3-r0-ledge-qemuarm-20200218104425.tgz
	│   ├── modules--mainline-5.3-r0-ledge-qemuarm-20200218104425.tgz
	│   ├── modules-stripped-ledge-qemuarm-for-debian.tgz
	│   ├── modules-stripped-ledge-qemuarm.tgz -> modules-stripped--mainline-5.3-r0-ledge-qemuarm-20200218104425.tgz
	│   ├── modules-stripped--mainline-5.3-r0-ledge-qemuarm-20200218104425.tgz
	│   ├── optee
	│   │   ├── tee.bin
	│   │   ├── tee-header_v2.bin
	│   │   ├── tee-pageable.bin
	│   │   ├── tee-pageable_v2.bin
	│   │   ├── tee-pager.bin
	│   │   └── tee-pager_v2.bin
	│   ├── u-boot-basic-1.0-r0.bin
	│   ├── u-boot.bin -> u-boot-basic-1.0-r0.bin
	│   ├── u-boot.bin-basic -> u-boot-basic-1.0-r0.bin
	│   ├── u-boot-ledge-qemuarm.bin -> u-boot-basic-1.0-r0.bin
	│   ├── u-boot-ledge-qemuarm.bin-basic -> u-boot-basic-1.0-r0.bin
	│   ├── zImage -> zImage--mainline-5.3-r0-ledge-qemuarm-20200218104425.bin
	│   ├── zImage-for-debian
	│   ├── zImage-ledge-qemuarm.bin -> zImage--mainline-5.3-r0-ledge-qemuarm-20200218104425.bin
	│   └── zImage--mainline-5.3-r0-ledge-qemuarm-20200218104425.bin
	├── ledge-stm32mp157c-dk2
	│   ├── arm-trusted-firmware
	│   │   ├── bl2.bin
	│   │   ├── bl2.elf
	│   │   └── tf-a-stm32mp157c-dk2.stm32
	│   ├── optee
	│   │   ├── tee.bin
	│   │   ├── tee-header_v2.bin
	│   │   ├── tee-header_v2.stm32
	│   │   ├── tee-pageable.bin
	│   │   ├── tee-pageable_v2.bin
	│   │   ├── tee-pageable_v2.stm32
	│   │   ├── tee-pager.bin
	│   │   ├── tee-pager_v2.bin
	│   │   └── tee-pager_v2.stm32
	│   ├── spl
	│   │   └── u-boot-spl.stm32-basic
	│   ├── u-boot-basic.img
	│   └── u-boot-trusted.stm32
	└── ledge-ti-am572x
	    ├── MLO -> MLO-ledge-ti-am572x-1.0-r0
	    ├── MLO-ledge-ti-am572x -> MLO-ledge-ti-am572x-1.0-r0
	    ├── MLO-ledge-ti-am572x-1.0-r0
	    ├── optee
	    │   ├── tee.bin
	    │   ├── tee-header_v2.bin
	    │   ├── tee-pageable.bin
	    │   ├── tee-pageable_v2.bin
	    │   ├── tee-pager.bin
	    │   └── tee-pager_v2.bin
	    ├── u-boot.img -> u-boot-ledge-ti-am572x-1.0-r0.img
	    ├── u-boot-ledge-ti-am572x-1.0-r0.img
	    └── u-boot-ledge-ti-am572x.img -> u-boot-ledge-ti-am572x-1.0-r0.img

armv8 family:
-------------

.. code-block:: bash

   MACHINE=ledge-multi-armv8 DISTRO=rpb source ./setup-environment build-rpb
   bitbake mc:qemuarm64:ledge-iot mc:qemuarm64:ledge-gateway ${FIRMWARE}

x86_64:
-------

.. code-block:: bash

   MACHINE=ledge-qemux86-64 DISTRO=rpb source ./setup-environment build-rpb
   bitbake ledge-iot ledge-gateway

Install and boot procedure
==========================

* DISK="buildid-rootfs.wic"  - WIC image generated on build procedure. Like ledge-gateway-ledge-qemuarm64-20200216225638.rootfs.wic.
* OVMF="QEMU_EFI.fd" - OVMF is an EDK II based project to enable UEFI support for Virtual Machines. OVMF contains sample UEFI firmware for QEMU and KVM.

OVMF firmware for different architectures can be downloaded from here: https://storage.kernelci.org/images/uefi/111bbcf87621/

OE maintains script called 'runqemu'. This script automatically added to the path after source ./setup-environment is done. This script can be used to run
qemu virtual machine with all required parameters to boot from image and run networking. Configuration file ledge-iot-ledge-qemuarm-*.qemuboot.conf is 
generated during the build process.

Usage example usage:

.. code-block:: bash

   runqemu ledge-iot-ledge-qemuarm-20200218104425.qemuboot.conf wic serial

Example boot log:

.. code-block:: bash

	maxim.uvarov@hackbox2:~/build-test-update/build-rpb-mc/armhf-glibc/deploy/images/ledge-qemuarm$ runqemu ledge-iot-ledge-qemuarm-20200218104425.qemuboot.conf wic serial
	runqemu - INFO - Running MACHINE=ledge-qemuarm bitbake -e...
	runqemu - INFO - Overriding conf file setting of STAGING_DIR_NATIVE to /home/maxim.uvarov/build-test-update/build-rpb-mc/tmp-rpb-glibc/work/armv7at2hf-vfp-linaro-linux-gnueabi/defaultpkgname/1.0-r0/recipe-sysroot-native from Bitbake environment
	runqemu - INFO - Continuing with the following parameters:

	MACHINE: [ledge-qemuarm]
	FSTYPE: [wic]
	ROOTFS: [/home/maxim.uvarov/build-test-update/build-rpb-mc/armhf-glibc/deploy/images/ledge-qemuarm/ledge-iot-ledge-qemuarm-20200218104425.rootfs.wic]
	CONFFILE: [/home/maxim.uvarov/build-test-update/build-rpb-mc/armhf-glibc/deploy/images/ledge-qemuarm/ledge-iot-ledge-qemuarm-20200218104425.qemuboot.conf]

	runqemu - INFO - Setting up tap interface under sudo
	[sudo] password for maxim.uvarov: 
	runqemu - INFO - Network configuration: 192.168.7.2::192.168.7.1:255.255.255.0
	runqemu - INFO - Using block virtio drive
	runqemu - INFO - Interrupt character is '^]'
	runqemu - INFO - Running sudo /home/maxim.uvarov/build-test-update/build-rpb-mc/armhf-glibc/work/x86_64-linux/qemu-helper-native/1.0-r1/recipe-sysroot-native/usr/bin/qemu-system-arm -device virtio-net-pci,netdev=net0,mac=52:54:00:12:34:02 -netdev tap,id=net0,ifname=tap0,script=no,downscript=no -drive id=disk0,file=/home/maxim.uvarov/build-test-update/build-rpb-mc/armhf-glibc/deploy/images/ledge-qemuarm/ledge-iot-ledge-qemuarm-20200218104425.rootfs.wic,if=none,format=raw -device virtio-blk-device,drive=disk0 -no-reboot -show-cursor -device virtio-rng-pci -monitor null -nographic -d unimp -semihosting-config enable,target=native -bios bl1.bin -dtb ledge-qemuarm.dtb -drive id=disk1,file=ledge-kernel-uefi-certs.ext4.img,if=none,format=raw -device virtio-blk-device,drive=disk1  -machine virt,secure=on -cpu cortex-a15 -m 1024  -device virtio-serial-device -chardev null,id=virtcon -device virtconsole,chardev=virtcon 

	NOTICE:  Booting Trusted Firmware
	NOTICE:  BL1: v2.2(debug):v2.2-78-g76f25eb52
	NOTICE:  BL1: Built : 08:42:37, Feb 10 2020
	INFO:    BL1: RAM 0xe04e000 - 0xe056000
	WARNING: BL1: cortex_a15: CPU workaround for 816470 was missing!
	INFO:    BL1: cortex_a15: CPU workaround for cve_2017_5715 was applied
	INFO:    BL1: Loading BL2
	WARNING: Firmware Image Package header check failed.
	INFO:    Loading image id=1 at address 0xe01b000
	INFO:    Image id=1 loaded: 0xe01b000 - 0xe0201c0
	NOTICE:  BL1: Booting BL2
	INFO:    Entry point address = 0xe01b000
	INFO:    SPSR = 0x1d3
	NOTICE:  BL2: v2.2(debug):v2.2-78-g76f25eb52
	NOTICE:  BL2: Built : 08:42:37, Feb 10 2020
	INFO:    BL2: Doing platform setup
	INFO:    BL2: Loading image id 4
	WARNING: Firmware Image Package header check failed.
	INFO:    Loading image id=4 at address 0xe100000
	INFO:    Image id=4 loaded: 0xe100000 - 0xe10001c
	INFO:    OPTEE ep=0xe100000
	INFO:    OPTEE header info:
	INFO:          magic=0x4554504f
	INFO:          version=0x2
	INFO:          arch=0x0
	INFO:          flags=0x0
	INFO:          nb_images=0x1
	INFO:    BL2: Loading image id 21
	WARNING: Firmware Image Package header check failed.
	INFO:    Loading image id=21 at address 0xe100000
	INFO:    Image id=21 loaded: 0xe100000 - 0xe12e1f8
	INFO:    BL2: Skip loading image id 22
	INFO:    BL2: Loading image id 5
	WARNING: Firmware Image Package header check failed.
	INFO:    Loading image id=5 at address 0x60000000
	INFO:    Image id=5 loaded: 0x60000000 - 0x600976bc
	NOTICE:  BL1: Booting BL32
	INFO:    Entry point address = 0xe100000
	INFO:    SPSR = 0x1d3


	U-Boot 2020.01 (Feb 10 2020 - 08:42:58 +0000)

	DRAM:  1 GiB
	WARNING: Caches not enabled
	Flash: 64 MiB
	In:    pl011@9000000
	Out:   pl011@9000000
	Err:   pl011@9000000
	Net:   No ethernet found.
	Hit any key to stop autoboot:  0 
	ERROR: reserving fdt memory region failed (addr=7fe00000 size=200000)
	1313 bytes read in 2 ms (640.6 KiB/s)
	Scanning disk virtio-blk#30...
	Scanning disk virtio-blk#31...
	** Unrecognized filesystem type **
	Found 4 disks

	Warning: virtio-net#32 using MAC address from ROM
	ERROR: reserving fdt memory region failed (addr=7fe00000 size=200000)
	2299 bytes read in 1 ms (2.2 MiB/s)
	ERROR: reserving fdt memory region failed (addr=7fe00000 size=200000)
	2299 bytes read in 1 ms (2.2 MiB/s)
	Booting: kernel
	EFI stub: Booting Linux Kernel...
	EFI stub: UEFI Secure Boot is enabled.
	EFI stub: Using DTB from configuration table
	EFI stub: Exiting boot services and installing virtual address map...
	[    0.000000] Booting Linux on physical CPU 0x0
	[    0.000000] Linux version 5.3.6 (oe-user@oe-host) (gcc version 8.2.1 20180802 (Linaro GCC 8.2-2018.08~dev)) #1 SMP Tue Feb 18 10:49:14 UTC 2020
	[    0.000000] CPU: ARMv7 Processor [412fc0f1] revision 1 (ARMv7), cr=30c5387d
	[    0.000000] CPU: div instructions available: patching division code
	[    0.000000] CPU: PIPT / VIPT nonaliasing data cache, PIPT instruction cache
	[    0.000000] OF: fdt: Machine model: linux,dummy-virt
	[    0.000000] OF: fdt: Ignoring memory block 0xe00000


armv7 (qemu_arm)
----------------

.. code-block:: bash

   qemu-system-arm  \
       -device virtio-net-pci,netdev=net0,mac=52:54:00:12:34:02 -netdev tap,id=net0,ifname=tap0,script=no,downscript=no \
       -drive id=disk0,file=${DISK},if=none,format=raw -device virtio-blk-device,drive=disk0 -no-reboot -show-cursor \
       -device virtio-rng-pci -monitor null -nographic \
       -d unimp -semihosting-config enable,target=native -bios bl1.bin -dtb ledge-qemuarm.dtb \
       -drive id=disk1,file=ledge-kernel-uefi-certs.ext4.img,if=none,format=raw -device virtio-blk-device,drive=disk1 \
       -machine virt,secure=on -cpu cortex-a15 -m 1024  -device virtio-serial-device \
       -chardev null,id=virtcon -device virtconsole,chardev=virtcon 

armv8 (qemu_arm64)
------------------

OVMF:


.. code-block:: bash

   qemu-system-aarch64 \
      -cpu cortex-a57 -machine virt -nographic -net nic,model=virtio,macaddr=DE:AD:BE:EF:36:03 -net tap -m 1024 -monitor none \
      -bios ${OVMF} -drive id=disk0,file=${DISK},if=none,format=raw -device virtio-blk-device,drive=disk0 -m 4096 -smp 4 -nographic

UBOOT:

.. code-block:: bash

   qemu-system-aarch64 \
      -device virtio-net-pci,netdev=net0,mac=52:54:00:12:34:02 -netdev tap,id=net0,ifname=tap0,script=no,downscript=no \
      -drive id=disk0,file=${ROOTFS},if=none,format=raw -device virtio-blk-device,drive=disk0 -show-cursor \
      -device virtio-rng-pci -monitor null -nographic \
      -d unimp -semihosting-config enable,target=native \
      -bios bl1.bin \
      -drive id=disk1,file=${KEYS},if=none,format=raw \
      -device virtio-blk-device,drive=disk1  -nographic -machine virt,secure=on -cpu cortex-a57 -m 4096 -serial mon:stdio -serial null \
      -no-reboot

x86_64
------

.. code-block:: bash

   qemu-system-x86_64 \ 
      -cpu host -enable-kvm -nographic -net nic,model=virtio,macaddr=DE:AD:BE:EF:36:03 -net tap -m 1024 -monitor none \
      -drive file=${DISK},id=hd,format=raw \
      -drive if=pflash,format=raw,file=${OVMF} \
      -m 4096 -serial mon:stdio -show-cursor -object rng-random,filename=/dev/urandom,id=rng0 -device virtio-rng-pci,rng=rng0

Pre built binaries
=================

Pre built binaries can be downloaded with the following link:
http://snapshots.linaro.org/components/ledge/oe/
(Linaro account is required).

CI run task can be found here: https://ci.linaro.org/job/ledge-oe/
