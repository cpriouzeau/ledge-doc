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
