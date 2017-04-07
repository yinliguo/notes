# vagrant


### 1、what is vagrant？ ###
vagrant是一款使用虚拟机统一开发环境的虚拟机管理工具。

### 2、示例 ###

> 本示例中的主机操作系统是win7，vagrant是支持OS X和Linux的。
> 本示例中的虚拟机为virtualbox，vagrant是支持vmware和AWS的。

 1. **安装virtualbox**
 
	到virtualbox的官网下载对应自己主机操作系统的安装包并安装。

 2. **安装虚拟机操作系统**
 
	本示例使用了CentOS 5.11作为开发环境的操作系统。注意网络采用NAT。
		
 3. **在virtual box中搭建开发环境**

	本示例使用了nginx + php，将这些软件安装到CentOS中，并保证在虚拟机中能运行程序。
		
	本示例中将nginx的root目录配置为`/www`
		
	**@important**   
	**安装VBoxGuestAdditions**  
	*这是virtualbox的增强功能，用于虚拟机访问主机的共享目录。*  
	（1）在centos窗口的菜单中，点击 设备-->安装增强功能  
	（2）`mount /dev/cdrom /mnt`  
	（3）`./VBoxLinuxAdditions.run`  
	
	**@important**  
	**配置vagrant用户**  
	vagrant默认使用vagrant用户，需要在centos中建立这个用户，并设置密码为vagrant  
	（1）`groupadd vagrant;useradd -g vagrant vagrant`  
    	（2）`passwd vagrant`    连续输入两次密码"vagrant"  
    	（3）`visudo` 打开一个文件修改如下两处：  
	     A.在`root ALL=(ALL) ALL`下方添加`vagrant ALL=(ALL) NOPASSWD: ALL`;  
	     B.注释掉`Defaults requiretty`  
	（4）`wget https://raw.githubusercontent.com/mitchellh/vagrant/master/keys/vagrant.pub`  
	（5）`mv ./vagrant.pub /home/vagrant/.ssh/authorized_keys`  
	（6）`chown vagrant:vagrant -R /home/vagrant/.ssh`  
	（7）`chmod 0700 /home/vagrant/.ssh`  
	（8）`chmod 0600 /home/vagrant/.ssh/authorized_keys`  
	
	**@important** 
	**禁用防火墙**  
	`chkconfig --level 35 iptables off`  
	
 4. **安装vagrant**
 
	到vagrant官网下载并安装到主机，此时关闭虚拟机。		 
		 
 5. **打包虚拟机开发环境为box文件**
 
	（1）打开DOS窗口，进入centos所在的目录	
	（2）`vagrant package --output centos1.box --base centos`此时会生成一个centos1.box的文件，这个文件就是将虚拟机中的开发环境打包的结果，把它发给其他的开发人员，别人便可利用它在自己的机器上还原开发环境。
	
 6. **使用box文件还原开发环境**
 
 比如某个开发人员拿到了centos1.box，他可以按如下步骤来快速搭建开发环境。  
 （1）安装vagrant和virtualbox  
 （2）进入centos1.box所在的目录  
 （3）`vagrant box add centos1.box --name centos1`将centos还原到virtualbox里  
 （4）`vagrant init centos1`生成Vagrantfile，这是这个centos的vagrant配置文件  
 （5）编辑Vagrantfile，将`config.vm.network "private_network", ip: "192.168.33.10"`这一行的注释去掉  
 （6）将`config.vm.synced_folder`的注释去掉，并将后面的第一个参数路径改为自己的代码所在的位置，第二个参数修改为centos中配置的项目路径，本示例中就是上面提到的`/www`  
 （7）配置完成保存，并在当前目录运行`vagrant up`，启动完成后，即可在主机中使用192.168.33.10访问centos中的项目

 7. **访问虚拟机**

 Linux和OS X下可以使用`vagrant ssh`登录虚拟机终端  
 windows下则要通过SSH连接工具，如SecureCRT、Xshell等工具使用vagrant用户登录，采用ssh私钥免密码验证登录，授权文件为`C:\users\$user\.vagrant.d\insecure_private_key`


-----------
FAQ：

 1. vagrant和virtutalbox到底是什么关系？
 
 vagrant是虚拟机的管理工具，能够创建、配置、删除虚拟机，其实vagrant能做到的，虚拟机自己也能做到，但是vagrant作为工具让虚拟机的管理更方便了。
 
 2. vagrant有哪些常用命令？
 
 `vagrant up`--启动虚拟机  
 `vagrant halt`--关闭虚拟机  
 `vagrant reload`--重启虚拟机  
 `vagrant destroy`--删除虚拟机
 
 3. 主机如何访问虚拟机？
 
 在Vagantfile文件中提供了三种主机访问虚拟机的方式：  
 **（1）端口转发**  
 就是将主机特定端口的消息转发到虚拟机中的端口上，比如配置`config.vm.network "forwarded_port", guest:80, host:8080`，那么在主机浏览器中打开`http://localhost:8080`时就会转发到虚拟机的80端口，相当于在虚拟机中访问`http://localhost:80`  
 **（2）Host-only**  
 搭建一个私有网络，主机和虚拟机可以互访，比如配置  
 `config.vm.network "private_network", ip: "192.168.33.10"`在主机中就可以使用这个地址访问虚拟机了  
 **（3）桥接**  
 使用桥接的方式，使虚拟机成为跟主机同一网络的机器，去掉下方的代码的注释即可  
 `config.vm.network "public_network"`

	注意：三种方式不可同时配置！

 4. 无法安装VBoxGuestAdditions，提示“building the main Guest Additions module  [failed]”
   需要安装gcc和gcc-c++，再安装kernel-devel，红帽系列使用yum -y install gcc gcc-c++ kernel kernel-devel kernel-headers，debian系列使用sudo apt-get install gcc gcc-c++ kernel kernel-devel kernel-headers，然后重启就可以正常安装VBoxGuestAdditions了

 5. 导入box文件，启动虚拟机时，出现无法启动网卡的问题  
  删除/etc/udev/rules.d/70-persistent-net.rules，然后再vagrant reload即可

 6. 挂载共享文件夹后的访问权限问题  
 
  可以通过设置Vagrantfile文件的config.vm.synced_folder的属性，比如  `config.vm.synced_folder "/Users/abc/myproject", "/www", :mount_options => ["dmode=777", "fmode=777"]`

 7. 如何在原来的基础上打包？  
 
  在Vagrantfile文件所在的目录下使用`vagrant package`则会把新增加的内容打包

 8. Vagrant was unable to mount VirtualBox shared folders. This is usuallybecause the filesystem "vboxsf" is not available. This filesystem ismade available via the VirtualBox Guest Additions and kernel module.Please verify that these guest additions are properly installed in theguest. This is not a bug in Vagrant and is usually caused by a faultyVagrant box.
 
  使用命令`vagrant plugin install vagrant-vbguest`
  
 9. 安装VBoxGuestAdditions时出现Could not find the X.Org or XFree86 Window System
```
yum -y install xorg-x11-drivers xorg-x11-utils
```
 10. 启动时出现Warning: Authentication failure. Retrying  
 可以在Vagrantfile里加上
 ```
 config.ssh.username="vagrant"
 config.ssh.password="vagrant"
 ```
