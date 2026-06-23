---
title: 移动固态硬盘烧录Ubuntu20.04
published: 2025-01-05
updated: 2026-06-23
description: 各种 Ubuntu 镜像下载地址
image: /assets/bolg_cover/ubuntu20-use.webp
tags: [Ubuntu, 镜像, 硬盘]
category: ubuntu
draft: false
author: larry
password: ""
passwordHint: ""
---



---

> 这篇文章应该是硬盘买来后一个月记录的，这里的话整理一下，去掉许多基础内容，若想查看原文可以在`语雀`中查看`Ubuntu`部分

# 前言

在新设备 `Samsung T7 Shile` 上配置 `Ubuntu 20.04`，作为新的开发环境，主要是因为原先的 `Ubuntu 18.04` 不能分屏显示、笔记本音量与亮度不能调节等问题，下面用来记录在新设备上的配置操作。

# 1. Ubuntu 安装

Ubuntu操作系统安装在移动固态硬盘，以实现在不同电脑即插即用。

## a. 前期准备

- 一个用于制作系统启动盘的U盘（读卡器加cd卡也可以）
- rufus软件：直接百度搜索
- DiskGenius软件（用于磁盘分区）
- 待安装系统的移动固态硬盘SSD或者一个U盘（尽量大一点）

**软件链接（百度一下也可以找到）：**

rufus 软件 ：https://pan.quark.cn/s/29ba2e6bbc06

DiskGenius 软件 ：https://pan.quark.cn/s/274b121b3814

## b. 制作系统启动盘

### i. Ubuntu 20 镜像下载

