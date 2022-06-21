---
layout: post
---

## 前言

根据实际需求，将ubuntu系统配置为远程服务器供多人使用。

操作系统：Ubuntu 16.04

本文主要介绍四个内容：

* 在ubuntu上创建多个用户
* 配置ssh服务器（供用户进行ssh远程连接）
* 配置vncserver （供用户进行远程桌面访问）
* 配置ftp服务器 （供用户进行ftp上传与下载文件）

## 创建用户

**1. 创建新用户：**

```bash
sudo useradd -m -s /bin/bash -d /home/username username
```

**2. 添加管理员权限**

```bash
sudo vim /etc/sudoers
```

添加内容如下：

```bash
username ALL=(ALL:ALL) ALL
```

**3. 设置密码**

输入：

```bash
sudo passwd username
```

修改密码。

## ssh服务器

**1. 配置端口映射**

ssh服务器默认使用`22`号端口，因为这里我用了一个路由器，因此在路由器的`虚拟服务器`功能里面添加了一个端口映射，将内部的`22`号端口映射为`5002`号端口。

<div class="fig figcenter">
    <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC0xZTVkYmRjNTNkMmM4MmY3LnBuZw?x-oss-process=image/format,png#pic_center" width=600px>
    <div class="figcaption" style="text-align:left">虚拟服务器端口映射，这里的ip地址填写的是路由器给服务器分配的ip地址（局域网），如果申请连接的客户端没有连接到该路由器上，ssh连接时要写路由器上一层的ip地址</div>
</div>

**2. 配置ssh服务器**

* 安装openssh-server

    ```bash
    sudo apt-get update
    sudo apt-get install openssh-server
    ```

    打开”终端窗口”，输入`sudo ps -e |grep ssh`–>回车–>有sshd,说明ssh服务已经启动。如果没有启动，输入`sudo service ssh start`–>回车–>ssh服务就会启动

* 进入到`/etc/ssh/sshd_config`修改配置文件

    将PermitRootLogin 设置为yes，如下图所示：

<div class="fig figcenter">
    <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC1kZTYwMGQ3OTBiZTUyOWEyLnBuZw?x-oss-process=image/format,png#pic_center" width=600px>
    <div class="figcaption">PermitRootLogin设置为yes</div>
</div>

<div class="fig figcenter">
    <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC1hMWEzMWQ4ZDMyYjBiMWQ1LnBuZw?x-oss-process=image/format,png#pic_center" width=600px>
    <div class="figcaption">这里增加了最后两行，使得ssh服务器每隔一段时间和客户端通信一次，防止ssh超时断开</div>
</div>


* 重启ssh服务器

    ```bash
    sudo service ssh restart
    ```

* 完成

    通过`ssh username@ip -port 5002`即可进入（这里将内部22号端口映射到5002号端口）

## vncserver

* 安装vnc4server

```bash
sudo apt-get install vnc4server
```

输入`vncserver`启动服务，初次启动需要设置密码，如下：

<div class="fig figcenter">
    <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC03MGMxYzliZDRkNTkxMDE3LnBuZw?x-oss-process=image/format,png">
    <div class="figcaption">vncserver</div>
</div>


* 编辑生成的xstartup文件，如下：

```sh
#!/bin/sh

# Uncomment the following two lines for normal desktop:
#如果出现灰屏鼠标变X的情况将下面两行解注释即可，建议解开
# unset SESSION_MANAGER
# exec /etc/X11/xinit/xinitrc

[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
vncconfig -iconic &
#x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
#x-window-manager &
#下面有两种界面的选择，建议xfce4，因为问题少。
#读者根据需要选择其一，并删除汉字（我也不知道不删除会不会有影响）
#以下是xfce4所需的配置
sesion-manager & xfdesktop & xfce4-panel &
xfce4-menu-plugin &
xfsettingsd &
xfconfd &
xfwm4 &
#以下是gnome所需的一些配置
#dbus-launch gnome-panel &
#dbus-launch gnome-settings-daemon &
#metacity &
#nautilus &
#dbus-launch gnome-terminal &
```

> 在图形界面中，一般有`gnome`和`xfc4`，试了一次gnome发现会有些问题，xfc4问题较少，因此也就不折腾了，上面介绍的是xfc4的配置，爱折腾的朋友可以考虑gnome的界面，这里就不在折腾了，实用为主。
> * 如果选择xfce4，则安装如下软件（建议选这个）：
> ```bash
> sudo apt-get install xfce4
> ```
> * 如果选择gnome，则安装对应组件
> ```bash
> sudo apt-get install gnome-panel gnome-settings-daemon metacity nautilus gnome-terminal
> ```

* 完成，打开ubuntu自带的Remmina，输入ip:port，协议选择vnc，即可登录。

<div class="fig figcenter">
    <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC04N2YxNWIyMTAxYWQ3N2VlLnBuZw?x-oss-process=image/format,png#pic_center" width=600px>
    <div class="figcaption">vnc</div>
</div>

