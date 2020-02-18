.. SPDX-License-Identifier: CC-BY-SA-4.0

************
OpenEmbedded
************

This chapter discusses specific OpenEmbedded LEDGE build and run.

Supported platfroms
===================
- armv7/ledge-multi-armv7 (qemu, ti-am572x, stm32mp157c-dk2);
- armv8/ledge-multi-armv7 (qemu, synquacer)
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

armv7 (qemu_arm)
----------------

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

Prebuild binaries
=================

Prebuild binaries can be downloaded with the following link:
http://snapshots.linaro.org/components/ledge/oe/
(Linaro account is required).

CI run task can be found here: https://ci.linaro.org/job/ledge-oe/
