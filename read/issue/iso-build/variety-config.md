---
title: Variety Config
nav_order: 9013
has_children: false
parent: ISO Build
grand_parent: 議題
---


# Variety Config


## 主題

* [前提](#前提)
* [/etc/skel](#etcskel)
* [autostart](#autostart)
* [~/.config/variety/.firstrun](#configvarietyfirstrun)
* [~/.config/variety/variety.conf](#configvarietyvarietyconf)
* [關於「variety」第一次執行時，採用的「桌面圖片」](#關於variety第一次執行時採用的桌面圖片)


## 前提

一般使用「Variety」應該很容易上手，

只要執行「variety」，然後透過「圖形介面程式」操作，就行了。

這篇主要是要紀錄，在「製作Debian ISO」時，

設定「Variety」要注意的事項。


## /etc/skel

以下範例「/etc/skel」這個資料夾裡面放置的檔案，

當透過「Live System」開機時，或是新增一個新帳號時，

就會複製到「家目錄」，也就是「~/」

在「Debian」的「Live System」開機時，帳號是「user」，密碼是「live」。

也就是「家目錄(~/)」的路徑就是「/home/user」。


## autostart

產生一個檔案「[/etc/skel/.config/autostart/variety.desktop](https://github.com/samwhelp/eznixos-adjustment-iso-profile-start-lxqt/blob/main/debian-12/locale/en_us/eznixos-adjustment-lxqt-with-openbox/asset/overlay/etc/skel/.config/autostart/variety.desktop)」，

內容如下

``` ini
[Desktop Entry]
Name=Variety
Comment=Variety Wallpaper Changer
Categories=GNOME;GTK;Utility;
Exec=sh -c 'sleep 20 && variety';
MimeType=text/uri-list;x-scheme-handler/variety;x-scheme-handler/vrty;
Icon=variety
Terminal=false
Type=Application
StartupNotify=false
Actions=Next;Previous;PauseResume;History;Preferences;
Keywords=Wallpaper;Changer;Change;Download;Downloader;Variety;
StartupWMClass=Variety
```

關於「~/.config/autostart/variety.desktop」這個檔案的功用

就是當啟動「桌面環境」時，會自動啟動「variety」。

原本「variety」自動產生的程式碼片段，類似如下，上面我有修改過


``` ini
Exec=/bin/bash -c "sleep 20 && /usr/bin/variety --profile /home/user/.config/variety/"
```

``` ini
Exec=/bin/bash -c "sleep 20 && /usr/bin/variety --profile .config/variety/"
```


## ~/.config/variety/.firstrun

因為當第一次啟動「variety」時，會出現一些對話框

來跟您確認一些「variety」的「初始設定」。

但是我們在「Live System」，我們並不希望出現對話框。

所以「[~/.config/variety/.firstrun](https://github.com/samwhelp/eznixos-adjustment-iso-profile-start-lxqt/blob/main/debian-12/locale/en_us/eznixos-adjustment-lxqt-with-openbox/asset/overlay/etc/skel/.config/variety/.firstrun)」這個檔案，

讓「variety」執行時，就不會出現對話框。



關於「[~/.config/variety/.firstrun](https://github.com/samwhelp/eznixos-adjustment-iso-profile-start-lxqt/blob/main/debian-12/locale/en_us/eznixos-adjustment-lxqt-with-openbox/asset/overlay/etc/skel/.config/variety/.firstrun)」這個檔案的內容類似如下，

只是一個時間戳記。

```
2024-02-27 11:59:33
```

## ~/.config/variety/variety.conf

雖然上面設定，讓「variety」第一次執行時，不會出現對話框，

不過第一次執行時，也會用「預設值」來做「variety」的「初始設定」，

也就是會產生一些相關的資料夾和檔案。


而「variety」的主要設定檔，就是「[~/.config/variety/variety.conf](https://github.com/samwhelp/eznixos-adjustment-iso-profile-start-lxqt/blob/main/debian-12/locale/en_us/eznixos-adjustment-lxqt-with-openbox/asset/overlay/etc/skel/.config/variety/variety.conf)」。

我們其實不需要自己產生，第一次執行時，會自動產生，

我們也可以自己產生，例如：加入一些「下載來源(sources)」

也就是我們可以自行產生「~/.config/variety/variety.conf」，內容如下

``` ini
[sources]
src1 = True|favorites|The Favorites folder
src2 = True|fetched|The Fetched folder
src3 = True|folder|/usr/share/backgrounds
src4 = True|flickr|user:www.flickr.com/photos/peter-levi/;user_id:93647178@N00;
src5 = False|apod|NASA's Astronomy Picture of the Day
src6 = False|bing|Bing Photo of the Day
src7 = False|earthview|Google Earth View Wallpapers
src8 = False|natgeo|National Geographic's photo of the day
src9 = False|unsplash|High-resolution photos from Unsplash.com
src10 = True|folder|Pictures/Wallpaper
src11 = True|wallhaven|space
```

我只是加入「`src10`」和「`src11`」。

``` ini
src10 = True|folder|Pictures/Wallpaper
src11 = True|wallhaven|space
```

其他的設定值，在「variety」啟動時，會自動用「預設值」，寫入「~/.config/variety/variety.conf」。

也就是「variety」會幫我們填補「我們沒有一開始沒有寫入的設定值」。

不過因為我要保留註解，所以我就用「variety」預設產生的「~/.config/variety/variety.conf」來做修改。

也就是上面提到的，我只是加入「`src10`」和「`src11`」。

``` ini
src10 = True|folder|Pictures/Wallpaper
src11 = True|wallhaven|space
```


## 關於「variety」第一次執行時，採用的「桌面圖片」

雖然我們在「~/.config/variety/variety.conf」有設定「[change_on_start = False](https://github.com/samwhelp/eznixos-adjustment-iso-profile-start-lxqt/blob/main/debian-12/locale/en_us/eznixos-adjustment-lxqt-with-openbox/asset/overlay/etc/skel/.config/variety/variety.conf#L2)」

不過發現到，第一次執行時，會採用「/usr/share/images/desktop-base/desktop-background.xml」，

一開始我還摸不著頭緒，

我還設定了「[~/.config/variety/history.txt](https://github.com/samwhelp/eznixos-adjustment-iso-profile-start-lxqt/blob/main/debian-12/locale/en_us/eznixos-adjustment-lxqt-with-openbox/asset/overlay/etc/skel/.config/variety/history.txt)」，內容如下，

```
0
/usr/share/backgrounds/default.jpg
/usr/share/backgrounds/default-login.jpg
/usr/share/backgrounds/default-grub.jpg
```

**不過幾經測試，結果並不是如我預期的。**

後來觀察發現，猜測「variety」第一次執行時，

應該是會去抓取「gsettings」的設定，來當作第一次執行時，採用的「桌面圖片」。

也就是會抓取「org.gnome.desktop.background picture-uri」的「設定值」。

執行下面指令，將「org.gnome.desktop.background picture-uri」還原成「預設值」

``` sh
gsettings reset org.gnome.desktop.background picture-uri
```


執行下面指令，顯示「org.gnome.desktop.background picture-uri」目前的「設定值」

``` sh
gsettings get org.gnome.desktop.background picture-uri
```

顯示

```
'file:///usr/share/images/desktop-base/desktop-background.xml'
```

這些「預設值」，在「Debian」有被「覆寫」，

被定義在「/usr/share/glib-2.0/schemas/10_desktop-base.gschema.override」這個檔案。

``` sh
cat /usr/share/glib-2.0/schemas/10_desktop-base.gschema.override
```

顯示

``` ini
[org.gnome.desktop.background]
picture-options='zoom'
picture-uri='file:///usr/share/images/desktop-base/desktop-background.xml'

[org.gnome.desktop.screensaver]
picture-options='zoom'
picture-uri='file:///usr/share/images/desktop-base/desktop-lockscreen.xml'
```

所以我們可以產生一個新的檔案「[/usr/share/glib-2.0/schemas/50_desktop-background.gschema.override](https://github.com/samwhelp/eznixos-adjustment-iso-profile-start-lxqt/blob/main/debian-12/locale/en_us/eznixos-adjustment-lxqt-with-openbox/asset/overlay/usr/share/glib-2.0/schemas/50_desktop-background.gschema.override)」，內容如下。

``` ini
[org.gnome.desktop.background]
picture-uri='file:///usr/share/backgrounds/default.jpg'
picture-uri-dark='file:///usr/share/backgrounds/default.jpg'

[org.gnome.desktop.screensaver]
picture-uri='file:///usr/share/backgrounds/default-login.jpg'
```

這樣「org.gnome.desktop.background picture-uri」的「預設值」，就會變成「'file:///usr/share/backgrounds/default.jpg'」。

**測試過後，結果就是如我預期的**


**注意**，

若是在「製作Debian ISO」，加入「/usr/share/glib-2.0/schemas/50_desktop-background.gschema.override」，並不需要做其他的額外動作。


若是在已經安裝的系統加入「/usr/share/glib-2.0/schemas/50_desktop-background.gschema.override」後，

則是要先執行下面指令，才會將「org.gnome.desktop.background picture-uri」的「預設值」，變成「'file:///usr/share/backgrounds/default.jpg'」。

``` sh
sudo glib-compile-schemas /usr/share/glib-2.0/schemas
```

接著執行下面指令，才會將「org.gnome.desktop.background picture-uri」目前的「設定值」，變成「'file:///usr/share/backgrounds/default.jpg'」。

``` sh
gsettings reset org.gnome.desktop.background picture-uri
```


接著執行下面指令，顯示「org.gnome.desktop.background picture-uri」目前的「設定值」。

``` sh
gsettings get org.gnome.desktop.background picture-uri
```

顯示

```
'file:///usr/share/backgrounds/default.jpg'
```
