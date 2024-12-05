---
title: "Integrating an LTE Cell Modem with Zymbit Secure Edge Node"
seoTitle: "Integrate LTE Modem with Zymbit Node"
seoDescription: "Learn how to integrate an LTE Cell Modem with a Zymbit Secure Edge Node for secure data transmission in this comprehensive guide"
datePublished: Mon Nov 18 2024 15:42:25 GMT+0000 (Coordinated Universal Time)
cuid: cm3n72chl000d09mdgbx941l0
slug: integrating-an-lte-cell-modem-with-zymbit-secure-edge-node
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1732204503239/ae893726-46da-47aa-a5ff-9f27f92e0aa4.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1731944514092/b44cfe12-e85c-4cb1-a50f-edc58435e216.png
tags: raspberry-pi, secure, edgecomputing, securityawareness

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732203566366/acbeba1f-cecf-4808-bfdc-e185d551e4b2.png align="center")

This How To Guide will show you how to integrate an LTE Cell Modem card with a [Zymbit Secure Edge Node Developer Kit](https://store.zymbit.com/products/sen-400-proto-kit/?utm_source=hashnode&utm_medium=web&utm_term=bootware). The Goals is to be able to get your Zymbit Secure Edge Node Developer Kit to come up with an alternative data path provided by the LTE Cell Modem.

The Zymbit SEN Developer Kit contains a Secure Compute Module (SCM) running Linux. The SCM is a hardened Linux computer that provides a secure element for the Raspberry Pi. The LoRaWAN card is a low-power, long-range wireless communication card that allows you to connect your Raspberry Pi to a LoRaWAN network.

## Overview

Here is a tl;dr of the steps we will take to create a Secure Edge Node with an LTE Cell modem as a data path:

1. Assemble the LTE Modem Pi HAT and connect it to the Secure Edge Node
    
2. Install the SX1302 Hardware Abstraction Layer (HAL) software on the SEN
    
3. Configure the SixFab software to connect the modem to the network
    
4. Install & Configure [Bootware](https://zymbit.com/bootware/?utm_source=hashnode&utm_medium=web&utm_term=bootware)™ on the SEN
    

## Parts list

Here are all the parts you’ll need in order to complete this tutorial:

1. A [Zymbit Edge Node 400 - Developer Kit](https://store.zymbit.com/products/sen-400-proto-kit/?utm_source=hashnode&utm_medium=web&utm_term=bootware)
    
2. An [LTE Cell Modem](https://sixfab.com/product/raspberry-pi-4g-lte-modem-kit/). Make sure you get one for your Geography as a North American version uses an entirely different frequency than the European version.  This integration is for  this specific LTE Modem from SixFab.
    

## Software

You’ll need to install the following software on your SCM:

1. [sx1302\_hal](https://github.com/Lora-net/sx1302_hal)
    

## Hardware setup

If you’ve ordered the parts from the list above, here’s what you you’ll get.

The [Zymbit Secure Edge Node Developer kit](https://store.zymbit.com/products/sen-400-proto-kit/?utm_source=hashnode&utm_medium=web&utm_term=bootware)

The Zymbit DevKit comes in 3 parts:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731941977989/28f45d39-b618-41de-bd22-0354ef1f5491.jpeg align="center")

1. The parts bag contains all the adapters you will need for your country’s power outlet.
    
2. The smaller white box is the power adapter
    
3. The larger brown box is the Secure Edge Node Developer Kit and associated parts.
    

Once you unbox (or unbag) all those, your DevKit should look something like this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942169530/4aefa6be-58c6-4b3f-bc5d-d795a28c1b2b.jpeg align="center")

Note that the Developer Kit comes with a couple of Jumper Wires (which you won’t need for this), a small battery, and a set of header pins.

## LTE Modem

The LTE Modem Kit comes in a small box with the card itself, the Pi HAT adapter, a set of antennas, and a SIM card with $10US of free airtime credits. Here’s the unboxing, though yours may be in a slightly different order, it should still have all the parts shown here.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942220260/eb4b25c9-14a6-4e4f-8988-f590200d776c.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942229309/69c17080-8ef9-4317-9bb4-be4e6f945c31.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942278019/13013a0d-f0d4-48b3-9c48-3dc86da194d5.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942286939/8e32c9c8-edca-4433-9ff4-92160ae324b7.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942294048/d84538bc-4994-4846-b651-086983b8177e.jpeg align="center")

## Assembly

The first thing to do is to attach the standoffs for the Pi HAT to the board. You will need to do this *before* you attach the SCM to the board, as the holes are not accessible once the SCM is attached.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942323488/40ff51f2-beed-45eb-94cd-f6a26009493d.jpeg align="center")

> **Note:** Though the picture shows using the standoffs that came with the kit, I ended up using 4 nylon M3 screws and 8 M3 nylon nuts instead to greatly reduce the height required for the Pi HAT. I placed the M3 screws through the holes, added an M3 nut, put the Pi HAT over the nut, and secured it with a final nut.

Now you can turn the board over and install the Secure Compute Module (SCM) onto the board.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942343299/2b9cb1ec-5059-47cc-ad6e-0a5ed3e6d491.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942553220/ade4565d-f193-42fd-a613-865cf7140f2d.jpeg align="center")

Make sure that the SCM is securely seated before proceeding.

Once you have connected the SCM to the board (you should hear a couple of clicks as the connectors seat fully), you can secure the SCM to the motherboard with the 4 provided nylon screws.

Now it’s time to turn the board over again and install the Pi HAT on the bottom.

First, take the set of header pins and insert them into the Board. This will allow you to seat the Pi HAT onto the pins

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942589987/4d280e0e-bb84-42bf-ab3d-1393210bfef8.jpeg align="center")

