---
title: Calamares Install User Group
nav_order: 9012
has_children: false
parent: ISO Build
grand_parent: 議題
---


# Calamares Install User Group




## 期望的系統

在我期望的系統

執行下面指令

``` sh
id
```

應該顯示類似如下

```
uid=1000(sam) gid=1000(sam) groups=1000(sam),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),106(netdev),112(bluetooth),114(lpadmin),117(scanner)
```

我的期望

* 讓「uid」保持「1000」
* 讓「gid」保持「1000」




## 遭遇情境

關於「Calamares Installer」的設定

我是複製「Package: [calamares-settings-debian](https://salsa.debian.org/live-team/calamares-settings-debian/-/tree/master/calamares?ref_type=heads)」的內容來[修改而成](https://github.com/samwhelp/lika-live-build-respin-xfce/tree/main/factory/installer/calamares)的。


其中有一個模組設定「[/etc/calamares/modules/users.conf](https://github.com/samwhelp/lika-live-build-respin-xfce/blob/main/factory/installer/calamares/modules/users.conf#L12-L13)」，內容如下


```
---
userGroup:       users
defaultGroups:
    - cdrom
    - floppy
    - sudo
    - audio
    - dip
    - video
    - plugdev
    - netdev
    - lpadmin
    - scanner
    - bluetooth
autologinGroup:  autologin
sudoersGroup:    sudo
setRootPassword: false
```

上面有設定兩個「`lpadmin`」和「`scanner`」這兩個「User Group」，

若是「Live系統」沒有這兩個「User Group」，

當透過「Calamares Installer」安裝時，

就會會自動加入這兩個「User Group」，

這樣會影響到「主要User」的「Group ID」不是「1000」，




## 解方一

編輯模組「[/etc/calamares/modules/users.conf](https://github.com/samwhelp/lika-live-build-respin-xfce/blob/main/factory/installer/calamares/modules/users.conf#L12-L13)」

刪除或是註解「`lpadmin`」和「`scanner`」那兩行。




## 解方二

若不刪除「`lpadmin`」和「`scanner`」那兩行。

就要在產生「目標系統(Live系統)」時，產生「`lpadmin`」和「`scanner`」這兩個「Group」。

在安裝一些「Package」時，在「Postinst」階段，就會自動產生這兩個「Group」，

可以參考以下的「[探索紀錄](#探索紀錄)」，和我整理的「[Package List](https://github.com/samwhelp/lika-live-build-respin-xfce/blob/main/asset/package/install/master-printer-scanner.list.chroot)」




## 探索紀錄


## For Group: lpadmin

> [cups-client](https://packages.debian.org/stable/cups-client)

> [cups-daemon](https://packages.debian.org/stable/cups-client)

執行

``` sh
grep lpadmin /etc/group
```

顯示

```
lpadmin:x:114:user
```

執行

``` sh
grep lpadmin /var/lib/dpkg/info/*.postinst -R | grep addgroup | less
```

顯示

```
/var/lib/dpkg/info/cups-client.postinst:            addgroup --system lpadmin
# /var/lib/dpkg/info/cups-daemon.postinst:        addgroup --system lpadmin
```



## For Group: scanner

> [sane-utils](https://packages.debian.org/stable/sane-utils)

> [libsane1](https://packages.debian.org/stable/libsane1)

執行

``` sh
grep scanner /etc/group
```

顯示

```
scanner:x:117:saned,user
```

執行

``` sh
grep scanner /var/lib/dpkg/info/*.postinst -R | grep addgroup | less
```

顯示

```
/var/lib/dpkg/info/colord.postinst:         addgroup --quiet --system scanner
/var/lib/dpkg/info/libsane1:amd64.postinst:     addgroup --quiet --system scanner || true
```

執行

``` sh
grep scanner /var/lib/dpkg/info/*.postinst -R | less
```

顯示

```
/var/lib/dpkg/info/apparmor.postinst:            for i in usr.bin.media-hub-server usr.bin.mediascanner-service-2.0 usr.lib.mediascanner-2.0.mediascanner-extractor usr.bin.messaging-app usr.bin.webbrowser-app ; do
/var/lib/dpkg/info/colord.postinst:# create the scanner group if it isn't already there
/var/lib/dpkg/info/colord.postinst:        if ! getent group scanner >/dev/null; then
/var/lib/dpkg/info/colord.postinst:	    addgroup --quiet --system scanner
/var/lib/dpkg/info/libsane1:amd64.postinst:    # Add the scanner system group if it doesn't exist
/var/lib/dpkg/info/libsane1:amd64.postinst:    if ! getent group | grep -q "^scanner:"; then
/var/lib/dpkg/info/libsane1:amd64.postinst:	echo "Adding scanner group..."
/var/lib/dpkg/info/libsane1:amd64.postinst:	addgroup --quiet --system scanner || true
/var/lib/dpkg/info/sane-utils.postinst:    db_get sane-utils/saned_scanner_group
/var/lib/dpkg/info/sane-utils.postinst:	adduser --quiet saned scanner
/var/lib/dpkg/info/sane-utils.postinst:	if id saned | grep -q "groups=.*\(scanner\)"; then
/var/lib/dpkg/info/sane-utils.postinst:	    deluser --quiet saned scanner
```




## 連結

* Debian Wiki / [SystemGroups](https://wiki.debian.org/SystemGroups)
* Debian Wiki / [Printing](https://wiki.debian.org/Printing)
* Debian Wiki / [SystemPrinting](https://wiki.debian.org/SystemPrinting)
