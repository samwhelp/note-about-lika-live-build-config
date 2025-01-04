---
title: ISO Build
nav_order: 9010
has_children: true
parent: 議題
---


# ISO Build




## Tips


### Dir: .build

cd `.build` dir

``` sh
cd /opt/tmp/lika/work/iso-profile/.build
```

run to sort by time

``` sh
ls -alt
```

run to sort by time, reverse order

``` sh
ls -altr
```


* [man ls](https://manpages.debian.org/stable/coreutils/ls.1.en.html)


## Explore

run

``` sh
cat /opt/tmp/lika/work/iso-profile/binary/.disk/mkisofs
```

show

```
xorriso -as mkisofs -R -r -J -joliet-long -l -cache-inodes -iso-level 3 -isohybrid-mbr /usr/lib/ISOLINUX/isohdpfx.bin -partition_offset 16 -A "Lika-Linux" -p "live-build 20230502; https://salsa.debian.org/live-team/live-build" -publisher "Lika" -V "Lika-Live" --modification-date=2025010315195500 -b isolinux/isolinux.bin -c isolinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -eltorito-alt-boot -e boot/grub/efi.img -no-emul-boot -isohybrid-gpt-basdat -isohybrid-apm-hfsplus -o live-image-amd64.hybrid.iso binary
```

> Refer: [/usr/lib/live/build/binary_iso](https://salsa.debian.org/live-team/live-build/-/blob/master/scripts/build/binary_iso?ref_type=heads#L189-L192)