Once the pins are completely seated, you can attach the Pi HAT to the board. Make sure that all the pins line up and that the board is completely seated.

Once the pins are all seated and the board is secure, use the provided screws to secure the Pi HAT to the board. (See note above)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942619314/a77b37ac-c3e1-4941-8abb-306dfa90fc01.jpeg align="center")

Before attaching the LTE modem card to the Pi HAT it’s important to get the various antennas properly attached. The LTE modem uses 3 separate antennas (2 are combined), so it’s important to attach them to the right connectors.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942643918/30bf6063-782b-4efd-8f23-ab01526198a9.jpeg align="center")

Make **sure** that the antennas are attached as shown below:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942667590/ccbb7a1f-4bc2-4808-9a61-8527f3028e6c.jpeg align="center")

Once the antennas are connected correctly, you can slide the LTE Modem card into the PCIe slot on the Pi HAT. There is a metal retainer at the back that should snap into place over the card to hold it firmly in place.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942700798/080fc45e-e217-4f94-a3fd-35cd399f9be4.jpeg align="center")

Finally, you will need to insert the SIM Card into the SIM Card Slot on the Pi HAT. The SIM Card comes in a credit-card sized card, which can be broken out to various sized for the SIM itself. Break the largest version of the SIM Card out, and then break the Micro-SIM out of that insert it into the SIM Slot on the Pi HAT.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942724772/429e933a-1ac8-4099-b895-486d84dc03bf.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942738269/e77b3bf0-fe52-4263-b24f-41cbbb78dd9e.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942752893/122f3ea2-460c-4279-9d47-b40704a5df3a.jpeg align="center")

The hardware is now assembled and it’s time to move on configuring the LTE Modem card.

## LTE Modem setup

**Note:** I will not be going into the details of setting up the SCM itself. As long as you have attached it correctly, and attached an ethernet cable to the board, you should be able to login to the SCM with:

ssh zymbit@zymbit-dev.local

Use the password zymbit to login, but you should immediately change it to something secure using the passwdcommand.

passwd

You will be prompted to enter the current password, and then the new password twice.