> 注意：实际操作时我们发现xfce4打开的vncviewer终端下TAB键不能自动联想，一种最简单的处理方法如下：选择Applications->Settings->Window Manager->Keyboard->Switch window for same application->Clear
即清除掉某个特定的快捷键即可，如下图：

<div class="fig figcenter">
    <img src="https://img-blog.csdnimg.cn/img_convert/dda0153b4633a5d489e984148ace78c3.png#pic_center">
    <div class="figcaption">settings</div>
</div>

<div class="fig figcenter">
    <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC0xZTg0Y2Y1ODVmNDA1OGFkLnBuZw?x-oss-process=image/format,png#pic_center" width=600px>
    <div class="figcaption">window manager</div>
</div>

> 注意2：如果发现图形界面没有Window Manager，可以按照下面的操作（但较为麻烦，且需要重启电脑）：
>
> 1 编辑下面文件：
>
>```
>~/.config/xfce4/xfconf/xfce-perchannel-xml/xfce4-keyboard-shortcuts.xml
>```
>
> 2 找到如下属性：
>
> ```
> <property name="&lt;Super&gt;Tab" type="string" value="switch_window_key"/>
> ```
>
> 3 修改如下（**注意别修改错了**）：
>
> ```
> <property name="&lt;Super&gt;Tab" type="empty"/>
> ```
>
> 4 重启电脑


>注意3: 可通过下面的命令修改终端样式：
>
>```
>sudo update-alternatives --config x-terminal-emulator
>```
>
>然后输入对应数字选择Terminal类型



## ftp服务器

### 服务器端配置

* 安装vsftpd

```bash
sudo apt-get install vsftpd
```

* 修改`/etc/vsftpd.conf`.

```sh
# Example config file /etc/vsftpd.conf
#
# The default compiled in settings are fairly paranoid. This sample file
# loosens things up a bit, to make the ftp daemon more usable.
# Please see vsftpd.conf.5 for all compiled in defaults.
#
# READ THIS: This example file is NOT an exhaustive list of vsftpd options.
# Please read the vsftpd.conf.5 manual page to get a full idea of vsftpd's
# capabilities.
#
#
# Run standalone?  vsftpd can run either from an inetd or as a standalone
# daemon started from an initscript.
listen=NO
#
# This directive enables listening on IPv6 sockets. By default, listening
# on the IPv6 "any" address (::) will accept connections from both IPv6
# and IPv4 clients. It is not necessary to listen on *both* IPv4 and IPv6
# sockets. If you want that (perhaps because you want to listen on specific
# addresses) then you must run two copies of vsftpd with two configuration
# files.
listen_ipv6=YES
#
# Allow anonymous FTP? (Disabled by default).
anonymous_enable=NO
#
# Uncomment this to allow local users to log in.
local_enable=YES
#
# Uncomment this to enable any form of FTP write command.
write_enable=YES
#
# Default umask for local users is 077. You may wish to change this to 022,
# if your users expect that (022 is used by most other ftpd's)
#local_umask=022
#
# Uncomment this to allow the anonymous FTP user to upload files. This only
# has an effect if the above global write enable is activated. Also, you will
# obviously need to create a directory writable by the FTP user.
#anon_upload_enable=YES
#
# Uncomment this if you want the anonymous FTP user to be able to create
# new directories.
#anon_mkdir_write_enable=YES
#
# Activate directory messages - messages given to remote users when they
# go into a certain directory.
dirmessage_enable=YES
#
# If enabled, vsftpd will display directory listings with the time
# in  your  local  time  zone.  The default is to display GMT. The
# times returned by the MDTM FTP command are also affected by this
# option.
use_localtime=YES
#
# Activate logging of uploads/downloads.
xferlog_enable=YES
#
# Make sure PORT transfer connections originate from port 20 (ftp-data).
connect_from_port_20=YES
#
# If you want, you can arrange for uploaded anonymous files to be owned by
# a different user. Note! Using "root" for uploaded files is not
# recommended!
#chown_uploads=YES
#chown_username=whoever
#
# You may override where the log file goes if you like. The default is shown
# below.
#xferlog_file=/var/log/vsftpd.log
#
# If you want, you can have your log file in standard ftpd xferlog format.
# Note that the default log file location is /var/log/xferlog in this case.
#xferlog_std_format=YES
#
# You may change the default value for timing out an idle session.
#idle_session_timeout=600
#
# You may change the default value for timing out a data connection.
#data_connection_timeout=120
#
# It is recommended that you define on your system a unique user which the
# ftp server can use as a totally isolated and unprivileged user.
#nopriv_user=ftpsecure
#
# Enable this and the server will recognise asynchronous ABOR requests. Not
# recommended for security (the code is non-trivial). Not enabling it,
# however, may confuse older FTP clients.
#async_abor_enable=YES
#
# By default the server will pretend to allow ASCII mode but in fact ignore
# the request. Turn on the below options to have the server actually do ASCII
# mangling on files when in ASCII mode.
# Beware that on some FTP servers, ASCII support allows a denial of service
# attack (DoS) via the command "SIZE /big/file" in ASCII mode. vsftpd
# predicted this attack and has always been safe, reporting the size of the
# raw file.
# ASCII mangling is a horrible feature of the protocol.
#ascii_upload_enable=YES
#ascii_download_enable=YES
#
# You may fully customise the login banner string:
#ftpd_banner=Welcome to blah FTP service.
#
# You may specify a file of disallowed anonymous e-mail addresses. Apparently
# useful for combatting certain DoS attacks.
#deny_email_enable=YES
# (default follows)
#banned_email_file=/etc/vsftpd.banned_emails
#
# You may restrict local users to their home directories.  See the FAQ for
# the possible risks in this before using chroot_local_user or
# chroot_list_enable below.
#chroot_local_user=YES
#
# You may specify an explicit list of local users to chroot() to their home
# directory. If chroot_local_user is YES, then this list becomes a list of
# users to NOT chroot().
# (Warning! chroot'ing can be very dangerous. If using chroot, make sure that
# the user does not have write access to the top level directory within the
# chroot)
#chroot_local_user=YES
#chroot_list_enable=YES
# (default follows)
#chroot_list_file=/etc/vsftpd.chroot_list
#
# You may activate the "-R" option to the builtin ls. This is disabled by
# default to avoid remote users being able to cause excessive I/O on large
# sites. However, some broken FTP clients such as "ncftp" and "mirror" assume
# the presence of the "-R" option, so there is a strong case for enabling it.
#ls_recurse_enable=YES
#
# Customization
#
# Some of vsftpd's settings don't fit the filesystem layout by
# default.
#
# This option should be the name of a directory which is empty.  Also, the
# directory should not be writable by the ftp user. This directory is used
# as a secure chroot() jail at times vsftpd does not require filesystem
# access.
secure_chroot_dir=/var/run/vsftpd/empty
#
# This string is the name of the PAM service vsftpd will use.
pam_service_name=vsftpd
#
# This option specifies the location of the RSA certificate to use for SSL
# encrypted connections.
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
ssl_enable=NO

#
# Uncomment this to indicate that vsftpd use a utf8 filesystem.
#utf8_filesystem=YES
local_root=/home/username
```

