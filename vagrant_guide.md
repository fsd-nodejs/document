<!-- # Nodejs全栈开发环境使用引导 -->

## 基础环境+工具

下载地址[https://test-superip.aiplussales.cn/release/](https://test-superip.aiplussales.cn/release/)

阿里云加速地址 https://dev-vr-static.oss-cn-shenzhen.aliyuncs.com/box/nodejs_v1.0.13.zip

该虚拟机默认使用私钥登录，第一次启动成功后，私钥存放在当前目录的`.vagrant/machines/default/virtualbox/private_key`文件中

默认账号密码均为: `vagrant`

### Nginx

安装目录: `/usr/local/nginx`

默认开机启动

bind_ip: `0.0.0.0`

端口: `80`

可在虚拟机外访问 ，默认转发到主机的`8080`端口

默认通过127.0.0.1:8080访问

### Redis

安装目录: `/usr/local/redis`

默认开机启动

bind_ip: `0.0.0.0`

端口: `6379`

无密码

可在虚拟机外访问

### RabitMQ

默认端口: `5672`

默认用户: `guest`

默认密码: `guest`

默认管理页面端口: `15672`

使用yum 安装， 目录默认

### Mysql

安装目录: `/usr/local/mysql`

默认开机启动

bind_ip: `0.0.0.0`

端口: `6379`

默认用户:`root`

默认密码:`root`

可在虚拟机外访问

### Nodejs

默认是lts版本，使用nvm进行管理，在~/.nvm目录

### NPM

随nodejs安装的

### NVM

nodejs管理工具，支持nodejs多版本自由切换

### PM2

nodejs守护程序

### nodemon

调试开发的必备工具

### GIT

GIt ...



## Vagrant box指南

先下载定制的Nodejs Vagrant Box文件，解压后有如下文件。

```bash
.
├── README.md
├── Vagrantfile
├── metadata.json
├── private_key
└── virtualbox.box
```

 

在执行下面操作时，请确保你已经安装好 [VirtualBox](https://www.virtualbox.org/wiki/Downloads) 和 [Vagrant](https://www.vagrantup.com/)。

安装步骤非常简单，直接文件双击运行按照提示安装即可。接下来进入正式使用环节



命令行今日文件夹目录，Windows用户最好使用`git bash`



1.添加box文件到本机

```bash
$ vagrant box add metadata.json
```



2.修改`Vagrantfile`配置，目前有一些默认配置注释掉了。

```shell
.
.
. # 这里是配置端口转发，guest表示虚拟机内，host表示真实主机。如果有冲突请替换端口号后取消注释
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.network "forwarded_port", guest: 3306, host: 3306
  # config.vm.network "forwarded_port", guest: 6379, host: 6379
  # config.vm.network "forwarded_port", guest: 5672, host: 5672
  # config.vm.network "forwarded_port", guest: 15672, host: 15672
  # config.vm.network "forwarded_port", guest: 9999, host: 9999
.
.
.
  # 这里配置需要同步的目录，../表示真实主机的目标，/vagrant_data表示挂载在虚拟机内的目录
  # config.vm.synced_folder "../", "/vagrant_data"
.
.
.
   # 如果需要界面可以在这里取消该注释
	 # vb.gui = true

    # 这里是虚拟机性能配置，如果真实主机性能强，可酌情调整
    # vb.memory = "2048"
    # vb.cpus = 2
```



3.如果不修改上面的配置也是可以正常启动的，只是内部服务，外部无法访问

```
$ vagrant up
```



4. 进入虚拟机

```
$ vagrant ssh
```

## 常见问题

### ssh问题

- 办法1:

  如下操作，替换了~/.ssh/目录下文件再打包

  ```bash
  $ wget https://raw.githubusercontent.com/mitchellh/vagrant/master/keys/vagrant.pub -O .ssh/authorized_keys
  $ chmod 700 .ssh
  $ chmod 600 .ssh/authorized_keys
  $ chown -R vagrant:vagrant .ssh
  ```

  或者直接拷贝原打包目录.vagrant下的privacy key

- 办法2:

  将解压文件夹中的`private_key`拷贝到 `.vagrant/machines/default/virtualbox`目录下

  

### 共享文件夹出现问题
可能的错误提示:`mount: unknown filesystem type 'vboxsf'`

```bash
$ vagrant plugin install vagrant-vbguest
$ vagrant destroy && vagrant up
```

同步文件修改还要装插件

```bash
$ vagrant plugin install vagrant-winnfsd
```



### 打包的box文件过大压缩一下

[VirtualBox压缩vmdk、vagrant打包box一口气全对](https://www.zh30.com/virtualbox-vmdk-vagrant-box.html)

```bash

$ VBoxManage showhdinfo name

```

### 热更新

nodemon -L 

提高io性能，启用nfs或samba