The complete SixFab documentation on setting up your LTE Modem can be found [here](https://docs.sixfab.com/docs/raspberry-pi-4g-lte-cellular-modem-kit-getting-started), but I will summarize them for you.

Most of this setup will be done via the [SixFab Connect](https://connect.sixfab.com/) interface. You will need to create an account there in order to register your device (and claim your free airtime).

From the dashboard, add your free airtime credits  by going to Billing -&gt; Add Credit -&gt; Coupon Code and enter your code.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942780118/11a87f85-ebb4-45cf-8660-c1c7ff83d114.jpeg align="center")

To initialize the Sixfab CORE device, an active Sixfab SIM asset is required.  
If you don’t have one, please register and activate a Asset from the **"Assets -&gt; Register Asset"** section.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731942987397/6d58af43-dae0-43bd-bada-e1e822a569e8.jpeg align="left")

Navigate to the details of the asset you want to install the CORE device for on the [Assets page.](https://connect.sixfab.com/#/assets)

Click on the **'Initialize CORE Device'** button located in the Sixfab CORE section within the [asset](https://connect.sixfab.com/#/assets) details.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731943041224/68e00e5c-39cd-4bfd-a249-39123cb28d22.png align="left")

Select the hardware board you want to install Sixfab CORE on and proceed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731943052520/8119bc17-a196-4c23-9796-2738cfd73bcf.jpeg align="left")

Here, choose according to your region.

| **Region** | **Description** |
| --- | --- |
| Global | For devices to be deployed Worldwide |
| EMEA | For devices to be deployed in Europe, the Middle East and Africa |
| APAC | For devices to be deployed in Asia-Pacific only |

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731943165548/fe738e87-6b24-49ba-be36-34a37c6139ed.jpeg align="center")

Copy the initialization code

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731943222750/5df2764a-cc64-4a9f-abca-e802ea7a8529.jpeg align="center")

Now run the installation code you have just copied above. Just paste it into the terminal and hit ‘return’ or ‘enter’.

```bash
sudo bash -c "$(curl -sN https://install.connect.sixfab.com)" -- -t YOUR_TOKEN_APPEARS_HERE'
    .&@&.             %%          .%@%
   #@@@%           *&@@@&.         %@@@#
  &@@&. .&@@%   .%@@@@@%/.   .%@@&. ,&@@%
 %@@&. /@@@#  *&@@@@&&&@@@@&,  %@@&* .&@@&
/@@@* .@@@#  &@@&(       (@@@%  #@@&, *@@@*
%@@&. #@@&. (@@@,         ,&@@( .@@@(  &@@#
%@@&. #@@&. /@@@,         *&@@( .@@@(  &@@#
*@@@* .&@@#  %@@@#       %@@@#  #@@&, /@@@,
 %@@&, *@@@%  .&@@@@@@@@@@@%. .&@@&, ,&@@%
  %@@&*  %@@#     *#%%%#*     #@@%  *&@@%
   (@@@&.                         .&@@&/
     #@#                           #@#

  ____  _       __       _        ____
 / ___|(_)_  __/ _| __ _| |__    / ___|___  _ __ ___
 \___ \| \ \/ / |_ / _` | '_ \  | |   / _ \| '__/ _ \
  ___) | |>  <|  _| (_| | |_) | | |__| (_) | | |  __/
 |____/|_/_/\_\_|  \__,_|_.__/   \____\___/|_|  \___|
=====================================================
[INFO] Creating sixfab user...
[INFO] Updating sudoers...
[INFO] Sudoers updated
[INFO] Updating system package index...
[INFO] Looking for dependencies...
[INFO] Git is not installed, installing...
[INFO] Pip for python3 is not installed, installing...
[INFO] ifmetric is not installed, installing...
[INFO] Installing Sixfab ATCom tool...
[INFO] Sixfab ATCom installed
[INFO] Initializing environment file...
[INFO] Initialized environment file
[INFO] Downloading agent source...
[INFO] Installing agent dependencies...
[INFO] Installed agent dependencies.
[INFO] Initializing agent service...
[INFO] Agent service initialized successfully.
[INFO] Downloading manager source...
[INFO] Installing manager dependencies...
[INFO] Installed manager dependencies.
[INFO] Initializing manager service...
[INFO] Manager service initialized successfully.
[INFO] Setting default network priority : eth0 > wlan0 > usb0 = wwan0
[DONE]
Installation completed successfully.
-----------------------------------------------------------------------
Press ENTER to reboot your system. (Recommended)
Press Ctrl+C (^C) to finish installation without reboot.

