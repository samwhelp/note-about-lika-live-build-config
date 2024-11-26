

# 首頁

> Lika OS / Live Build Config / 探索筆記

| Link | GitHub |
| ---- | ------ |
| [Lika OS / Live Build Config / 探索筆記](https://samwhelp.github.io/note-about-lika-live-build-config/) | [GitHub](https://github.com/samwhelp/note-about-lika-live-build-config) |
| [Lika OS 探索筆記](https://samwhelp.github.io/note-about-lika/) | [GitHub](https://github.com/samwhelp/note-about-lika) |





## 主題

* [ISO Profile](#iso-profile)
* [Boot ISO By GRUB](#boot-iso-by-grub)
* [相關筆記](#相關筆記)




## Lika OS / Live System

| Account  | Value  |
| -------- | ------ |
| Username | `lika` |
| Password | `live` |

若想要移除目前帳號的密碼，可以執行下面指令

``` sh
sudo passwd -d $(whoami)
```




## ISO Profile

> 關於「lika-live-build-config」這個專案，是「Live ISO 產生架構」。

| Link | GitHub |
| ---- | ------ |
| [lika-live-build-config](https://samwhelp.github.io/lika-live-build-config/) | [GitHub](https://github.com/samwhelp/lika-live-build-config) |

> 以下的專案，則是會複製「lika-live-build-config」這個專案。

> 並將「自訂的設定檔」，覆蓋到「lika-live-build-config」這個專案。

> 最後再進行「製作 Debian Live ISO」的流程。

| Link | GitHub |
| ---- | ------ |
| [lika-live-build-config-using](https://samwhelp.github.io/lika-live-build-config-using/) | [GitHub](https://github.com/samwhelp/lika-live-build-config-using) |
| [lika-live-build-config-remix](https://samwhelp.github.io/lika-live-build-config-remix/) | [GitHub](https://github.com/samwhelp/lika-live-build-config-remix) |
| [lika-live-build-config-enhance](https://samwhelp.github.io/lika-live-build-config-enhance/) | [GitHub](https://github.com/samwhelp/lika-live-build-config-enhance) |


| Link | GitHub |
| ---- | ------ |
| [lika-live-build-respin-xfce](https://samwhelp.github.io/lika-live-build-respin-xfce/) | [GitHub](https://github.com/samwhelp/lika-live-build-respin-xfce) |
| [lika-live-build-respin-kde](https://samwhelp.github.io/lika-live-build-respin-kde/) | [GitHub](https://github.com/samwhelp/lika-live-build-respin-kde) |
| [lika-live-build-respin-lxqt](https://samwhelp.github.io/lika-live-build-respin-lxqt/) | [GitHub](https://github.com/samwhelp/lika-live-build-respin-lxqt) |
| [lika-live-build-respin-mate](https://samwhelp.github.io/lika-live-build-respin-mate/) | [GitHub](https://github.com/samwhelp/lika-live-build-respin-mate) |
| [lika-live-build-respin-cinnamon](https://samwhelp.github.io/lika-live-build-respin-cinnamon/) | [GitHub](https://github.com/samwhelp/lika-live-build-respin-cinnamon) |
| [lika-live-build-respin-gnome](https://samwhelp.github.io/lika-live-build-respin-gnome/) | [GitHub](https://github.com/samwhelp/lika-live-build-respin-gnome) |
| [lika-live-build-respin-openbox](https://samwhelp.github.io/lika-live-build-respin-openbox/) | [GitHub](https://github.com/samwhelp/lika-live-build-respin-openbox) |
| [lika-live-build-respin-bspwm](https://samwhelp.github.io/lika-live-build-respin-bspwm/) | [GitHub](https://github.com/samwhelp/lika-live-build-respin-bspwm) |
| [lika-live-build-respin-i3](https://samwhelp.github.io/lika-live-build-respin-i3/) | [GitHub](https://github.com/samwhelp/lika-live-build-respin-i3) |
| [lika-live-build-respin-herbstluftwm](https://samwhelp.github.io/lika-live-build-respin-herbstluftwm/) | [GitHub](https://github.com/samwhelp/lika-live-build-respin-herbstluftwm) |


| Link | GitHub |
| ---- | ------ |
| [lika-live-build-respin-lxqt-with-kwin](https://samwhelp.github.io/lika-live-build-respin-lxqt-with-kwin/) | [GitHub](https://github.com/samwhelp/lika-live-build-respin-lxqt-with-kwin) |
| [lika-live-build-respin-xfce-with-kwin](https://samwhelp.github.io/lika-live-build-respin-xfce-with-kwin/) | [GitHub](https://github.com/samwhelp/lika-live-build-respin-xfce-with-kwin) |




## Boot ISO By GRUB

> 將產出的「iso檔案」放置到「`/opt/iso/lika/latest/lika.iso`」這個路徑

> 產生一個檔案「`/boot/grub/custom.cfg`」，內容如下

``` sh
menuentry "Lika OS" --class Debian {
	set iso_file="/opt/iso/lika/latest/lika.iso"
	search --set=iso_partition --no-floppy --file $iso_file
	probe --set=iso_partition_uuid --fs-uuid $iso_partition
	set img_dev="/dev/disk/by-uuid/$iso_partition_uuid"
	loopback loop ($iso_partition)$iso_file

	set extra_option=""
	#set extra_option="components quiet splash"

	set locale_option=""
	#set locale_option="locales=en_US.UTF-8"
	#set locale_option="locales=zh_TW.UTF-8"
	#set locale_option="locales=zh_CN.UTF-8"
	#set locale_option="locales=zh_HK.UTF-8"
	#set locale_option="locales=ja_JP.UTF-8"
	#set locale_option="locales=ko_KR.UTF-8"

	set boot_option="${locale_option} ${extra_option}"
	linux	(loop)/live/vmlinuz boot=live buuid=${iso_partition_uuid} findiso=${iso_file} ${boot_option}
	initrd	(loop)/live/initrd.img
}
```

> 重新開機後，就會在「GRUB」的開機選單，看到「`Lika OS`」這個選項。




## 相關筆記

| Link | GitHub |
| ---- | ------ |
| [Debian 探索筆記](https://samwhelp.github.io/note-about-debian/) | [GitHub](https://github.com/samwhelp/note-about-debian) |
| [EznixOS 探索筆記](https://samwhelp.github.io/note-about-eznixos/) | [GitHub](https://github.com/samwhelp/note-about-eznixos) |
| [Lika OS 探索筆記](https://samwhelp.github.io/note-about-lika/) | [GitHub](https://github.com/samwhelp/note-about-lika) |




## Samwhelp

* [個人筆記](https://samwhelp.github.io/book/)
