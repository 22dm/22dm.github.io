---
layout: post
category: "pi"
title: "树莓派中文参考"
tags: ["raspberry pi"]
---

系统安装
------------------------------

### 系统烧写

#### Windows

USB Image Tool (可免解压识别.zip/.gz包)  
XP需要.net 2.0

SD卡容量恢复：SD Formatter

#### Linux

确认设备文件：`lsblk`

* `.img`烧写：`sudo dd bs=4M if=image.img of=/dev/sdx` **危险命令，注意不要选错目标设备！！**
* `.zip`免解压直接写盘：`unzip -p image.zip *.img | sudo dd bs=4M of=/dev/sdx`
* `.gz`免解压直接写盘：`gunzip -c image.img.gz | sudo dd bs=4M of=/dev/sdx`

SD卡容量恢复：`sudo gparted`，删除所有分区，全部空间建立一个FAT32主分区。

#### 首次开机固定IP

cmdline.txt加入： `ip=192.168.0.50::192.168.0.1:255.255.255.0`  
(格式——客户端IP:服务器IP:网关IP:子网掩码)

### sudo raspi-config

* 1 Expand Filesystem 分区扩展
* 2 Change User Password 更改pi口令(推荐更改)
* 3 Enable Boot to Desktop 启动选项
    * Console 文字终端(默认)
    * Desktop 图形环境+LXDE桌面
    * Scratch 小猫编程(图形环境无桌面)
* 4 I18n options 国际化选项  
  如果界面不能显示中文，则执行`LANG=C`临时改系统语言为英语，再重进`raspi-config`。
    * I1 Locale 语言支持
        * 操作键: `PgUp`, `PgDn`, `Space`, `Tab`, `Enter`, `方向`
        * 必要：`en_US.UTF-8`
        * 必要：`zh_CN.UTF-8`
        * 可选：`zh_CN.GB18030`
        * 下一步默认预言必须选：`zh_CN.UTF-8`
    * I2 Timezone 时区 `Asia/Shanghai`
    * I3 Keyboard 键盘布局
        1. 硬件类型：Generic 104-key PC
        1. 键盘布局：Other
        1. 国家地区：Chinese
        1. 键盘布局：Chinese
        1. 变换键：No AltGr Key
        1. 多功能键：No compose key
        1. Ctrl+Alt+Bksp：Yes
* 8 Advanced Options 高级选项
    * A2 Hostname 主机名(可选更改)

### APT 软件管理器

#### 软件源镜像站

镜像站列表：

* 辽宁大连 大连东软信息学院（We·Cloud 云技术小组） <http://mirrors.neusoft.edu.cn/raspbian/raspbian/>
* 湖北武汉 华中科技大学（联创团队） <http://mirrors.hustunique.com/raspbian/raspbian/>
* 安徽合肥 中国科学技术大学（中科大Linux用户协会） <http://mirrors.ustc.edu.cn/raspbian/raspbian/>
* 广东广州 中山大学（校内开源爱好者） <http://mirror.sysu.edu.cn/raspbian/raspbian/>
* 四川成都 电子科技大学（凝聚网络安全工作室） <http://raspbian.cnssuestc.org/raspbian/>
* 山东济南 搜狐公司 （Sohu Tech-NO） <http://mirror.sohu.com/raspbian/raspbian/>

更换软件源 `sudo nano /etc/apt/sources.list`  

`sources.list`内容：

    deb <url> wheezy main contrib non-free rpi
    deb-src <url> wheezy main contrib non-free rpi

#### 系统升级

* 更新软件列表 `sudo apt-get update`  
  （安装新软件如有任何错误，就先update）
* 升级所有软件 `sudo apt-get upgrade`

#### 软件包管理

* 安装 `sudo apt-get install <name>`
* 删除 `sudo apt-get remove <name>`（只删程序）
* 卸载 `sudo apt-get purge <name>`（删程序和配置文件）
* 搜索 `apt-cache search <keyword>`

#### 实用软件包