Reminder: Plug the USB cable to Sixfab HAT!
Warning: Network priority settings will be effective after reboot! 
-----------------------------------------------------------------------
```

Now you can plug the short USB cable into the Pi HAT, and then into the DevKit USB Port.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731944051249/666b7cba-7c4f-4a78-b36a-ae11ecd07619.jpeg align="center")

After the installation is successfully completed, press 'Enter' to restart the Raspberry Pi, and then plug the USB cable to the HAT.

After the installation is complete, it may take up to 5 minutes for the services to configure your modem, set up your cellular internet connection settings, etc. If everything goes smoothly, you will see the data on your CORE Dashboard page, as shown below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731944116652/8521e412-1ff0-4682-a31c-a13a85c0d9b2.png align="left")

## Install & Configure Bootware

### Background on Bootware

Bootware is software from Zymbit that helps you configure and manage your Zymbit SEN and Raspberry Pi assets seamlessly and securely. It allows you to have A/B partitioning for reliable, recoverable updates over the air, and is simple to configure and use.

* A/B updates managed in secure silicon.
    
* Encrypted filesystem and user kernel.
    
* Signed images and updates.
    
* Fallback and recovery options.
    
* Seamless integration with Raspberry Pi OS and Ubuntu.
    
* Easy upgrade path from standard RPi products.
    
* Available on most Zymbit products.
    

### Installing Bootware

```bash
curl -sSf https://raw.githubusercontent.com/zymbit-applications/zb-bin/main/install.sh | sudo bash
```

This will install the `zbcli` Zymbit CLI. It will also ask you some questions. Since

```bash
zb-install.sh: bootstrapping the zbcli installer
	---------
	Pi Module:         Raspberry Pi 4/Compute Module 4
	Operating System:  Rpi-Bullseye
	Zymbit module:     Secure Compute Module
	Kernel:            kernel8.img
	---------

✔ 'zbcli' comes with software signing by default. Include hardware key signing? (Requires SCM or HSM6) · Yes
✔ Select version · zbcli-1.2.0-29
Installing zbcli
Installed zbcli. Run 'zbcli install' to install Bootware onto your system or 'zbcli --help' for more options.
zb-install.sh: cleaning up
```

Since we are installing this on an SEN, which uses a Secure Compute Module (SCM) we can use hardware signing to sign our Bootware images.

We can now use the CLI to install Bootware:

```bash
sudo zbcli install
```

This will trigger an install, and a reboot

```bash
zymbit@zymbit-dev:~ $ sudo zbcli install
	---------
	Pi Module:         Raspberry Pi 4
	Operating System:  Rpi-Bullseye
	Zymbit module:     Secure Compute Module
	Kernel:            kernel8.img
	---------
     Deleted '/boot/uboot.env'
   Installed 'u-boot-tools'
     Deleted '/usr/bin/zb_get_root_dev.sh'
       Found OpenSSL 1
✔ Found /boot/zb_config.enc. Overwrite this file? · yes
     Created '/boot/zb_config.enc'
    Modified zbconfig 'kernel_filename'
   Installed zboot
    Modified '/etc/rc.local'
    Modified '/boot/config.txt'
    Modified /etc/update-motd.d/01-zymbit-fallback-message
