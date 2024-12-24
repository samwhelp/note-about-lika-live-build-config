---
title: resolv.conf
nav_order: 9011
has_children: false
parent: ISO Build
grand_parent: 議題
---


# resolv.conf




## 遭遇情境

當在 build iso 時，若遇到下面的情形

```
can't download dctrl-tools
```

只要編輯「/etc/resolv.conf」內容如下

```
nameserver 8.8.8.8
```

或是也可以直接執行下面指令，產生「/etc/resolv.conf」

``` sh
echo 'nameserver 8.8.8.8' | sudo tee /etc/resolv.conf
```

當透過「Debian Live Build」來製作「Live ISO」時，會自動將「目前系統」的「/etc/resolv.conf」複製到「目標系統」，這樣就可以解除上面遇到的情況。