* SCIM中文输入法：`scim-pinyin`
* fcitx小企鹅输入法：`fcitx-sunpinyin`
* 文鼎字体：`fonts-arphic-uming` `fonts-arphic-ukai`
* 文泉驿字体：`ttf-wqy-zenhei` `ttf-wqy-microhei` `xfonts-wqy`
* Google Chromium浏览器：`chromium` `chromium-l10n`
* FileZilla FTP客户端：`filezilla`
* Geany 轻量级多语言IDE：`geany`
* AbiWord 轻量级文字处理：`abiword`
* mPaint “画图”：`mtpaint`
* GIMP “Photoshop”：`gimp`
* Aria2 下载机管理器：`aria2`
* Transmission BT客户端：`transmission`
* Arduino开源硬件 IDE：`arduino`

#### 实用命令行工具

* tmux 远程登录终端复用：`tmux`  
  (掉线后程序不终止，随时恢复现场，SSH必备)
* minicom 串口超级终端：`minicom`  
  (调试树莓派自带串口必备)
* htop 任务管理器top加强版：`htop`
* lftp FTP客户端：`lftp`
* ImageMagick 命令行图像处理：`imagemagick`

常用命令
------------------------------

### Linux基础

* 命令行操作
    * Ctrl+C 终止进程（不要乱按）
    * Ctrl+D 终止文件输入、注销
    * Ctrl+W 删除最后一个单词
    * Tab 文件名/命令名/参数自动补全
    * `<` `>` `2>` 标准输入/输出/错误重定向
    * `|` 管道操作
    * ``` `<command>` ``` 以输出结果当作命令执行(eval)
    * 一般的命令行顺序：命令 参数 操作文件列表
    * 变量定义 `VARNAME='value'` 等号两边不得加空格！
    * 变量使用 `${VARNAME}`
* 基本文件操作
    * pwd
    * ls (-a -l)
    * cat
    * cp/mv
    * rm (-rf)
    * mkdir/rmdir
    * chmod (+x -x)
    * file
* 文本与管道命令
    * echo
    * less
    * head/tail
    * xargs
    * grep/egrep
    * sort
* 常用设备文件
    * /dev/zero 无限输出0x00
    * /dev/null 数据黑洞(读为空文件，写一律丢弃)
    * /dev/stdin(out,err) 标准输入/输出/错误
* 运行可执行文件 `./<filename>`
* 提升权限 `sudo <command>`
* 进入root命令行 `sudo -i`
* 注销 `logout`
* 关机重启 `sudo halt` `sudo reboot`

### 软件相关

* 文本编辑器 `nano` (vi/emacs随意，只要会用)
* 压缩包管理
    * .tar(.gz/.bz)解包 `tar xf <tarfile>`
    * .tar.gz打包 `tar zcf <tarfile> <file-dir-to-add>`
    * .zip解包 `unzip <zipfile>`
    * .zip打包 `zip <zipfile> <file-dir-to-add>`
    * Linux下推荐.tar.gz包（可记录文件权限）
    * 习惯将所有文件整理进子文件夹再打压缩包
    * 习惯解压到原位，不新建文件夹
* 进程列表 `ps aux`
* 系统监视器 `top` `htop`(须安装)
* 终止进程
    * 凭进程ID `kill <procid>`
    * 凭进程名称 `pkill <procname>`
    * 凭进程完整命令行 `pkill -f <fullcmdline>`
    * 均可加参数 `-9` 强杀进程
* 启动图形桌面 `startx`

### 开发相关

* 文本处理器 `awk` `sed`
* 16进制查看器 `od -t x1 <filename>`
* 格式化输出，任意原始字节输出 `printf`  
  (用法与C的printf极其类似)
* ASCII码表 `man ascii`

### 硬件相关

* 剩余内存 `free -h`
* 剩余存储 `df -h`
* 在线时间与负载 `uptime`
* USB设备列表 `lsusb`
* 块设备列表 `lsblk`
* 查看挂载 `mount`
* 网络信息 `ifconfig`
* 无线信息 `iwconfig`

### Wi-Fi配置

图形界面Wi-Fi配置 `wpa_gui`（桌面WiFi Config）