? A reboot into zboot is required. Reboot now? (y/n) › yes
```

Once the machine reboots, we will create our first `zboot` image from the running system:

In order to complete this step, you will need a USB Drive on which to create the image. Once you have plugged that in, make sure it is erased:

```bash
sudo fdisk -W always /dev/sda
```

That will ask you to create a partition table:

```bash
Welcome to fdisk (util-linux 2.38.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS (MBR) disklabel with disk identifier 0x27b0681a.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1):
First sector (2048-125313282, default 2048):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-125313282, default 125313282):

Created a new partition 1 of type 'Linux' and of size 59.8 GiB.
Partition #1 contains a ext4 signature.

The signature will be removed by a write command.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```

The important thing to notice there is that you will enter `n` to create a new partition map, then `p` to make it a Primary partition. After that you can simply accept the defaults and then enter `w` to write the partition map to the disk.

Once that’s done, you’ll have to create a filesystem on the disk

```bash
sudo mkfs.ext4 -j /dev/sda1 -F
```

Which will create am `ext4` filesystem on the disk

```bash
mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 15663904 4k blocks and 3916304 inodes
Filesystem UUID: 4a3af5d0-bac4-4903-965f-aa6caa8532cf
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
	4096000, 7962624, 11239424

Allocating group tables: done
Writing inode tables: done
Creating journal (65536 blocks): done
Writing superblocks and filesystem accounting information: done
```

Now you’re ready to create your boot image!

You’ll need to mount the new disk:

```bash
sudo mount /dev/sda1 /mnt
```

And run the imager

```bash
sudo zbcli imager
```

This will again ask you some questions:

```bash
   Validated bootware installation
	---------
	Pi Module:         Raspberry Pi 4
	Operating System:  Rpi-Bullseye
	Zymbit module:     Secure Compute Module
	Kernel:            kernel8.img
	---------
     Created '/etc/zymbit/zboot/update_artifacts/tmp'
? Enter output directory › /mnt
✔ Enter output directory · /tmp
✔ Enter image name · z-image-1
✔ Select image type · Full image of live system
✔ (Optional) enter image version · 1.0
✔ Select signing method. Note: software/hardware keys are not interchangeable. Stick with one method. · Hardware-based keys
✔ Select key · Create new hardware keys
```

Notice that I used the mount point for the USB Drive as our output directory. I then chose a name and version number for the image and chose to use a software key, since I’m using a Zymkey.

Don’t be surprised if this step takes a while. What it’s doing is making a complete copy of the files on the running disk, and signing it with the hardware key that it has generated.

```bash
     Created signing key
     Created '/etc/zymbit/zboot/update_artifacts/file_manifest'
     Created '/etc/zymbit/zboot/update_artifacts/file_deletions'
    Verified path unmounted '/etc/zymbit/zboot/mnt'
     Cleaned '/etc/zymbit/zboot/mnt'
     Deleted '/etc/crypttab'
    Verified disk size (required: 2.33 GiB, free: 26.39 GiB)
     Created initramfs
     Created snapshot of boot (/etc/zymbit/zboot/update_artifacts/tmp/.tmpBgEBJk/z-image-1_boot.tar)
     Created snapshot of root (/etc/zymbit/zboot/update_artifacts/tmp/.tmpBgEBJk/z-image-1_rfs.tar)
     Created '/mnt/tmp'
     Cleaned '/mnt/tmp'
     Created staging directory (/mnt/tmp/.tmpEhjNN7)
     Created '/mnt/tmp/.tmpEhjNN7/header.txt'
     Created tarball (/mnt/tmp/.tmpEhjNN7/update_artifact.tar)
     Created header signature
     Created update artifact signature
     Created file manifest signature
     Created file deletions signature
     Created '/mnt/tmp/.tmpEhjNN7/signatures'
     Created signatures (/mnt/tmp/.tmpEhjNN7/signatures)
      Copied file (/etc/zymbit/zboot/update_artifacts/file_manifest) to (/mnt/tmp/.tmpEhjNN7/file_manifest)
      Copied file (/etc/zymbit/zboot/update_artifacts/file_deletions) to (/mnt/tmp/.tmpEhjNN7/file_deletions)
     Created tarball (/mnt/z-image-1.zi)
     Created '/mnt/z-image-1_private_key.pem'
       Saved private key '/mnt/z-image-1_private_key.pem'
     Created '/mnt/z-image-1_pub_key.pem'
       Saved public key '/mnt/z-image-1_pub_key.pem'
     Cleaned '/mnt/tmp'
       Saved image '/mnt/z-image-1.zi' (2.33 GiB)
    Finished in 384.8s
