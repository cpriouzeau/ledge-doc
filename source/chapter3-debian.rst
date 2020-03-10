.. SPDX-License-Identifier: CC-BY-SA-4.0

******
Debian
******

This chapter describes specific Debian LEDGE build and run.

Supported platfroms
===================
 - armv7/armhf
 - armv8/arm64

Build
===========
The Debian build system are based on fai tools (http://fai-project.org)
and on a Linaro debian configuration.

Fai tools can be use to generate cross-architecture disk via several way:

1. by using qemu and fai setup
 Described on section 'Building cross-architecture disk images' of https://github.com/faiproject/fai/blob/master/doc/fai-guide.txt

2. by using docker image for specific platform
 Only the docker usage are described here.

Build  steps and image generation for armv8
============================================

Docker configuration for arm64
------------------------------
Create a Dockerfile for arm64 (Dockerfile.arm64) with following content:
::
    FROM arm64v8/debian:buster

    # Upgrade system and install fai tools
    RUN apt-get update && apt-get -y upgrade && apt-get -y install \
        fai-server fai-setup-storage \
        qemu-utils procps pigz kpartx \
        u-boot-tools dosfstools gdisk

    # Clean up APT when done.
    RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

    # Replace dash with bash
    RUN rm /bin/sh && ln -s bash /bin/sh

    # Make /home/build the working directory
    RUN mkdir -p /home/build
    WORKDIR /home/build

Script to generate debian content:
---------------------------------
Script content (build-fai-linaro-debian.sh):
::
    #!/bin/sh
    LOCALPATH=$(pwd)
    cd fai
    sudo fai-diskimage -v --cspace $(pwd) \
         --hostname linaro-ledge-debian \
         -S 4G \
         --class SAVECACHE,BUSTER,DEBIAN,LINARO,LEDGE,RAW \
         "$LOCALPATH"/work.raw
    # copy fai log
    sudo cp /var/log/fai/linaro-ledge-debian/last/fai.log ../fai.log

    # extract debian content from raw
    LOOPDEV=$(sudo losetup -f -P --show "$LOCALPATH"/work.raw)
    sudo mount $LOOPDEV /mnt/
    cd /mnt/
    sudo tar cJf "$LOCALPATH"/rootfs-linaro-buster-raw-unknown.ext4 .
    cd -
    sudo umount /mnt

Create docker image:
-------------------

.. code-block:: bash

    docker build --force-rm --file Dockerfile.arm64 --tag creation .

Execute docker image on interactive mode with priviledge:
--------------------------------------------------------

.. code-block:: bash

    docker run -it --privileged -v $PWD:/home/build creation

The current directory are shared with docker.

Create Debian image:
--------------------
On Docker environment and fai directory

.. code-block:: bash

    DOCKER> ./build-fai-linaro-debian.sh
    DOCKER> exit

Image generation:
-----------------

For qemu (arm64):

.. code-block:: bash

    mkdir -p script
    wget https://raw.githubusercontent.com/Linaro/meta-ledge/zeus/meta-ledge-bsp/recipes-devtools/generate-raw-image/raw-tools/create_raw_from_flashlayout.sh \
     -O script/create_raw_from_flashlayout.sh
    chmod +x script/create_raw_from_flashlayout.sh

    wget https://raw.githubusercontent.com/Linaro/meta-ledge/zeus/meta-ledge-bsp/recipes-devtools/generate-raw-image/files/aarch64/FlashLayout_sdcard_arm64_without_boot_firmware.fld -O  FlashLayout_sdcard_arm64_without_boot_firmware.fld

    ./script/create_raw_from_flashlayout.sh FlashLayout_sdcard_arm64_without_boot_firmware.fld
    pigz -9 FlashLayout_sdcard_arm64_without_boot_firmware.raw


Build  steps and image generation for armv7
============================================

Docker configuration for armhf
------------------------------
Create a Dockerfile for armhf (Dockerfile.armhf) with following content:
::
    FROM arm32v7/debian:buster

    # Upgrade system and Yocto Proyect basic dependencie
    RUN apt-get update && apt-get -y upgrade && apt-get -y install \
        fai-server fai-setup-storage \
        qemu-utils procps pigz kpartx \
        u-boot-tools dosfstools gdisk

    # Clean up APT when done.
    RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

    # Replace dash with bash
    RUN rm /bin/sh && ln -s bash /bin/sh

    # Make /home/build the working directory
    RUN mkdir -p /home/build

    # Download fai config
    RUN cd /home/build && git clone https://git.linaro.org/ci/fai.git && cd fai && git checkout -b WORKING origin/master && cd /home/build

    WORKDIR /home/build

Script to generate debian content:
---------------------------------
Script content (build-fai-linaro-debian.sh):
::
    #!/bin/sh
    LOCALPATH=$(pwd)
    cd fai
    sudo fai-diskimage -v --cspace $(pwd) \
         --hostname linaro-ledge-debian \
         -S 4G \
         --class SAVECACHE,BUSTER,DEBIAN,LINARO,LEDGE,RAW \
         "$LOCALPATH"/work.raw
    # copy fai log
    sudo cp /var/log/fai/linaro-ledge-debian/last/fai.log ../fai.log

    # extract debian content from raw
    LOOPDEV=$(sudo losetup -f -P --show "$LOCALPATH"/work.raw)
    sudo mount $LOOPDEV /mnt/
    cd /mnt/
    sudo tar cJf "$LOCALPATH"/rootfs-linaro-buster-raw-unknown.ext4 .
    cd -
    sudo umount /mnt


Create docker image:
-------------------

.. code-block:: bash

    docker build --force-rm --file Dockerfile.armhf --tag creation .

Execute docker image on interactive mode with priviledge:
--------------------------------------------------------

.. code-block:: bash

    docker run -it --privileged -v $PWD:/home/build creation

The current directory are shared with docker.

Create Debian image:
--------------------
On Docker environment and fai directory

.. code-block:: bash

    DOCKER> ./build-fai-linaro-debian.sh
    DOCKER> exit

Image generation per board:
---------------------------
For qemu (armhf):

.. code-block:: bash

    mkdir -p script
    wget https://raw.githubusercontent.com/Linaro/meta-ledge/zeus/meta-ledge-bsp/recipes-devtools/generate-raw-image/raw-tools/create_raw_from_flashlayout.sh \
     -O script/create_raw_from_flashlayout.sh
    chmod +x script/create_raw_from_flashlayout.sh

    wget https://raw.githubusercontent.com/Linaro/meta-ledge/zeus/meta-ledge-bsp/recipes-devtools/generate-raw-image/files/armv7a/FlashLayout_sdcard_armhf_without_boot_firmware.fld -O FlashLayout_sdcard_armhf_without_boot_firmware.fld

    ./script/create_raw_from_flashlayout.sh FlashLayout_sdcard_armhf_without_boot_firmware.fld
    pigz -9 FlashLayout_sdcard_armhf_without_boot_firmware.raw

For stm32mp157c-dk2 (armhf):

.. code-block:: bash

    mkdir -p script
    wget https://raw.githubusercontent.com/Linaro/meta-ledge/zeus/meta-ledge-bsp/recipes-devtools/generate-raw-image/raw-tools/create_raw_from_flashlayout.sh \
     -O script/create_raw_from_flashlayout.sh
    chmod +x script/create_raw_from_flashlayout.sh

    wget https://raw.githubusercontent.com/Linaro/meta-ledge/zeus/meta-ledge-bsp/recipes-devtools/generate-raw-image/files/ledge-stm32mp157c-dk2/FlashLayout_sdcard_ledge-stm32mp157c-dk2-debian.tsv.template -O FlashLayout_sdcard_ledge-stm32mp157c-dk2-debian.tsv.template

    ./script/create_raw_from_flashlayout.sh FlashLayout_sdcard_ledge-stm32mp157c-dk2-debian.tsv.template
    pigz -9 FlashLayout_sdcard_ledge-stm32mp157c-dk2-debian.raw)



