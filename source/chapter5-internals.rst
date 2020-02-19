.. SPDX-License-Identifier: CC-BY-SA-4.0

***************
LEDGE Internals
***************

This chapter discusses specific features for LEDGE RP.

Applications
============

ledge-iot image package set is aligment with Fedora IoT package set. List of the packages can be found in bitbake recepie ( https://github.com/Linaro/meta-ledge/blob/zeus/meta-ledge-sw/recipes-samples/packagegroups/packagegroup-ledge-iot.bb ).

ledge-gateway image includes minimal console image with Ostree and Docker updates support. ( https://github.com/Linaro/meta-ledge/blob/zeus/meta-ledge-sw/recipes-samples/images/ledge-gateway.bb )

U-Boot hardening
================

Security is very important on EDGE nodes. Hardening and protecting the bootloader is the first step towards a secure system. U-boot command line is disabled and kernel boot parameters are preset in LEDGE image.

QEMU with TF-A and OP-TEE
=========================


Ledge image support only qemu versions from 4.1 and above. To load TF-A and OP-TEE with qemu you need to place files at the run directory and name them as bl1.bin, bl2.bin and etc:

.. code-block:: bash

	│   ├── bl1.bin -> arm-trusted-firmware/bl1.bin
	│   ├── bl2.bin -> arm-trusted-firmware/bl2.bin
	│   ├── bl32.bin -> optee/tee-header_v2.bin
	│   ├── bl32_extra1.bin -> optee/tee-pager_v2.bin
	│   ├── bl32_extra2.bin -> optee/tee-pageable_v2.bin
	│   ├── bl33.bin -> u-boot-ledge-qemuarm.bin

Then -semihosting and -bios options are used to boot up qemu virtual machine:

.. code-block:: bash

	-d unimp -semihosting-config enable,target=native \
      	-bios bl1.bin

QEMU with firmware TPM (fTMP) in OP-TEE, TF-A and U-Boot
========================================================

LEDGE patches default dtb for qemu with ftpm entry.


.. code-block:: bash 

	tpm@0 {
		compatible = "microsoft,ftpm";
		linux,sml-base = <0x0 0xC0000000>;
		linux,sml-size = <0x10000>;
	};

Once qemu is run you might see that tmp_ftpm_tee module privides TEE supplicant storage.

.. code-block:: bash

	~ # rmmod tpm_ftpm_tee && modprobe tpm_ftpm_tee
	~ # tpm2_getrandom 256
	[  107.057613] audit: type=1130 audit(1574348463.038:33): pid=1 uid=0 auid=4294967295 ses=4294967295 msg='unit=tpm2-abrmd comm="systemd" exe="/lib/systemd/systemd" hostname=? addr=? terminal=? res=success'
	0xD0 0x31 0xA5 0xB9 0xF5 0xD1 0x5D 0x91 0x95 0x19 0x59 0x83 0x9F 0x7D 0x12 0x4F 0x8F 0xA2 0x8C 0xC2 0x10 0x71 0x09 0x84 0x6F 0x8B 0x1E 0xE6 0xD4 0xA9 0xA8 0xEB 0xB9 0xAB 0x39 0x92 0x66 0xCB 0x15 0x38 0x7C 0x3F 0x53 0x69 0x86 0xCC 0xA2 0x2A 0x33 0x6B 0x6D 0xFA 0x62 0xC3 0x70 0x93 0x9F 0x96 0xA8 0xFE 0xDA 0x4B 0x4F 0x15