```

In order to enable resilient updates, we will be creating A/B partitions on the disk.

Bootware encrypts the A, B, and DATA partitions. The A and B partition are locked with unique LUKS keys, meaning you cannot access the Backup partition from the Active partition. The encrypted DATA partition is accessible from either the A or B partition.

Setting up this A/B partitioning scheme is usually quite cumbersome and difficult to implement. Zymbit’s Bootware has taken that process and simplified it such that it’s a relatively easy process. So let’s go through that process now and make your Pi both secure and resilient.

### Create A/B partitions

Since we’ve not previously had a backup B partition, we will create one, and we will place the current image (which we know is good, since we’re currently running it) into that partition. To do that, we will update the configuration (really create it) with the `zbcli` tool.

```bash
sudo zbcli update-config
```

Will create the config that we need.

```bash
   Validated bootware installation
	---------
	Pi Module:         Raspberry Pi 4
	Operating System:  Rpi-Bullseye
	Zymbit module:     Secure Compute Module
	Kernel:            kernel8.img
	---------
        Info the root file system will be re-partitioned with your chosen configuration.
```

This process will ask you some questions to determine how to lay out your partitions. The first is what device partition layout you would like to use. Choose the recommended option:

```bash
? Select update policy ›
❯   [RECOMMENDED] BACKUP: Applies new updates to current backup filesystem and swap to booting the new updated backup partition as the active partition now. If the new update is bad, it will rollback into the pre
     Running [========================================] 2/1 (00:00:17):                                                                                                                                             WARNING! Detected active partition (28.71GB) is larger than 14.86GB needed for two filesystems.
 Active partition won't be saved!!!
 Changing update mode to UPDATE_BOTH!!!
       Using update mode (UPDATE_BOTH)
        Data partition size currently set to: 512 MB
        Info bootware will create a shared data partition after A/B in size MB specified
```

Next, you can select the size of the data partition. It defaults to 512MB, but I suggest increasing that to 1024MB.

```bash
✔ Enter size of data partition in MB · 1024
       Using Data Partition Size 1024MB
  Defaulting to configured endpoint '/dev/sda1'
        Info update endpoints can be either an HTTPS URL or an external mass storage device like a USB stick.
       Found update name 'z-image-1'
       Saved update name 'z-image-1'
       Using update endpoint '/dev/sda1'
Configuration settings saved
    Finished in 42.1s
```

We’ve now got a system that is configured to have A/B partitioning, and to apply updates to the backup partition when they are available.  
To complete the process, we will actually apply the update (which is really just a copy of the currently running system). This will trigger the re-partitioning and a reboot.

```bash
sudo zbcli update
```

```bash
   Validated bootware installation
	---------
	Pi Module:         Raspberry Pi 4
	Operating System:  Rpi-Bullseye
	Zymbit module:     Secure Compute Module
	Kernel:            kernel8.img
	---------
     Cleaned '/etc/zymbit/zboot/update_artifacts/tmp'
       Found update configs