**部分参数说明:**

> - anonymous_enable=NO 拒绝匿名登陆
> - write_enable=YES 设置可以上传文件，这个设置看需要个人需要
> - xferlog_enable=YES 开启日志记录
> - xferlog_file=/var/log/vsftpd.log 设置日志文件路径
> - xferlog_std_format=YES 设置日志格式为标准输出
> - connect_from_port_20=YES 绑定20端口
> - ftpd_banner=Welcome to FTP service. 欢迎语句，在使用shell时可以看到
> - chroot_local_user=YES
> - chroot_list_enable=YES
> - chroot_list_file=/etc/vsftpd.chroot_list 
> 上面的这几个配置实现的功能是：用户被限制在自己的主目录下。用户名单来源于/etc/vsftpd.chroot_list
> - user_sub_token=$USER
> - local_root=/home/$USER/ftp
>添加一个user_sub_token ，以便插入用户名在我们local_root directory路径，以便我们的配置将为该用户，并可能被添加任何未来的用户
> - pam_service_name=ftp原配置中为vsftpd，ubuntu用户需要更改成ftp

* 开启或者重启vsftpd

**开启:** `service vsftpd start`

**重启:** `service vsftpd restart`

### 客户端配置

- 下载filezilla

```bash
sudo apt-get install filezilla
```

- 配置用户信息

    打开filezilla，点击左上角site manager， 输入ip，端口，用户名等信息。Protocol 选择SFTP

<div class="fig figcenter">
    <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC0yZDBjMGEzYmJiZWMzNWM4LnBuZw?x-oss-process=image/format,png#pic_center" width=600px>
    <div class="figcaption">filezilla菜单栏，左上角为site manager按钮</div>
</div>


<div class="fig figcenter">
    <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC03YWJiY2FjNzNiY2MxOGZjLnBuZw?x-oss-process=image/format,png#pic_center" width=600px>
    <div class="figcaption">通过site manager设置要连接的服务器</div>
</div>


<div class="fig figcenter">
    <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC1jYjEyMTA4NzgyYTc1ZGVlLnBuZw?x-oss-process=image/format,png#pic_center" width=600px>
    <div class="figcaption">filezilla界面，左侧为本地文件，右侧为服务器文件</div>
</div>


## 总结

本文主要介绍了如何在ubuntu16.04上添加新用户，并配置ssh，vncviewer以及ftp服务器。

## 参考资料

- [ubuntu 16.04 搭建ftp服务器](https://blog.csdn.net/lj402159806/article/details/78209103)
- [Ubuntu添加新用户，并创建管理员权限](https://blog.csdn.net/u013431916/article/details/82503137)
- [ubuntu系统下配置VNC服务操作步骤（VNCserver）](https://blog.csdn.net/frank_ldw/article/details/84819343)
- [Ubuntu如何安装vncserve](https://jingyan.baidu.com/article/cbcede077f59bf02f40b4ddb.html)
- [Ubuntu 14.04远程登录服务器--ssh的安装和配置](https://jingyan.baidu.com/article/9c69d48fb9fd7b13c8024e6b.html)
