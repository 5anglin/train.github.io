
## 1 安装虚拟机系统

官网：https://www.virtualbox.org/

1. 下载并安装 VirtualBox

   ```
   https://download.virtualbox.org/virtualbox/6.1.38/VirtualBox-6.1.38-153438-Win.exe
   ```

   ![image-20221004160408015](resource/image-20221004160408015.png)

2. 下载ubuntu

   官网[企业开源和Linux | Ubuntu](https://cn.ubuntu.com/)
   
   ![image-20221004160525993](resource/image-20221004160525993.png)

3. 安装虚拟机系统

   ![image-20221005110243626](resource/image-20221005110243626.png)
   
   ![image-20221005110444065](resource/image-20221005110444065.png)
   
   ![image-20221005110458217](resource/image-20221005110458217.png)
   
   ![image-20221005110527685](resource/image-20221005110527685.png)
   
   ![image-20221005110616253](resource/image-20221005110616253.png)
   
   ![image-20221005110743042](resource/image-20221005110743042.png)
   
   ![image-20221005110755049](resource/image-20221005110755049.png)
   
   ![image-20221005110856019](resource/image-20221005110856019.png)
   
   ![image-20221005110904082](resource/image-20221005110904082.png)
   
   ![image-20221005110939298](resource/image-20221005110939298.png)
   
   ![image-20221005111021635](resource/image-20221005111021635.png)
   
   ![image-20221005111100209](resource/image-20221005111100209.png)
   
   ![image-20221005111253922](resource/image-20221005111253922.png)
   
   ![image-20221005111715791](resource/image-20221005111715791.png)
   
   ![image-20221005111724960](resource/image-20221005111724960.png)
   
   ![image-20221005111736170](resource/image-20221005111736170.png)
   
   ![image-20221005111809803](resource/image-20221005111809803.png)
   
   ![image-20221005111835622](resource/image-20221005111835622.png)
   
   > subnet: 0-255 --> .0/24
   
   ![image-20221005111925381](resource/image-20221005111925381.png)
   
   ![image-20221005112002129](resource/image-20221005112002129.png)
   
   ![image-20221005112012554](resource/image-20221005112012554.png)
   
   ![image-20221005112026002](resource/image-20221005112026002.png)
   
   ![image-20221005112039739](resource/image-20221005112039739.png)
   
   ![image-20221005112106713](resource/image-20221005112106713.png)
   
   ![image-20221005112114915](resource/image-20221005112114915.png)
   
   ![image-20221005113116013](resource/image-20221005113116013.png)
   
   ![image-20221005113129541](resource/image-20221005113129541.png)
   
   ![image-20221005113142985](resource/image-20221005113142985.png)
   
   ![image-20221005113206291](resource/image-20221005113206291.png)
   
   ![image-20221005113839770](resource/image-20221005113839770.png)
   
   ![image-20221005114358529](resource/image-20221005114358529.png)
   
   ![image-20221005114408272](resource/image-20221005114408272.png)
   
   虚拟机访问外网测试
   
   ![image-20221005114437649](resource/image-20221005114437649.png)
   
   虚拟机与宿主机间网络测试
   
   ![image-20221005114533736](resource/image-20221005114533736.png)
   
   远程终端密码登录虚拟机
   
   ![image-20221005114710144](resource/image-20221005114710144.png)
   
   ![image-20221005114719470](resource/image-20221005114719470.png)
   
   ![image-20221005114742390](resource/image-20221005114742390.png)
   
   远程终端免密登录
   
   1. windows下
   
   ```bash
   ssh-keygen -t ed25519 -C "name" -f ~/.ssh/vm_id_rsa
   ```
   
   ![image-20221005115005070](resource/image-20221005115005070.png)
   
   2. 复制.pub至虚拟主机的用户目录下的.ssh
   
   ![image-20221005115101977](resource/image-20221005115101977.png)
   
   虚拟机下
   
   ```bash
   cat ~/.ssh/vm_id_rsa.pub > ~/.ssh/authorized_keys
   ```
   
   设置用户名和密钥文件
   
   ![image-20221005115311357](resource/image-20221005115311357.png)
   
   登录成功
   
   ![image-20221005115346098](resource/image-20221005115346098.png)
   
   在VirtualBox中可以设置无界面启动，然后通过远程终端登录即可
   
   ![image-20221005115648397](resource/image-20221005115648397.png)