? Proceed with current configs? These can be modified through 'zbcli update-config'
	---------
 	Update endpoint   /dev/sda1
 	Update name       z-image-1
 	Endpoint type     LOCAL
 	Partition layout  A/B
 	Update policy     UPDATE_BOTH
 	---------
     Created temporary directory (/etc/zymbit/zboot/update_artifacts/tmp/.tmpCfhm6c)
     Mounted '/dev/sda1' to '/etc/zymbit/zboot/update_artifacts/tmp/.tmpyKYgR3'
       Found image tarball (/etc/zymbit/zboot/update_artifacts/tmp/.tmpyKYgR3/z-image-1.zi)
    Unpacked '/etc/zymbit/zboot/update_artifacts/tmp/.tmpCfhm6c/update_artifact.tar'
    Unpacked '/etc/zymbit/zboot/update_artifacts/tmp/.tmpCfhm6c/signatures'
    Unpacked '/etc/zymbit/zboot/update_artifacts/tmp/.tmpCfhm6c/header.txt'
    Unpacked '/etc/zymbit/zboot/update_artifacts/tmp/.tmpCfhm6c/file_manifest'
    Unpacked '/etc/zymbit/zboot/update_artifacts/tmp/.tmpCfhm6c/file_deletions'
     Decoded header signature
     Decoded image signature
     Decoded manifest signature
     Decoded deletions signature
       Found header data
       Found image data
       Found manifest data
       Found file deletions data
    Verified header signature
    Verified image signature
    Verified manifest signature
    Verified file deletions signature
    Modified zbconfig 'public_key'
    Modified zbconfig 'new_update_needed'
    Modified zbconfig 'root_a'
    Modified zbconfig 'root_b'
    Modified zbconfig 'root_dev'
      Copied file (/boot/firmware/usr-kernel.enc) to (/boot/firmware/zboot_bkup/usr-kernel-A.enc)
      Copied file (/boot/firmware/kernel8.img) to (/boot/firmware/zboot_bkup/kernel8.img)
    Modified zbconfig 'update_with_new_image'
    Modified zbconfig 'kernel_filename'
? Scheduled update for the next reboot. Reboot now? (y/n) › yes
```

When it asks to reboot, say yes, and then wait.

Once your Pi is rebooted, log in and check to see that it’s correct.

```bash
lsblk
NAME              MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINTS
sda                 8:0    1 59.8G  0 disk
└─sda1              8:1    1 59.8G  0 part
mmcblk0           179:0    0 29.7G  0 disk
├─mmcblk0p1       179:1    0  512M  0 part  /boot/firmware
├─mmcblk0p2       179:2    0 14.1G  0 part
│ └─cryptrfs_A    254:0    0 14.1G  0 crypt /
├─mmcblk0p3       179:3    0 14.1G  0 part
└─mmcblk0p4       179:4    0    1G  0 part
  └─cryptrfs_DATA 254:1    0 1008M  0 crypt
```

Notice that we now have two `cryptfs` devices. These are fully signed and encrypted filesystems.

What if the update had failed? Here’s the beauty of A/B partitioning with Bootware: if the system fails to boot (it fails to reach a `systemd init` target for 3 times in a row), Bootware will revert to the known-good partition, bringing your device back on-line.

### Verifying the changes

We’ve created A/B partitions, and have verified that they are both there, and that we are booted out of the active (A) partition. As part of the Bootware initialization we just performed, the same image was also installed on the B partition. We can verify this by running

```bash
sudo zbcli rollback-swap
```

This will trigger a reboot of your Pi, but when you log back in, run

```bash
lsblk
```

Again and see what filesystems you have available now.

Congratulations! You now have a fully secure IoT edge node that can take secure updates, and communicate via an LTE network!

## Conclusion

At this point you should have a Zymbit Secure Edge Node Developer Kit node that can successfully connect to the SixFab LTE Cellular network and send data.

You may be asking yourself “what next” and I’m glad you asked! You can now use your SEN to protect local data, sign data for transmission for verification purposes, and be confident that your SEN is capable of sending (and verifying) data via the LTE modem either as a primary communications channel or as a back-up channel in case the onboard LAN or WiFi connections can’t be used.

If you have further questions, or want to discuss this or any other Raspberry Pi security matter, please join our [Zymbit Community](https://community.zymbit.com/t/using-an-lte-modem-on-an-sen/1715).