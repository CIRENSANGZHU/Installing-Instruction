# Instruction-of-Installing-Ubuntu
Instruction of Installing Ubuntu 20.04.1 on a server

在服务器上安装ubuntu 20.04.1系统

部分内容参考自网络，

所写内容仅供参考，在我的安装过程中以下步骤可行

在不同的硬件上安装过程上可能会有不同的设置，不同的需求下设置可能也需要变化

## 1 准备

1.1 ubuntu镜像(.iso)文件，在网上下载

1.2 U盘(必须是空的或无用的，因为要格式化，里面的数据都会消失；最好用好一点的U盘，U盘如果有问题可能也装不上；最好大一点，U盘空间太小.iso文件刻录不上)

1.3 UltraISO软件，在网上下载

## 2 刻录.iso文件

2.1 插上U盘，打开UltralISO软件

2.2 UltraISO软件里选择“打开”找到ubuntu系统的.iso文件，选择“启动”-“写入硬盘映像”

2.3 确认刻录的设备是插入的U盘，并确认选择的.iso文件无误，先点击“格式化”将U盘格式化，格式化的设置默认即可

2.4 点击“写入”，将.iso文件写入U盘中，写入的设置默认即可

2.5 安全移除U盘

## 3 安装

3.1 将U盘插入服务器，开启服务器，开机过程中按F11进入开机选项

3.2 选择插入的U盘作为启动盘开机

3.3 按提示进行选择，可以先选择试用ubuntu但不安装，先看看

3.4 此时应该进入到ubuntu的图形化界面了，想安装了就双击桌面上的“Install Ubuntu 20.04.1 LTS”图标开始安装

3.5 一步一步进行选择即可，选择normal installation，建议选择全英文，安装的时候的错误信息中文可能显示不全，安装后可以再更改语言

3.6 选择something else自己进行分区

3.7 要将系统装在哪个磁盘里，就关注哪个磁盘的空间现状表和条状图，注意不要选错

3.8 在该磁盘的空间现状表里面，一个一个选择已有的分区然后删除，不要删别的盘里的东西

3.9 点击“+”号在该磁盘里新建分区，新建分区时，只用管"Size", "Use as", "Mount point"三项，其他的默认即可

3.10 新建分区如下：1G||ext4||boot, 900G||ext4||home, 500mb||efi, 40G||swap, 剩余空间||ext4||/

3.11 选择device for boot loader installation为要安装所在的整个磁盘（最后面不带数字编号的）

3.12 点击"install now"开始安装，按照提示继续

3.13 安装完成后，会根据提示自动或者按回车重启服务器，重启的时候按del键查看设置，在Boot选项卡里检查一下，没问题的话按esc退出

3.14 顺利的话，应该成功进入ubuntu系统的图形界面

## 4 挂载硬盘

4.1 说明：挂载服务器连接的其他硬盘，如果这些硬盘里有要用的数据，下面的操作过程中就不要进行格式化，否则可以格式化后挂载

4.2 修改/etc fstab文件进行永久挂载（开机自动挂载），网上可以查到步骤，先查硬盘的信息(lsblk, blkid等命令)，然后修改文件，修改的时候最后两个数字都写0（不知道写别的会咋样）

4.3 重启一下试试，验证有没有永久挂载（开机自动挂载上）

## 5 安装显卡驱动

5.1 查服务器上的显卡型号

5.2 在nvidia官网上输入显卡型号，查对应的显卡驱动，在服务器上下载好.run文件，中英文的都行

5.3 在安装nvidia显卡驱动之前，先禁用nouveau，步骤可在网上查到，安装完显卡驱动之后也不用解除禁用（下面所有的命令，不行的话试试加上sudo在最前面）

5.4 打开系统的blacklist：sudo vim /etc/modprobe.d/blacklist-nouveau.conf

5.5 添加禁用语句：第一行，blacklist nouveau；第二行，options nouvear modeset=0

5.6 更新initramfs：sudo update-initramfs -u

5.7 重启系统：sudo reboot

5.8 检查是否成功禁用nouveau：lsmod | grep nouveau，若没有显示内容，则成功禁用

5.9 sudo init 3关闭图形界面

5.10 chmod 777 [显卡驱动.run文件]

5.11 cd到显卡驱动.run文件所在目录

5.12 sudo apt install build-essential

5.13 sudo [显卡驱动.run文件] --no-opengl-files，安装过程中按提示选择，一般默认就可以

5.14 init 5打开图形界面

5.15 打开一个终端，运行nvidia-smi，若显示相关信息则安装成功

## 6 网卡配置

6.1 说明：如果对服务器网络的IP地址有固定要求，则需要进行网卡配置

6.2 通过图形界面依次选择Settings-Network，点击齿轮状的设置按钮进行设置，一般对IPv4进行设置

6.3 在IPv4选项卡里，IPv4 Method选择Manual就可以进行手动配置

6.4 Addresses大项里，在Address里输IP地址，Netmask里输子网掩码，Gateway里输网关；在DNS里输DNS，如果有备用DNS的话用逗号隔开

6.5 点击Apply

6.6 重启

6.7 检查：终端中ping网关的IP，看看通不通，通的话应该就没问题

## 7 安装SSH

7.1 说明：为了让服务器能够被远程登陆访问，安装SSH

7.2 sudo apt install ssh，按提示进行

7.3 检查：看看服务器能否被远程登陆，如果不行的话尝试重启服务器、等一段时间再试试




