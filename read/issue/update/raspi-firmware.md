---
title: raspi-firmware
nav_order: 9021
has_children: false
parent: 更新
grand_parent: 議題
---


# raspi-firmware

## Issue

when update linux-image-amd64

run

```sh
sudo apt-get update && sudo apt-get dist-upgrade
```

show

```

Setting up linux-image-6.1.0-12-amd64 (6.1.52-1) ...
/etc/kernel/postinst.d/dkms:
dkms: running auto installation service for kernel 6.1.0-12-amd64.
dkms: autoinstall for kernel: 6.1.0-12-amd64.
/etc/kernel/postinst.d/initramfs-tools:
update-initramfs: Generating /boot/initrd.img-6.1.0-12-amd64
raspi-firmware: missing /boot/firmware, did you forget to mount it?
run-parts: /etc/initramfs/post-update.d//z50-raspi-firmware exited with return code 1
run-parts: /etc/kernel/postinst.d/initramfs-tools exited with return code 1
dpkg: error processing package linux-image-6.1.0-12-amd64 (--configure):
 installed linux-image-6.1.0-12-amd64 package post-installation script subprocess returned error exit status 1
dpkg: dependency problems prevent configuration of linux-image-amd64:
 linux-image-amd64 depends on linux-image-6.1.0-12-amd64 (= 6.1.52-1); however:
  Package linux-image-6.1.0-12-amd64 is not configured yet.

dpkg: error processing package linux-image-amd64 (--configure):
 dependency problems - leaving unconfigured
Errors were encountered while processing:
 linux-image-6.1.0-12-amd64
 linux-image-amd64
E: Sub-process /usr/bin/dpkg returned an error code (1)
W: Operation was interrupted before it could finish

```




## 探索

執行

``` sh
dpkg -L raspi-firmware
```

顯示

```
/.
/etc
/etc/default
/etc/default/raspi-firmware
/etc/initramfs
/etc/initramfs/post-update.d
/etc/initramfs/post-update.d/z50-raspi-firmware
/etc/kernel
/etc/kernel/postinst.d
/etc/kernel/postinst.d/z50-raspi-firmware
/etc/kernel/postrm.d
/etc/kernel/postrm.d/z50-raspi-firmware
/lib
/lib/firmware
/lib/firmware/brcm
/lib/firmware/brcm/brcmfmac43430-sdio.txt
/lib/firmware/brcm/brcmfmac43455-sdio.txt
/lib/firmware/brcm/brcmfmac43456-sdio.bin
/lib/firmware/brcm/brcmfmac43456-sdio.clm_blob
/lib/firmware/brcm/brcmfmac43456-sdio.txt
/usr
/usr/lib
/usr/lib/raspi-firmware
/usr/lib/raspi-firmware/bootcode.bin
/usr/lib/raspi-firmware/fixup.dat
/usr/lib/raspi-firmware/fixup4.dat
/usr/lib/raspi-firmware/fixup4cd.dat
/usr/lib/raspi-firmware/fixup4db.dat
/usr/lib/raspi-firmware/fixup4x.dat
/usr/lib/raspi-firmware/fixup_cd.dat
/usr/lib/raspi-firmware/fixup_db.dat
/usr/lib/raspi-firmware/fixup_x.dat
/usr/lib/raspi-firmware/start.elf
/usr/lib/raspi-firmware/start4.elf
/usr/lib/raspi-firmware/start4cd.elf
/usr/lib/raspi-firmware/start4db.elf
/usr/lib/raspi-firmware/start4x.elf
/usr/lib/raspi-firmware/start_cd.elf
/usr/lib/raspi-firmware/start_db.elf
/usr/lib/raspi-firmware/start_x.elf
/usr/share
/usr/share/doc
/usr/share/doc/raspi-firmware
/usr/share/doc/raspi-firmware/changelog.Debian.gz
/usr/share/doc/raspi-firmware/copyright
/usr/share/initramfs-tools
/usr/share/initramfs-tools/hooks
/usr/share/initramfs-tools/hooks/raspi-firmware-fsck
/usr/share/lintian
/usr/share/lintian/overrides
/usr/share/lintian/overrides/raspi-firmware
```


## Howto

remove file first

``` sh

sudo rm -f /etc/initramfs/post-update.d/z50-raspi-firmware
sudo rm -f /etc/kernel/postinst.d/z50-raspi-firmware
sudo rm -f /etc/kernel/postrm.d/z50-raspi-firmware

```

then remove package: raspi-firmware

``` sh
sudo apt-get purge raspi-firmware
```