到[Ubuntu](https://releases.ubuntu.com/)官网找到自己想要的版本。

![1774882999964-d7c4111c-8b0f-453d-8120-b035f76c2e36](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623163534108.webp)

如果下载太慢，可以到[清华镜像源网站](https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/)下载

![1774883000058-7ae88490-ac6c-43c0-882b-aad4b7abcc18](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623163542903.webp)

### ii. 制作U盘启动盘

打开 `rufus` 软件，依据提示添加镜像文件，在一一选择进行镜像刻录即可

## c. 磁盘分区（重点）

1. 打开DiskGenius软件，选中自己准备装系统的磁盘（根据型号确定），比如我这里是 `**SamsungPSSD T7**`。
2. 选中磁盘，鼠标右键选择 **`“转换分区表类型为GUID模式”`**（这一步很关键，决定了移动硬盘上的Ubuntu系统插在不同电脑上都能运行）

![1774883000126-f305d139-2e21-4945-909e-225e022c6b69](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623163811615.webp)

3. 磁盘分区，可以分四个区： 
   - ESP(0)分区 ：文件系统类型为FAT32，大小我分配为1.0GB。
     该分区用于Linux系统的 /boot引导分区，后续启动 Ubuntu 系统的引导文件将会放在这个分区下的EFI目录，所以这个分区很重要。
   - 分区(1)：文件系统类型为Linux swap partition，大小我分配为16.0GB。
     该分区用于Linux系统的swap交换空间。
   - 分区(2)：文件系统类型为EXT4，大小我分配为64GB。
     该分区用于Linux系统的 “/” 目录。
   - 分区(3) ：文件系统类型为EXT4大小我分配为320 GB。
     该分区用于Linux系统的 “/home” 目录。
     我这里是将系统安装在1T的移动固态硬盘，除了上述4个分区外，还剩530G左右，剩下的这部分可以当作一个正常的存储硬盘来用。

4. 特别提醒：个人测试的情况是，只有 转换分区表类型为GUID模式，且硬盘的最开始位置是ESP分区(EFI格式) 才能保证移动硬盘上安装的Ubuntu系统插在不同的电脑上都能运行。

5. 上面的是在网址上粘贴的，以实际操作为主，这是我的分区情况

   - 分区表类型：选择 GUID（GPT）
   - ESP 分区设置：必须勾选，分配容量≥100M
   - MSR 分区：不创建、直接取消勾选
   - 系统分区规划（共 4 个分区）
     - 交换分区（swap）：16GB
     - 根目录 /：150GB
     - 用户目录 /home：200GB
     - 剩余空间：格式化为 exFAT

   > 原文：在使用分区工具时，分区表类型选择GUID，ESP分区勾选上（分配100M以上），MSR去掉，在分四个区，交换分区16GB，根目录 /  150GB ，/home目录  分配200GB ，最后归于 exFAT 类型的 硬盘 ，这是我的分区。

## d. 系统安装

1. 在电脑上同时插入U盘启动盘和准备安装系统的移动固态硬盘；
2. 重启电脑进入BIOS（我的电脑型号为联想拯救者 REN-7000K，开机按F2进入BIOS）
3. 进入BIOS后，设置启动优先级为U盘启动优先

4. 接下来的过程比较常规，`安装Ubuntu` -> `选择语言` -> `正常安装` -> `安装类型为“其他选项”` -> <span style="background:#FF9933;">安装启动引导器的设备(一定要选择 /boot 对应的分区ESP0, 不然启动不了) </span>-> `选择地区` -> `用户名、密码设置` -> `等待安装完成`

这部分的具体过程(图文详细)可以参考以下两个教程：

[在移动硬盘上安装Ubuntu20.04教程](https://zhuanlan.zhihu.com/p/424967021)

[新手安装 Ubuntu 操作系统步骤教程](https://blog.csdn.net/weixin_42776111/article/details/84961031)

## e. boot-repair

按照上述过程安装系统后，根据提示拔掉U盘，能够正常进入Ubuntu系统。
但是，当把移动固态硬盘插到其他电脑时，是不能正常使用的，原因是移动硬盘的ESP分区中没有引导文件，解决过程如下：

1. 重新插上U盘和移动固态硬盘，进入BIOS选择U盘优先启动，进入U盘中的Ubuntu系统后，选择 **Try Ubuntu without installing**（这里是进入的启动盘，不是安装好的那个系统内）
2. 连接网络，安装 **boot-repair** 软件：

   ```bash
   sudo apt-add-repository ppa:yannubuntu/boot-repair
   
   sudo apt-get update
   
   sudo apt-get install boot-repair
   ```

3. 安装完成后，打开终端执行该软件

   ```bash
   # 打开一个终端
   boot-repair
   ```
4. 选择 **“Recommended repair”** ，等待修复完成
   
   ```ini
   如何确认修复完成：
   
   如果修复完成，在ESP分区会出现一个名为 EFI 的目录，里面有 BOOT 和 ubuntu 两个子目录，用来启动 Ubuntu 系统的引导文件就是位于 ubuntu 目录中的 shimx64.efi 文件
   
   确认修复完成后，就可以将移动固态硬盘插在不同电脑上运行Ubuntu系统了。
   ```

这部分过程主要参考以下教程：

[移动固态硬盘中安装Ubuntu18.04，并且运行于其他电脑](https://blog.csdn.net/chenxinabcd/article/details/108398371)

[移动硬盘安装Ubuntu系统（UEFI引导）的一些记录](https://blog.csdn.net/u012939909/article/details/80753115)

## f. 关键点汇总

1. 移动固态硬盘需要通过DiskGenius（或类似功能的软件）将磁盘类型转换为GUID格式。
2. 为避免安装系统时出现磁盘分区不对齐、引导分区未在磁盘最开始位置等问题，最好提前将磁盘创建好分区。
3. 安装系统时，选择安装启动引导器的设备，一定要选择所创建的ESP(0)分区, 不然启动不了，因为ESP(0)分区对应系统的/boot引导分区。
4. 为了使移动硬盘上安装的Ubuntu系统在不同电脑上运行，需要执行 boot-repair 过程，创建ESP分区的EFI目录。
5. 参考链接：https://blog.csdn.net/hypc9709/article/details/127941834

## g. 关于系统迁移与备份

暂且不弄（最后也没有弄）

## h. 一些配置

进行所有的软件配置之前，先把火狐与office卸载，因为后面系统使用中有对应的替代品，并且进行系统的更新与配置Ubuntu 中的**软件与更新**。

### i. 卸载 火狐与office

```bash
# 火狐
sudo apt-get remove --purge firefox  

# office
sudo apt-get remove libreoffice-common 

# 卸载多余依赖
sudo apt autoremove
```

### ii. 软件与更新

打开**软件与更新**

![1774883000194-1d7095bb-eb10-4d24-9ff8-7ca1512c9ef4](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623165632971.webp)

将其中的配置变成与下面的图片一致

![1774883004300-1c8239a0-c891-46b3-a97f-a80dd68b29b9](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623165743419.webp)

这里还进行了系统的包更新，将系统安装的 `NVIDIA 525` 驱动版本升级到了`NVIDIA 535`。

![1774883004353-527bca84-9f3a-4cb6-8b5b-389d6a4c76bc](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623165855434.webp)

---

![1774883000278-dec7bf36-406c-4bfc-a660-3fe3cba2b96f](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623165916802.gif)

---

# 2. Ubuntu 美化

![1774883004427-9c37e326-f1ce-49b9-bb00-bf38bad868b2](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623170100109.webp)

需要进行美化一下，方便日常办公使用。

- 参考以下链接：
  -  [https://www.gnome-look.org/browse?cat=108](https://www.gnome-look.org/browse?cat=108)    
  - [史上最良心的 Ubuntu desktop 美化优化指导(1)](https://zhuanlan.zhihu.com/p/63584709)    
  - [Ubuntu 20.04 桌面美化](https://zhuanlan.zhihu.com/p/176977192)    
  - [如何更换 Ubuntu 系统的 GDM 登录界面背景](https://zhuanlan.zhihu.com/p/58834994)    
  - [Ubuntu 桌面美化教程](https://blog.csdn.net/FSKEps/article/details/122269118)    
  - [【Linux】Ubuntu美化主题【教程】](https://blog.csdn.net/qq_44940689/article/details/133094363?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522A4D4C6E4-1D47-4D24-AA24-6CEFF449F59D%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=A4D4C6E4-1D47-4D24-AA24-6CEFF449F59D&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-133094363-null-null.142^v100^pc_search_result_base5&utm_term=ubuntu%E7%BE%8E%E5%8C%96&spm=1018.2226.3001.4187)    
  - [Ubuntu桌面美化教程，手把手教你。含GRUB引导界面美化。](https://blog.csdn.net/2301_76911706/article/details/133000145?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_utm_term~default-0-133000145-blog-133094363.235^v43^pc_blog_bottom_relevance_base2&spm=1001.2101.3001.4242.1&utm_relevant_index=3)
- 所有的美化包都可以从以下这个网站下载。    
  - [https://www.gnome-look.oC](https://www.gnome-look.oC)

## a. Tweak

首先安装美化工具 `Tweak`

```bash
sudo apt-get install gnome-tweak-tool
```

**安装完毕后在菜单中打开`Tweak`**，如果apperenance中Shell部分出现了感叹号，则使用下面的语句安装依赖

```bash
sudo apt-get install gnome-shell-extensions
```

安装之后在扩展中开启**ubuntu dock** 和**ubuntu appindicators**。

## b. 美化任务栏

Ubuntu 20.04 默认的任务栏在桌面左侧，不使用时会自动隐藏。安装 plank dock 工具可以在桌面底部设置一个常驻任务栏。

### i. 安装与配置

****

```bash
# 安装
sudo apt install plank

# 打开配置界面
plank --preferences
```

**打开 plank 的配置界面，如图**

- 外观选项
  - 可以设置任务栏在桌面中的位置，任务栏的样式，任务栏中图标的大小、图标缩放（鼠标移到对应图标上，会有放大效果）等。


- 行为选项
  - 设置任务栏的隐藏模式

- 小部件
  - 提供了一些小工具，如 CPU monitor, applications 等

![image-20260623171951058](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623171951191.webp)

### ii. 设置开机自启动

打开优化工具（gnome-tweaks），将 plank 添加到开机自启动程序栏中。

![image-20260623171651458](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623171651573.webp)

### iii. 固定常用软件

安装 plank 后，任务栏中默认不会固定任何软件。

打开某个软件后，会在底部任务栏中显示，可以通过鼠标右键将其固定在 Dock 上。

![image-20260623172059220](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623172059352.webp)

## c. 更换主题

### i. 下载插件

1. **下载**

任意浏览器在扩展功能搜索插件GNOME Shell，下载并启用，这里推荐火狐浏览器或Chrome浏览器。或者

```bash
sudo apt install chrome-gnome-shell
```

![image-20260623172303429](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623172303523.webp)

2. **添加**

- 打开扩展，或者直接进入https://extensions.gnome.org/ 
- 然后安装Blur my shell、user themes、Hide Dash X这三个组件
  - Blur my shell可以让你的任务栏，Topbar，和窗口变成半透明风格
  - user themes用于更改图标和安装你喜欢的主题

Dash to Dock 用于将Dock隐藏，还需要 `sudo apt remove gnome-shell-extension-ubuntu-dock` 这条命令后重启。

![image-20260623172710758](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623172710863.webp)![image-20260623172730267](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623172730394.webp)

3. **配置**

打开Blur my shell，调整设置和参数，让你需要的地方变为半透明。

![image-20260623172934710](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623172934844.webp)

![aaa](https://1831996731.cdn.123clouddisk.com/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623172959484.webp)

### ii. 布局主题

- 主题包：**WhiteSur Gtk Theme**
  - 网址
    - https://www.gnome-look.org/p/1403328/
  - 主题包 系统存放位置
    - **/usr/share/themes/**
    - **[布局主题 _WhiteSur-gtk-theme-2024.09.02.tar.gz](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623173945279.zip)  (直接执行这里面的install.sh)(最终使用的)**

将下载好的主题包解压到系统存放位置，或者解压后拷贝到系统存放位置。

### iii. Icon 图标

- Icon 图标：**WhiteSur icon theme**
  - 网址
    - https://www.pling.com/p/1405756/
  - Icon 图标 系统存放位置
    - **/usr/share/icons**
    - **[Icon图标_WhiteSur-yellow.zip](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623174236346.zip) ( 最终使用的)**

将下载好的主题包解压到系统存放位置，或者解压后拷贝到系统存放位置。

![image-20260623174207385](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623174207556.webp)

### iv. 鼠标图标

- 鼠标图标
  - **McMojave cursors**  网址
    - https://www.pling.com/p/1355701/
  - **Saturn** 网址
    - https://www.pling.com/p/2194350
  - **Layan cursors** 网址
    - https://www.pling.com/p/1365214
  - 鼠标图标 系统存放位置
    - **/usr/share/icons**
    - **[鼠标图标_02-Layan-cursors.tar.xz](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623174510200.xz)  (最终使用的)**


将下载好的主题包解压到系统存放位置，或者解压后拷贝到系统存放位置。

![image-20260623174454985](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623174455096.webp)

### v. 最终配置

![image-20260623175032194](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623175032381.webp)

##  d. GRUB 主题

> [!TIP]
>
> - **1、下载 GRUB 主题**
>
> 首先，你需要从网上下载一个 GRUB 主题。可以在许多网站上找到主题，比如：https://www.pling.com/browse?cat=109&ord=latest或 GitHub 上的相关项目。
>
> - **2、解压缩主题**
>
> 下载的主题通常会以压缩文件格式（.zip 或 .tar.gz）提供。你需要解压缩它。
>
> 例如，如果你的主题文件名为 grub-theme.zip，可以使用以下命令解压：
>
> `unzip grub-theme.zip`
>
> 如果是 .tar.gz 文件：
>
> `tar -xzvf grub-theme.tar.gz`
>
> - **3、移动主题文件**
>
> 将解压后的主题文件夹移动到 GRUB 主题目录。通常，GRUB 主题位于 /boot/grub/themes/ 目录下。如果没有这个目录，可以创建它。
>
> `sudo mkdir -p /boot/grub/themes sudo mv /path/to/extracted/theme-folder /boot/grub/themes/`
>
> 确保替换 /path/to/extracted/theme-folder 为你的实际路径。
>
> - **4、配置 GRUB 主题**
>
> 接下来，在 GRUB 配置文件中设置新主题。编辑 /etc/default/grub 文件：
>
> `sudo nano /etc/default/grub`
>
> 找到 GRUB_THEME 行，如果没有这个行，可以添加一行。例如：
>
> `GRUB_THEME="/boot/grub/themes/theme-folder/theme.txt"`
>
> 替换 theme-folder 和 theme.txt 为你主题文件夹中的实际名称和文件名。
>
> - **5、更新 GRUB**
>
> 完成设置后，你需要更新 GRUB(很重要)，以便加载新的主题：
>
> `sudo update-grub`
>
> - **6、重启计算机**
>
> 最后重启计算机查看新的 GRUB 主题：
>
> `sudo reboot`
>
> 完成上述步骤后，你应该能够在 GRUB 菜单中看到新的主题。

### i. 配置

上面的是参考网址上面的内容，但是其实只要官方网址下载的时候，下载 **.tar.xz**格式就可以，其中有安装文件 **install.sh**,上面的也没有错误。

- 少量推荐：

  - **[MacOS Monterey inspired grub theme （最终使用的）](https://www.pling.com/p/1577873)**

    -   **GRUB主题_monterey-grub-theme.tar.xz** 
        ![image-20260623175452817](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623175453225.webp)

  - [BigSur GRUB Theme](https://www.pling.com/p/1443844)

    ![image-20260623175547689](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623175547854.webp)

  - [Elegant-wave-grub-themes](https://www.pling.com/p/2206122)

    ![image-20260623175628585](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623175628833.webp)

    ### ii. 成功

    ![image-20260623175724995](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623175725166.webp)

    ---

    ![v2-cb8443f07a41298e45191cef11b90fd2](https://vip.123pan.cn/1831996731/a_PicBed/ubuntu/ubuntu20-use/20260623175747640.gif)

    ---
