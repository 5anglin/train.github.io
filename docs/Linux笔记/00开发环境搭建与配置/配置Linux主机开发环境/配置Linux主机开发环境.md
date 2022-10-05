[toc]

# 搭建开发环境


1. 安装Linux系统(本地服务器/Vmware/docker/云服务器)：
2. SSHD服务 ([Putty](http://www.putty.be/)/[MobaXterm](https://mobaxterm.mobatek.net/)/[SecureCRT](https://www.vandyke.com/products/securecrt/)/[Xshell](https://www.netsarang.com/zh/xshell/)): windows --> Linux
	```bash
	sudo apt-get install openssh-server
	```
3. SAMBA服务: Linux --> windows 文件共享 TCP 445/139
4. NFS（Network FileSystem）: Linux --> Linux
5. FTP : windows --> Linux 文件传输(使用SAMBA可不安装FTP) SFTP/FTP客户端([FileZilla](https://filezilla-project.org/)/[WinSCP](https://winscp.net/eng/index.php)/[SecureFX](https://www.vandyke.com/cgi-bin/releases.php?product=securefx))

    文件传输方式：
   
   + 串口传输
   + 网络传输
   + 移动存储传输
   + 网络文件系统

+ [NVC](https://www.realvnc.com/en/)(Virtual Network Console) 是虚拟网络控制台 以图形界面访问
+ Mstsc（Microsoft Terminal Service Client）创建与终端服务器或其他远程计算机的连接
+ minicom(linux端串口工具)

## 一、NFS（Network FileSystem）

## 1. ubuntu服务器端搭建NFS服务
1. 安装NFS服务：  
	```bash
	sudo apt-get install nfs-kernel-server
	```
2. 修改配置文件：
    ```bash
   mkdir ~/nfs_root
   sudo vim /etc/exports
   ```
3. 添加：
	```bash
	/home/ubuntu/nfs_root *(rw,nohide,insecure,no_subtree_check,async,no_root_squash)
	```
	/home/ubuntu/nfs_root : NFS服务的根目录，即NFS客户端挂载目录

    | 参数| 说明
    -----|-----
    * | 允许所有网段访问
    ro | 挂载此目录的客户端对该目录具有只读权限
    rw                           | 挂载此目录的客户端对该目录具有读写权限
    async        	           | 优先将数据保存到内存，然后再写入硬盘；这样效率更高，但可能会丢失数据
    sync                        | 数据同步写入内存和硬盘中,保证不丢失数据
    root_squash            |当NFS客户端以root管理员访问时，映射为NFS服务器的匿名用户
    no_root_squash      | 当NFS客户端以root管理员访问时，映射为NFS服务器的root管理员
    all_squash              | 无论NFS客户端使用什么账户访问，均映射为NFS服务器的匿名用户
    no_subtree_check  | 不检查父目录的权限
4. 重启服务：
	```bash
	sudo service nfs-kernel-server restart
	```
5. 服务状态：
	```bash
	sudo service nfs-kernel-server status
	```
6. 查看挂载目录
	```bash
	showmount 
	```
	| 参数| 说明
	---- | -----
	  -e | 显示NFS服务器的共享列表
	  -a | 显示本机挂载的文件资源的情况NFS资源的情况
	  -v | 显示版本号

### 2. 客户端挂载NFS共享目录
```bash
mount -t nfs -o nolock,vers=3 193.112.167.248:/home/ubuntu/nfs_root /mnt*
```
  项目     | Value
-------- | -----
  mount |挂载命令
  nfs |使用的协议
  nolock |不阻塞
  vers|使用的NFS版本号
  IP|NFS服务器的IP（NFS服务器运行在哪个系统上，就是哪个系统的IP）
  /home/ubuntu/nfs_root| 要挂载的目录（服务器的目录）
  /mnt | 要挂载到的目录（客户端的目录，注意挂载成功后，/mnt下原有数据将会被隐藏，无法找到）

## 二、FTP/TFTP (File Transfer Protocol)/(Trivial File Transfer Protocol)

## 服务端
1. 安装TFTP服务器
	```bash
	sudo apt-get install tftp-hpa tftpd-hpa
	```
	
2. 创建TFTP服务目录
	```bash
	mkdir -p /home/ubuntu/tftpboot
	sudo chmod 777 /home/ubuntu/tftpboot
	```
	
3. 配置TFTP服务文件
	```bash
	sudo vim /etc/default/tftpd-hpa
	```
	
 4. 添加：
	```bash
	TFTP_DIRECTORY="/home/ubuntu/tftpboot"
	TFTP_OPTIONS="-l -c -s"
	```
	
4. 重启：
	```bash
	sudo service tftpd-hpa restart
	```
	
6. 查看TFTP服务是否在运行：
	```bash
	ps -aux | grep "tftp"
	```
附：windows TFTP服务[tftpd64](http://tftpd32.jounin.net/)
tftp命令上传与下载
在开发板上使用 tftp 命令下载文件
   
	```bash
    tftp g r zImage 192.168.1.123
	```

   在开发板上使用 tftp 命令上传文件
	```bash
    tftp p l 1.txt 192.168.1.123
	```


## 三、SAMBA 

## 服务端
1. 安装
	```bash
	sudo apt-get install samba
	```

2. 配置(备份配置文件)
	```bash
	cp /ect/smaba/samba.conf /ect/smaba/back_samba.conf
	vim /ect/smaba/samba.conf
	```

3. 添加：
	```bash
	[samba_root]
   	    comment = samba_share
   	    path = /home/用户名/samba_root
        writable = yes
        browseable = yes
	```
4. 用户权限
	```bash
	smbpassd -a <用户名>
    	设置密码
	```
5. 重启服务：
	```bash
	smbd restart
	nmbd restart
	```


## 四、Vim配置

### 1. 源代码阅读插件安装配置
1. Taglist
    Taglist 是 Vim 的一个 源码浏览插件，可从 http://www.vim.org 网站获得。
下载到压缩包后，在本地解压，然后将解压得到目录中的plugin目录复制到vim目录。
如果用户主目录下没有 .vim 目录，则建立一个这样的目录即可 。
    
	1. 打开vim，导入taglist的帮助文件
		```bash
		:helptags ~/.vim/doc
		```
	2. /.vimrc中添加
	
		```bash
		"显示行号
		set number
		
		"语法高亮
		set syntax=on
		
		"代码补全
		set completeopt=preview,menu
		
		"自动缩进
		set autoindent
		set cindent
		
		"Tab宽度
		set tabstop=4
		
		"统一缩进为4
		set softtabstop=4
		set shiftwidth=4
		
		"编码设值
		set enc=utf-8
		
		"语言设置
		set langmenu=zh_CN.UTF-8
		set helplang=cn	
		
		"不同时显示多个文件的tag，只显示当前文件
		let Tlist_Show_One_File=1
		
		"如果taglist窗口是最后一个窗口，则退出vim
		let Tlist_Exit_OnlyWindow=1
		
		"将taglist与ctags关联
		let Tlist_Ctags_Cmd="/usr/bin/ctags"
		
		"alway use mouse
		set mouse=a
		
		"启动vim后自动打开taglist窗口
		let Tlist_Auto_Open = 1
	    
		"taglist窗口显示在右侧，缺省为左侧
		let Tlist_Use_Right_Window =1
	 
		"设置taglist窗口大小
		"let Tlist_WinHeight = 100
		let Tlist_WinWidth = 40
	 
		"设置taglist打开关闭的快捷键F8
		noremap <F8> :TlistToggle<CR>
	 
		"更新ctags标签文件快捷键设置
		noremap <F6> :!ctags -R<CR>
		```
2. Ctags
    Ctags 是一个用于产生 tags 文件的软件，可以下载源码进行编译安装，在 Ubuntu 下，
可通过 apt-get 进行安装：
	
	```bash
	sudo apt-get install exuberant ctags
	```
### 2. 源代码阅读插件使用	
1. 源码阅读和跟踪,进入源码目录，执行`ctargs -R`命令生成tags文件，如果修改源码需重新生成tags文件

2. 将光标已至变量或函数处，使用快捷键`ctrl+]`可查看定义 ,使用快捷键`ctrl+o` 返回

3. 使用`:TlistToggle`查看列表,使用快捷键`ctrl+ww` 列表和代码区切换
