命令行Wi-Fi配置 `sudo wpa_cli`

```
> add_network
4 <-- 记住这个号码！
> set_network 4 ssid "Your SSID"
> set_network 4 key_mgmt WPA-PSK
> set_network 4 psk "Password"
> enable_network 4
> save_config
```

底层硬件
------------------------------

### config.txt

#### HDMI高清音视频

* 强制HDMI hdmi_force_hotplug=1
* 忽略HDMI hdmi_ignore_hotplug=1
* HDMI信号增强 config_hdmi_boost=0~7
* 禁止过扫描 disable_overscan=1
* 不读取显示器参数 hdmi_ignore_edid=0xa5000080
* CEA电视机模式 hdmi_group=1
* DMT显示器模式 hdmi_group=2
* DMT模式下HDMI输出声音 hdmi_drive=2

CEA电视机模式(hdmi_group=1)分辨率(60Hz)：

* hdmi_mode=2 480p
* hdmi_mode=4 720p
* hdmi_mode=16 1080p

DMT显示器模式(hdmi_group=2)分辨率(60Hz)：

* hdmi_mode=4 640x480
* hdmi_mode=9 800x600
* hdmi_mode=16 1024x768  
  16如有问题用17(70Hz)
* hdmi_mode=23 1280x768
* hdmi_mode=81 1366x768
* hdmi_mode=32 1280x960
* hdmi_mode=35 1280x1024
* hdmi_mode=73 1920x1440

#### RCA模拟复合视频

* 视频制式 sdtv_mode=0(NTSC)/3(PAL)
* 视频长宽比 sdtv_aspect=1(4:3)/3(16:9)
* 黑白模式 sdtv_disable_colourburst=1

#### 超频

超频选项（只调节这几项质保不失效）

* arm_freq ARM核心频率
* core_freq GPU核心频率
* sdram_freq SDRAM内存频率
* over_voltage 增加核心电压

raspi-config建议超频档位

* None： 700 250 400 0
* Modest: 800 250 400 0
* Medium: 900 250 450 2
* High: 950 250 450 6
* Turbo: 1000 500 600 6

### GPIO

**所有IO口均为3.3V电平！输入5V直接烧IO口！！！**

`[w:]`为wiringPi定义的编号。

#### P1 GPIO双排针

奇数排：

* P1-02 5V0 靠近板角
* P1-04 5V0
* P1-06 GND
* P1-08 GPIO14 [w:15] TXD
* P1-10 GPIO15 [w:16] RXD
* P1-12 GPIO18 [w:1] PWM(与模拟音频复用)
* P1-14 GND
* P1-16 GPIO23 [w:4]
* P1-18 GPIO24 [w:5]
* P1-20 GND
* P1-22 GPIO25 [w:6]
* P1-24 GPIO8 [w:10] SPI0:/CE0
* P1-26 GPIO7 [w:11] SPI0:/CE1

奇数排：

* P1-01 3V3 方块焊点
* P1-03 GPIO2 [w:8] I2C1:SDA 1K8上拉
* P1-05 GPIO3 [w:9] I2C1:SCL 1K8上拉
* P1-07 GPIO4 [w:7] 1-Wire
* P1-09 GND
* P1-11 GPIO17 [w:0]
* P1-13 GPIO27 [w:2]
* P1-15 GPIO22 [w:3]
* P1-17 3V3
* P1-19 GPIO10 [w:12] SPI0:MOSI
* P1-21 GPIO9 [w:13] SPI0:MISO
* P1-23 GPIO11 [w:14] SPI0:SCLK
* P1-25 GND

5V0在保险之后，所有3V3总和最大输出能力50mA

对于Rev1: I2C1->I2C0, GPIO:2->0, 3->1, 27->21

#### P5 Rev2附加GPIO

奇数排：

* P5-01	5V0 靠近板角,方焊点
* P5-03 GPIO28 [w:17] I2C0:SDA
* P5-05 GPIO30 [w:19]
* P5-07 GND

偶数排：

* P5-02 3V3
* P5-04 GPIO29 [w:18] I2C0:SCL
* P5-06 GPIO31 [w:20]
* P5-08 GND

