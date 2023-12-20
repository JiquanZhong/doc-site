---
sidebar_position: 2
---

# Linux常用命令

## 系统管理服务

systemctl

查看系统所有服务

```shell
systemctl list-units --type=service
```

查看服务运行状态

```shel
systemctl status firewalld
```

关闭服务

```shell
systemctl stop firewalld
```

启动服务

```shell
systemctl start firewalld
```

重启服务

```shell
systemctl reload firewalld
```

禁止服务开机启动

```shell
systemctl disable firewalld
```

设置服务开机启动

```shell
systemctl enable firewalld
```

## 文件管理

列出指定目录/下所有文件

```shell
ls -l /
```

当前目录绝对路径

```shell
pwd
```

改变当前路径到/jiquan/desktop目录

```shell
cd /jiquan/desktop
```

显示系统的时间（按照指定格式）

```shell
date '+%Y-%m-%d %H-%M-%S'
```

清除屏幕信息

```shell
clear
```

获取指定目录的帮助信息

```shell
man ls
```

查询系统处于什么运行级别

```shell
who -r
```

显示目前登陆到系统的用户

```shell
who -buT
```

显示系统的内存状态（单位M）

```shell
free -m
```

显示系统进程运行状态

```shell
ps -ef
```

查看sshd进程的运行状态

```shell
ps -ef | grep sshd
```

查看即时的活跃任务

```shell
top
```

当前目录下创建目录abc

```shell
mkdir abc
```

每页十行查看boot.log文件

```shell
more -c -10 /var/log/boot.log
```

查看boot.log并标注行号

```shell
cat -Ab /var/log/boot.log
```

创建text.txt文件

```shell
touch text.txt
```

强制删除某个目录及其子目录

```shell
rm -rf testdir/
```

拷贝test1目录到test2

```shell
cp -r /mydata/test1 /mydata/test2
```

移动或覆盖文件

```shell
mv text.txt text1.txt
```

## 压缩与解压

将etc文件夹下的文件归档到etc.tar（不压缩）

```shell
tar -cvf /mydata/etc.tar /etc
```

用gzip把etc下的文件压缩为etc.tar.gz

```shell
tar -zcvf /mydata/etc.tar.gz /etc
```

用bzip2压缩文件夹/etc到文件/etc.tar.bz2

```shell
tar -jcvf /mydata/etc.tar.bz2 /etc
```

分页查看压缩包装的内容

```shell
tar -ztvf /mydata/etc.tar.gz | more -c -10
```

解压文件到当前目录

```shell
tar -zxvf /mydata/etc.tar.gz
```

解压文件到指定目录

```shell
tar -zxvf /mydata/etc.tar.gz -C /mydata/etc
```

## 磁盘和网络管理

查看磁盘使用情况

```shell
df -hT
```

查看当前目录下的文件及文件夹所占大小

```shell
du -h --max-depth=1 ./*
```

显示当前网络接口状态

```shell
ifconfig
```

显示当前路由信息

```shell
netstat -rn
```

查看所有有效的tcp连接

```shell
netstat -an
```

查看系统中启动的监听服务

```shell
netstat -tulnp
```

查看处于连接状态的系统资源信息

```shell
netstat -atunp
```

## 文件的上传和下载

网络上下载文件

```shell
wget url
```

安装上传下载软件lrzsz

```shell
yum install -y lrzsz
```

弹出上传文件框

```shell
rz
```

弹出保存文件框

```shell
sz
```

rpm是`Red-Hat Package Manager`的缩写，一种Linux下通用的软件包管理方式，可用于安装和管理`.rpm`结尾的软件包。

```shell
rpm -ivh nginx-1.12.2-1.el7_4.ngx.x86_64.rpm
```

模糊搜索软件包

```shell
rpm -qa | grep nginx
```

精确查找软件包

```shell
rpm -qa nginx
```

查看软件包的概要信息

```shell
rpm -qi nginx-1.12.2-1.el7_4.ngx.x86_64
```

查询软件包的安装路径

```shell
rpm -ql nginx-1.12.2-1.el7_4.ngx.x86_64
```

验证软件包内容和安装文件是否一致

```shell
rpm -V nginx-1.12.2-1.el7_4.ngx.x86_64
```

更新软件包

```shell
rpm -Uvh nginx-1.12.2-1.el7_4.ngx.x86_64
```

删除软件包

```shell
rpm -e nginx-1.12.2-1.el7_4.ngx.x86_64
```

Yum是`Yellow dog Updater, Modified`的缩写，能够在线自动下载RPM包并安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，非常方便！

安装一个epel的安装源

```shell
yum install epel-release
```

安装软件包

```shell
yum install nginx
```

检查可以更新的软件包

```shell
yum check-update
```

更新指定的软件包

```shell
yum update nginx
```

在资源库中查找软件包信息

```shell
yum info nginx*
```

列出已经安装的所有软件包

```shell
yum info installed
```

模糊搜索软件包

```shell
yum search nginx
```

卸载软件包

```shell
yum remove nginx
```

## 用户管理

查看用户信息

```shell
cat /etc/passwd
```

查看用户组信息

```shell
cat /etc/group
```

passwd

设置用户密码

```shell
passwd root
```

改变用户身份

```shell
# 切换到root用户
su -
# 切换到macro用户
su jiquan
```

添加用户组，使用`-g`可以设置用户组的标志号

```shell
groupadd -g 1024 admin
```

删除用户组

```shell
groupdel admin
```

添加用户，`-u`设置标志号，`-g`设置主用户组

```shell
useradd -u 1024 -g admin jiquan
```

删除用户，使用`-r`可以删除用户主目录

```shell
userdel jiquan -r
```



