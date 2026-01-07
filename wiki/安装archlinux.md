
本文大量参考了[Shorin-ArchlinuxGuide](https://github.com/SHORiN-KiWATA/ShorinArchExperience-ArchlinuxGuide/wiki/安装ArchLinux)的系统安装部分。

其他重要概念讲解，如文件系统、分区、挂载等，请看[这里](https://github.com/SHORiN-KiWATA/ShorinArchExperience-ArchlinuxGuide/wiki/安装ArchLinux#重要概念讲解)。

有两种安装方法，手动安装和`archinstall`脚本安装，两种方法都可以且基本等价，后者会更简单一点。


## 准备工作

通过 bios 启动进入 live 安装界面，在正式安装系统之前，需要做一些准备工作。

### 连接网络

有线网自动连接，使用如下命令确认网络连接状态：
```sh
ip a #查看网络连接信息
ping -c 3 bilibili.com #确认网络正常
```

`-c 3` 表示测试3次，Ctrl+C可以中止正在运行的命令。


> **提示**：手机可以通过数据线连接通过USB共享网络。


#### 连接wifi

使用`iwctl`命令行工具连接wifi（此工具由`iwd`提供）

1. 启动iwctl工具，此时提示符会产生变化。

    ```sh
    iwctl
    ```

2. 连接wifi

    ```sh
    station wlan0 connect <此处是你的wifi名字（不能是中文）>
    ```
    其他命令：
    ```sh
    device list #列出设备
    station wlan0 scan  # 扫描网络
    station wlan0 get-networks  # 列出所有扫描的wifi
    station wlan0 show #查看连接状态
    station wlan0 disconnect #断开连接
    ```

3. 退出iwctl

    ```sh
    exit
    ```

4. 测试网络

    ```sh
    ping -c 3 bilibili.com
    ```


### 同步时间

连接到互联网上的公共时间服务器：
```sh
timedatectl set-ntp true 
```

### 自动设置镜像源

用reflector配置最快最新的国内镜像源，大幅提高下载速度。需要等待一会儿。

```sh
reflector -a 24 -c cn -f 10 --sort score --save /etc/pacman.d/mirrorlist --v
```
说明：
```
-a（age） 24 指定最近24小时更新过的源
-f（fastest） 10 筛选出下载速度最快的10个
--sort score 按照下载速度和同步时间综合评分并排序，比单纯按照下载速度排序更可靠
--save /etc/pacman.d/mirrorlist 将结果保存到/etc/pacman.d/mirrorlist
--v（verbose） 过程可视化
```

### 更新密钥

通过`pacman`更新密钥。pacman是包管理器，管理软件的安装、卸载之类的。
```sh
pacman -Sy archlinux-keyring
```

### 查看硬盘分区

#### 查看分区情况

```sh
lsblk -pf  #查看当前分区情况
fdisk -l /dev/想要查询详细情况的硬盘  #小写字母l，查看详细分区信息
```

#### 检查分区表类型

需要保证硬盘的分区表类型为GPT，看`fdisk -l`的输出信息的 `Disklabel type` 是不是 gpt。

如果不是，使用 `gdisk` 命令修改：
```sh
gdisk /dev/硬盘盘符
```
输入 w 回车确认更改，注意这一步会格式化磁盘。


#### 硬盘格式化

如果需要对某个硬盘进行格式化，可以用 `cfdisk` 命令
```sh
cfdisk /dev/nvme0n1 #选择要使用的硬盘
```

- 新硬盘会弹出选项，选GPT

- **方法**：`Delete`原有分区 > `Write`写入更改。



## 正式安装

接下来正式安装archlinux，手动安装或者脚本安装皆可，后者更简单。

- [手动安装](https://github.com/jalaxy33/archlinux-tour/wiki/手动安装系统)
- [脚本安装](https://github.com/jalaxy33/archlinux-tour/wiki/用脚本安装系统)


## 重启前的工作

在安装完系统重启电脑之前，还需要完成一些必要步骤。

**请看这里👉**：[重启前的工作](https://github.com/jalaxy33/archlinux-tour/wiki/安装后重启前的工作)