Rev2另加P6接口，用镊子短路可让树莓派硬重启。

#### GPIO命令行工具：gpio

`gpio`工具由wiringPi提供。无需`sudo`。默认使用wiringPi针脚编号。

* 查看所有GPIO：`gpio readall`
* 设置模式：`gpio mode <pin> in/out/pwm`
* 设置上下拉：`gpio mode <pin> up/down/tri`
* 写入：`gpio write <pin> 0/1`
* 读取：`gpio read <pin>`
* 等待边沿：`gpio wfi <pin> rising/falling/both`

### SPI

启用SPI内核模块: `sudo raspi-config` -> 8 Advanced... -> A5 SPI

设备文件：`/dev/spidev0.0`和`0.1`（1总线，2片选）

### I2C

启用I2C内核模块:

* `/etc/modules`加两行：`i2c-bcm2708`和`i2c-dev`
* `/etc/modprobe.d/raspi-blacklist.conf`删`blacklist i2c-bcm2708`

设备文件：`/dev/i2c-0`和`i2c-1`（2总线）。  
新版(Rev2)用i2c-1，旧版(Rev1)用i2c-0。（Rev2必须焊接才能强行连接i2c-0）

I2C总线扫描：`sudo i2cdetect -y 1` (或0) (需要`i2c-tools`软件包)

### UART串口

* 设备文件：`/dev/ttyAMA0`
* 默认串口参数：115200 8N1 无流控

如果另有USB转串口设备，则在`/dev/ttyUSB*`。

#### 解除Linux终端占用

* `/boot/cmdline.txt` 删除 `console=ttyAMA0,115200 kgdboc=ttyAMA0,115200`
* `/etc/inittab` 删除 `T0:23:respawn:/sbin/getty -L ttyAMA0 115200 vt100`

#### minicom

授权用户访问串口设备 `sudo usermod -a -G dialout pi` (重登录生效)

minicom设置：`sudo minicom --setup` 需要修改：

* Serial port setup 串口配置
    * A Device = `/dev/ttyAMA0`
    * E Bps/Par/Bits = 115200 8N1
    * F HW Flow Ctrl = No
    * G SW Flow Ctrl = No
* Save setup as dfl 保存默认配置

自带编程环境
------------------------------

### 命令行

* C/C++: gcc/g++
* Python: python(Py2.7) python3(Py3K)
* Oracle Java: javac(编译器) java(虚拟机)

### 图形界面

* Python IDE: idle(Py2.7) idle3(Py3K)
* Mathematica: mathematica  
  (APT软件包：`wolfram-engine`)
* 小猫: scratch
* 音乐: sonic-pi

版本管理系统
------------------------------

### Git

最新Raspbian附带。其他系统：安装`git`或`git-core`。

建立仓库

* 空仓库 `git init`
* 克隆远程 `git clone <url>`

基本概念

* 项目主线，默认本地分支 `master`
* 默认远程分支 `origin`
* 当前分支 `HEAD`

文件操作

* 查看状态 `git status`
* 加入文件 `git add <filename>`
* 加入所有 `git add .`
* 删除文件 `git rm <filename>`
* 文件移动 `git mv <src> <dest>`

提交操作

* 更新本地副本 `git pull`
* 提交本地修改 `git commit`
* 快速提交(承认所有文件更改) `git commit -a`
* 推送远程仓库 `git push`

分支操作

* 列出分支 `git branch`
* 复制分支 `git branch <new-branch>`  
* 删除分支 `git branch -d <branch>`
* 切换分支 `git checkout <branch>`
* 并入分支 `git merge <branch>`  
* 标注标签 `git tag <v1.0>`
* 推送分支到远程 `git push origin <branch>`
* 删除远程分支 `git push origin :<branch>`

修正错误

* 查看提交历史 `git log`
* 本次提交算作上次提交 `git commit --amend`  
  (修正上次提交的错误)
* 丢弃所有未提交改动 `git reset --hard HEAD`
* 回滚到某个提交 `git revert <commit-hash>`


