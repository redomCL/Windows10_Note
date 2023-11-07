http://bbs.wuyou.net/forum.php?mod=viewthread&tid=303679

http://bbs.wuyou.net/forum.php?mod=viewthread&tid=299643&fromuid=396698 

# 1.Windows10引导方案(当UEFI配置了直接指向bootmgfw.efi的方案(Windows Boot Manager))：
* UEFI BIOS→ESP分区→\EFI\Microsoft\Boot\bootmgfw.efi→(读取)\EFI\Microsoft\Boot\BCD→\C:(Windows系统分区(俗称C盘）)→\Windows\System32\winload.efi。
* 引导结束，整个硬件系统由Windows操作系统完成接管。

# 2.Windows10引导方案(UEFI下直接操作读盘引导)：
* UEFI BIOS→ESP分区→\EFI\Boot\bootx64.efi→(读取)\EFI\Microsoft\Boot\BCD→\C:(Windows系统分区(俗称C盘）)→\Windows\System32\winload.efi。
* 引导结束，整个硬件系统由Windows操作系统完成接管。

# 3.GPT+UEFI引导范式(UEFI下直接操作读盘引导)：
* UEFI BIOS→ESP分区→\EFI\Boot\bootx64.efi→读取类似BCD的引导菜单

## 注：

* 1.bootmgfw.efi：Windows10自己的引导器，配合并编辑BCD引导菜单可以实现轮转，纯净的Win10系统下，这份文件也会在  ESP\EFI\Boot下以bootx64.efi的名字存在一份。Windows Boot Manager档案会在Windows10安装之初，由Windows程序将其保存在UEFI的NVRAM区域。

* 2.bootx64.efi：任意有效引导文件，可以是Windows，Linux，macOS等任意有效的引导器改名而来，接下来的引导将会因不同系统出现差异。位于GPT分区表硬盘的ESP\EFI\Boot下，这是GPT+UEFI引导的第一步。

* 3.BCD为Windows10引导菜单。可用相关工具编辑实现轮转！不同系统可能以efi+某格式扩展名的引导菜单结合进行引导和轮转。Windows的引导菜单为BCD。

* 4.ESP分区本质为GPT分区表硬盘中的一块fat16文件系统的分区，一般位于初始扇区（0磁道0柱面1扇区）。

![](https://github.com/redomCL/Windows10-GPT-UEFI/blob/main/%E6%88%AA%E5%9B%BE/%E6%88%AA%E5%9B%BE.png)
