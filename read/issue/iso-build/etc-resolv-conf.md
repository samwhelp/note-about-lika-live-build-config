---
title: /etc/resolv.conf
nav_order: 9011
has_children: false
parent: ISO Build
grand_parent: 議題
---


# /etc/resolv.conf




## 遭遇情境

當在「build iso」時，若遇到下面的情形

```
can't download dctrl-tools
```

只要在「build iso」前，編輯「主系統」的「/etc/resolv.conf」內容如下

```
nameserver 8.8.8.8
```

或是也可以直接執行下面指令，產生「主系統」的「/etc/resolv.conf」

``` sh
echo 'nameserver 8.8.8.8' | sudo tee /etc/resolv.conf
```

若是要附加到「/etc/resolv.conf」，則是執行下面指令（多了個參數「-a」）

``` sh
echo 'nameserver 8.8.8.8' | sudo tee -a /etc/resolv.conf
```

> 上面的「`8.8.8.8`」也可以改為「`1.1.1.1`」


當透過「Debian Live Build」來製作「Live ISO」時，會自動將「主系統」的「/etc/resolv.conf」複製到「目標系統」，這樣就可以解除上面遇到的情況。


> 請參考下面的實作: live-build / scripts

| live-build / scripts |
| -------------------- |
| [/usr/lib/live/build/chroot_resolv](https://salsa.debian.org/live-team/live-build/-/blob/master/scripts/build/chroot_resolv)
