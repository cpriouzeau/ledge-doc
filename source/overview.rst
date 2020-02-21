.. SPDX-License-Identifier: CC-BY-SA-4.0

**************
LEDGE Overview
**************

General
=======

LEDGE images are related to IoT and EDGE devices. It has advanced security features supported:

 - Secure UEFI boot
 - OP-TEE (Open Portable Trusted Execution Environment)
 - ARM trusted Firmware (AT-F)
 - Teanocore edk2 firmware or Uboot with UEFI mode support
 - fTMP (Firmware TPM driver with backend to OP-TEE)
 - Kernel image sign with certificate
 - Kernel modules sign
 - IMA/EVM for integrity user applications
 - Selinux
 - Containered isolation (docker)
 - Advanced system update

The LEDGE image consist of WIC image and firmware to boot this image on specific board or virtual machine.

WIC image
=========

LEDGE WIC image for consists of 2 partiotions - ESP partition and linux rootfs partition. Disk image may have
gpt lable type or may not, depends on varios hardware requirements.

.. code-block:: bash

	Units: sectors of 1 * 512 = 512 bytes
	Sector size (logical/physical): 512 bytes / 512 bytes
	I/O size (minimum/optimal): 512 bytes / 512 bytes
	Disklabel type: gpt
	Disk identifier: 5B707C60-D281-441C-A31C-73849DD89C49

	Device        Start     End Sectors   Size Type
	/dev/loop6p1     40  123319  123280  60.2M Microsoft basic data
	/dev/loop6p2 123320 1861731 1738412 848.9M Linux filesystem

ESP partition
-------------

ESP partition is about 60 Megabytes vfat partiontion wit the following structure:

.. code-block:: bash

	├── dtb
	├── EFI
	│   └── BOOT
	│       └── bootarm.efi
	└── ledge-initramfs.rootfs.cpio.gz

Where:

  - dtb - directory which contains all dtbs for all supported devices.

  - bootarm.efi - linux kernel compiled as UEFI stab and which boots directly from firmware. (bootx64.efi for x86, bootaarch.efi)

  - ledge-initramfs.rootfs.cpio.gz - initramfs is used to do initial initialization and find and mount rootfs.
