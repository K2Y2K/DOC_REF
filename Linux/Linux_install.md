# Unbuntu系统apt-get安装

## 一、Ubuntu系统下WebStorm安装

下载地址：http://www.jetbrains.com/webstorm/download/#section=linux-version

1、lee@lee-ThikingStation:~/Downloads$ sudo cp WebStorm-11.0.2.tar.gz /opt/webstorm/

2、lee@lee-ThikingStation:~/Downloads$ cd /opt/webstorm

3、lee@lee-ThikingStation:/opt/webstorm$ sudo tar -zxvf WebStorm-11.0.2.tar.gz

4、lee@lee-ThikingStation:/opt/webstorm$ sudo mv WebStorm-143.1184.19/ ./webstorms　【修改文件名称】

5、由于webstorm.sh不能识别JAVA_HOME，只能手动添加了。

lee@lee-ThikingStation:/opt/webstorm/webstorms/bin$ sudo gedit webstorm.sh

在第7行加入

JAVA_HOME=/usr/lib/jvm/jdk1.7.0_79

6.安装
lee@lee-ThikingStation:/opt/webstorm/webstorms/bin$ ./webstorm.sh

{
lee@lee-ThikingStation:/opt/webstorm/webstorms/bin$ sudo su
root@lee-ThikingStation:/opt/webstorm/webstorms/bin# ./webstorm.sh
【sudo sh webstorm.sh //启动webstorm】
}

一步步安装就完成了。

7.破解

注册时选择“License server”输入“http://idea.imsxm.com/”点击“OK”快速激活JetBrains系列产品

安装完成也可在Help --> Register --> License server --> License server address --> 输入 http://idea.imsxm.com/ --> ok 激活成功！

８、设置webstom.sh启动的快捷方式
在主目录下创建空白文档，（或者用命令创建文档#vi /usr/share/applications/etbrains-webstorm.desktop），将如下内容拷贝到文档中：
[Desktop Entry]
    Encoding=UTF-8
    Name=WebStorm
    GenericName=Nodejs,HTML5, JavaScript and CSS editor
    Comment=Nodejs,HTML5, JavaScript and CSS editor
    Exec=/opt/webstorm/webstorms/bin/webstorm.sh
    Icon=/opt/webstorm/webstorms/bin/webstorm.svg
    Terminal=false
    StartupNotify=true
    Type=Application
    Categories=Application;Development

然后给文档命名为：webstorm.desktop 再给文件添加其他用户访问权限【运行“chmod u+x eclipse.desktop“命令即可，
或者直接右键点击文档-》属性-》权限，勾上“允许做为程序执行文件”；】。
双击执行该文件，该快捷方式创建成功。（可以选择锁定到启动器上）。

## 二、Ubuntu16.04 下如何安装搜狗拼音输入法

（1）.添加fcitx键盘输入法系统【系统默认是iBus】

1.将下载源添加至系统源：

sudo add-apt-repository ppa:fcitx-team/nightly

2.更新系统列表获得最新软件版本信息

sudo apt-get update

3.安装fcitx

sudo apt-get install fcitx                      

4.安装fcitx的配置工具

sudo apt-get install fcitx-config-gtk

5.安装fcitx的table-all软件包

sudo apt-get install fcitx-table-all

6.安装输入法切换工具

sudo apt-get install im-switch

（2）.安装搜狗输入法

1.下载linux版搜狗输入法。下载地址：http://pinyin.sogou.com/linux/。

2.用dpkg命令来安装搜狗输入法资源包

sudo dpkg -i  下载的包【以deb结尾】

3.设置语言选项（如果找不到该设置，执行sudo apt-get install language-selector-gnome）：
将键盘输入法系统由默认的iBus设置为fcitx，然后注销重新进入。
进入之后在系统输入法中添加搜狗输入法即可！
【如果安装出错，缺少依赖库，卸载,安装依赖库再安装
sudo apt --purge remove sogoupinyin
sudo apt install libopencc1 libopencc2 fcitx-libs fcitx-libs-qt fonts-droid-fallback libqtwebkit4 
sudo dpkg -i sogoupinyin_2.2.0.0102_amd64.deb】

## 三、ubuntu 安装印象笔记

```
sudo add-apt-repository ppa:nvbn-rm/ppa

sudo apt-get update

sudo apt-get install everpad

```



## 四、ubuntu16.06完全安装gnome桌面及应用

 方法1)、直接安装

 $sudo apt-get  install gnome  

 方法2)、ubuntu最简化安装使用gnome桌面             

           1、gnome桌面窗口管理程序
    
             $sudo apt-get  install gnome-shell  
    
           2、安装gnome面板
    
           $sudo apt-get  install  gnome-panel  
    
           3、安装gnome菜单
    
           $sudo apt-get  install   gnome-menus
    
           4、安装gnome-session
    
            $sudo apt-get  install  gnome-session
    
           5、安装gdm会话切换器
    
             $sudo apt-get  install  gdm
    
           6、注销并选择gnome登陆
    
          {方法三 只安装gnome桌面环境（不全）：
          $ sudo apt-get install gnome-session-flashback
          }
    
           安装完成之后，注销系统，进入登陆界面，点击Ubuntu icon。
           【安装Cairo-Dock (Fallback Mode) ，也是一种桌面环境】

## 五、安装firefox开发者版本

   下载地址：

     https://download-installer.cdn.mozilla.net/pub/devedition/releases/；
   //如果本机安装了Firefox别的版本,查看版本安装位置，修改软连接
   lee@lee:/opt/firefox$ firefox -v
     Mozilla Firefox 57.0

   lee@lee:/opt/firefox$ which firefox
   /usr/bin/firefox

   lee@lee:/opt/firefox$ sudo mv /usr/bin/firefox /usr/bin/firefox-old

   lee@lee:/opt/firefox$ firefox-old -v
   Mozilla Firefox 57.0
 //把新安装的Firefox解压到/opt/目录下，再建立软连接
  lee@lee:~$ sudo mv firefox  /opt/
  lee@lee:~$ sudo ln -s /opt/firefox/firefox  /usr/bin/firefox
  lee@lee:~$ firefox -v
  Mozilla Firefox 58.0
  lee@lee:~$

  //卸载Ubuntu自带的Firefox：
  dpkg --get-selections |grep firefox

  sudo apt-get autoremove firefox firefox-branding firefox-gnome-support ubufox     //保留配置文件

  sudo apt-get purge firefox firefox-branding firefox-gnome-support ubufox     //配置文件也一起删掉 



## 六、安装nodejs

手动安装：
方法1）

1、先下载nodejs,下载地址可参考：https://nodejs.org/zh-cn/download/；
2、将node-v8.9.1-linux-x64.tar.xz 解压并重命名为node 并移动到/opt/目录下
    tar -zxvf node-v8.9.1-linux-x64.tar.xz
    mv node-v8.9.1-linux-x64  node
    sudo mv node /opt/
    查看node版本
  	cd /opt/node/bin  
    ls  
    ./node -v  
3、设置软连接：
    sudo ln -s /opt/node/bin/node /usr/local/bin/node
    sudo ln -s /opt/node/bin/npm /usr/local/bin/npm

方法2）
先解压，再重命名为node(mv node-v8.9.1-linux-x64 /node )，再移动到opt目录下(mv node /opt/)，
然后vim /etc/profile（当然可以配置到个人的环境变量中），i进入编辑模式，在最后添加export NODE=/opt/node,
然后换行添加export PATH=$PATH:$NODE/bin,esc退出编辑模式，:wq进行保存退出，(也可以换成添加export PATH=$PATH:/opt/node/bin)
然后用source /etc/profile命令更新环境变量，echo $PATH查看输出是否已有node路径，有则环境变量生效，
然后node -v查看node版本，假如输出版本号，则node环境配置成功。（该步骤需重启电脑才能生效）

【安装了nvm （nvm ls ;nvm install 8.0.0 ;nvm use 8.0.0）管理note版本的工具
 安装nvm命令：
 curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.1/install.sh | bash
 】

## 七、Ubuntu安装sublime text3

（一）安装
方法1）需要翻墙：
在终端中输入：
sudo add-apt-repository ppa:webupd8team/sublime-text-3  #添加sublime text 3的仓库
sudo apt-get update    #更新软件库
sudo apt-get install sublime-text-installer  #安装Sublime Text
如果需要启动sublime，那么在terminal中输入:subl;

卸载 sublime text 命令：
sudo apt-get remove sublime-text-installer（只卸载应用程序，并没有删除配置文件）
sudo apt-get --purge remove sublime-text-installer
【
安装稳定版本
首先下载Sublime Text 3存储库的安全密钥。这将确保从回购站下载的软件包经过身份验证等：
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
下一步。 将Sublime Text稳定库添加到您的软件源中：
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
如果你想运行Sublime Text的最新开发版本，你可以：只需运行下面这个命令，而不是上面的命令：
echo "deb https://download.sublimetext.com/ apt/dev/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
然后更新软件库，下载：
sudo apt-get update
sudo apt-get install sublime-text-installer
】

方法2）手动安装：
先去官网下载sublime text 安装包，解压并移动到opt目录下，
sudo mv sublime_text_3 /opt/
sudo ln -s /opt/sublime_text_3/sublime_text /usr/bin/subl　　//配置快捷键为subl（自定义）

注册码：
Sublime_text_3 for 3143 授权码/注册码

    —– BEGIN LICENSE —–
    TwitterInc
    200 User License
    EA7E-890007
    1D77F72E 390CDD93 4DCBA022 FAF60790
    61AA12C0 A37081C5 D0316412 4584D136
    94D7F7D4 95BC8C1C 527DA828 560BB037
    D1EDDD8C AE7B379F 50C9D69D B35179EF
    2FE898C4 8E4277A8 555CE714 E1FB0E43
    D5D52613 C3D12E98 BC49967F 7652EED2
    9D2D2E61 67610860 6D338B72 5CF95C69
    E36B85CC 84991F19 7575D828 470A92AB
    —— END LICENSE ——

sublime_text_3 for 3126的授权码/注册码:

```
—– BEGIN LICENSE —–
Michael Barnes
Single User License
EA7E-821385
8A353C41 872A0D5C DF9B2950 AFF6F667
C458EA6D 8EA3C286 98D1D650 131A97AB
AA919AEC EF20E143 B361B1E7 4C8B7F04
B085E65E 2F5F5360 8489D422 FB8FC1AA
93F6323C FD7F7544 3F39C318 D95E6480
FCCC7561 8A4A1741 68FA4223 ADCEDE07
200C25BE DBBC4855 C4CFB774 C5EC138C
0FEC1CEF D9DCECEC D3A5DAD1 01316C36
—— END LICENSE ——
```


{*如果是默认配置的情况下，当有新的版本出现的时候总是会有更新提示，为了取消更新提示，需进行如下操作：

    菜单Sublime Text → Preferences → Settings（点击），则会打开Preferences.sublime-settings — User文件。
    
    将下面的代码复制并粘贴到该文件的最外层中括号里。需要注意的是：取消更新提示的前提是你已经完成了Sublime Text的激活（ 
    
    "update_check":false
}
【安装Package Control插件：https://packagecontrol.io，其他请参考：http://blog.csdn.net/stilling2006/article/details/54376743；】


（二）Ubuntu下让sublime显示在右键菜单上
1、Terminal里面cd到如下路径：
~/.local/share/nautilus/scripts

2、在该目录下创建一个文件，命名“sublime”（名字可以随意）。
sudo gedit sublime

文件内容如下：

!/bin/bash

exec "<把这里替换成sublime的全路径>" $NAUTILUS_SCRIPT_SELECTED_FILE_PATHS
【我自己的路径

!/bin/bash

exec "/opt/sublime_text_3/sublime_text" $NAUTILUS_SCRIPT_SELECTED_FILE_PATHS】
【环境变量 描述
NAUTILUS_SCRIPT_SELECTED_FILE_PATHS	所选文件的新行分割路径（仅针对本地）
NAUTILUS_SCRIPT_SELECTED_URIS	所选文件的新行分割 URIs
NAUTILUS_SCRIPT_CURRENT_URI	当前位置
NAUTILUS_SCRIPT_WINDOW_GEOMETRY	当前窗口的位置和大小
】
3、保存文件并把文件属性设置为可执行：
chmod u+x sublime


（三）为sublime text创建桌面快捷方式
在~/桌面/ 下新建一个文件，如Sublime.desktop:

```
[Desktop Entry]
Type=Application
Name=Sublime Text 3
Comment=Sublime Text 3
Categories=Edit;Python;
Exec= /opt/sublime_text_3/sublime_text
Icon=/opt/sublime_text_3/Icon/48x48/sublime_text.png
Terminal=false
StartupNotify=true
```

保存即可.
确保Sublime.desktop有可执行权限：
sudo chmod +x Sublime.desktop

## 八、安装tomcat连接mysql的jar包

下载mysql的Java连接程序jar包，然后为tomcat导入mysql的连接jar包：
sudo cp -r mysql-connector-java-5.1.21-bin.jar /usr/lib/tomcat/apache-tomcat-8.0.47/lib;
（将该驱动程序复制到Tomcat服务器安装目录的/common/lib或/lib文件夹中,重启服务器。）

## 九、右键菜单栏中出现终端

sudo apt-get install nautilus-open-terminal



## 十、安装wireshark

**安装：**

```
sudo apt-add-repository ppa:wireshark-dev/stable
sudo apt-get update
sudo apt-get install wireshark
```

安装过程中需要设置，选择“是”，若没出现，安装之后再设置（方法一）：

```
sudo dpkg-reconfigure wireshark-common  //GUI出于安全方面的考虑，普通用户不能够打开网卡设备进行抓包，Wireshark不建议用户通过sudo在root权限下运行,该命令是使普通用户获取root权限。
```

然后

```
sudo gedit /etc/group

wireshark:x:132:lee    ;lee对应当前登录的用户名,我用户名是lee

```

注销后重启，在终端输入`wireshark`进行启动。



配置普通用户登录wireshark方法二：

```
sudo groupadd wireshark        		//添加wireshark用户组；
sudo chgrp wireshark /usr/bin/dumpcap		//修改dumpcap所属组为wireshark；
sudo chmod 4755 /usr/bin/dumpcap		//修改dumpcap的权限，让用户组拥有root权限；
sudo gpasswd -a lee wireshark   	//将普通用户lee加入wireshark组；
```



****

**命令行使用tshark：**

如果想要使用命令行版本时，需要安装tshark

```
sudo apt-get install tshark
```

命令行抓取特定网卡和端口的包

```
tshark -i eth0 port 6060
```



**命令行使用tcpdump：**

常用的参数：

```
tcpdump [-i 网卡] -nnAX '表达式'
```

各参数说明如下：

- -i：interface 监听的网卡。
- -nn：表示以ip和port的方式显示来源主机和目的主机，而不是用主机名和服务。
- -A：以ascii的方式显示数据包，抓取web数据时很有用。
- -X：数据包将会以16进制和ascii的方式显示。
- 表达式：表达式有很多种，常见的有：host 主机；port 端口；src host 发包主机；dst host 收包主机。多个条件可以用and、or组合，取反可以使用!，更多的使用可以查看man 7 pcap-filter。

```
$sudo tcpdump -i eno1  		//监听eno1
$tcpdump -i eno1 -nn "tcp" 			//监听指定协议的数据
$tcpdump -i eno1 -nn 'host 192.168.1.231' 	//监听指定的主机,这样只有192.168.1.231这台主机发送的包才会被抓取。
$tcpdump -i eno1 -nn 'src host 192.168.1.231' 		//这样只有192.168.1.231这台主机发送的包才会被抓取。

$ tcpdump -i eno1 -nn 'dst host 192.168.1.231'		//这样只有192.168.1.231这台主机接收到的包才会被抓取。

$ tcpdump -i eno1 -nnA 'port 443'  //监听指定端口,用来监听主机的80端口收到和发送的所有数据包。

$ tcpdump -i eno1 -nnA 'port 80 and src host 192.168.1.231'  //监听指定主机和端口,多个条件可以用and，or连接。上例表示监听192.168.1.231主机通过80端口发送的数据包。

$ tcpdump -i eno1 -nnA '!port 22		//监听除某个端口外的其它端口的数据包。

```

## 十一、安装samba

```
1、samba的安装
$ apt-get install samba samba-common smbclient 
//保存现有的配置文件
$ cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
2、Samba配置（可不设置）
$ vim /etc/samba/smb.conf
在文件 smb.conf 最后追加
# Add by Tony for Samba && tftpd
[tony]
   path = /home/tony
   available = yes
   browseable = yes
   writable = yes
[tftpboot]
   path = /home/tony/tftpdir
   available = yes
   browseable = yes
   writable = yes
   public = yes
[opt]
   path = /opt
   available = yes
   browseable = yes
   writable = yes
   public = yes
上面 tony 是用户名，不能有“#”符号

3、创建samba帐户
$ smbpasswd -a USERNAME (USERNAME换成你的用户名)
会要求你输入samba帐户的密码
New SMB password:
Retype new SMB password:
［ 如果没有这一步， 当你登录时会提示 session setup failed:
NT_STATUS_LOGON_FAILURE

4、重启samba服务器
$ /etc/init.d/smbd reload (修改过smb.conf的话要执行一次)
$ /etc/init.d/smbd restart

5、测试
可以到windows下输入ip试一下了
在文件夹处输入 "\\" + "Ubuntu机器的ip或主机名"
Ubuntu 14.04 访问Window XP下的文件
直接在地址栏中输入 "smb://1XP机器的ip地址/

6、开机随机自启动
如上配置，如果重启机器samba将不可以使用，需要配置系统随机启动服务
$ apt-get install -y sysv-rc-conf
$ sysv-rc-conf 
配置 smbd   samba   nmbd 服务 等级 1到5

```

## **十二、Install Typora in Ubuntu**

```
1. Add Typora Linux repository via command:
$ sudo add-apt-repository 'deb https://typora.io linux/'

2. Setup the key:
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE

3. Finally update and install this simple markdown editor:
$ sudo apt update
$ sudo apt install typora

4.And the Typora markdown editor can be removed via command:
$ sudo apt remove typora && sudo apt autoremove
```

## 十三、Install TeamViewer in ubuntu

```
1. 首先到https://www.teamviewer.com/zhcn/download/linux/下载相应linux版本的Teamviewer，即软件安装包teamviewer_13.1.3026_amd64.deb。

2. 安装依赖包，执行命令：(64位系统忽视如下命令，若是32位的系统则需要执行如下命令)
$ sudo apt-get install libjpeg62:i386 libxinerama1:i386 libxrandr2:i386 libxtst6:i386 ca-certificates

3. 安装deb软件包，执行命令：
sudo dpkg -i teamviewer_13.1.3026_amd64.deb
```

## 十四、Install  Virtualbox in ubuntu 16.04

```
Step 1 – Prerequsities
$ sudo apt-get update
$ sudo apt-get upgrade

Step 2 – Configure Apt Repository
$ sudo gedit /etc/apt/sources.list
#将下面的软件源地址加入sources.list的末尾
deb http://download.virtualbox.org/virtualbox/debian xenial contrib
or
$ sudo add-apt-repository "deb http://download.virtualbox.org/virtualbox/debian `lsb_release -cs` contrib"

Step 3 – Import VirtualBox Package Sign Key
$ wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
$ wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -

Step 4 – Install Oracle VirtualBox
$ sudo apt-get update
$ sudo apt-get install virtualbox-5.2

Step 5 – Launch VirtualBox
$ virtualbox
```

修改vbox虚拟磁盘空间

```
#####查看虚拟磁盘空间信息，确认需要扩容的虚拟硬盘
ldx@ldx:~$ VBoxManage showmediuminfo "/mnt/ldx/software/virtualbox/vms/win777/win777.vdi" 
UUID:           8235534f-78a1-4774-a8f5-0d5a91e4da6c
Parent UUID:    base
State:          locked write
Type:           normal (base)
Location:       /mnt/ldx/software/virtualbox/vms/win777/win777.vdi
Storage format: VDI
Format variant: dynamic default
Capacity:       32768 MBytes
Size on disk:   2 MBytes
Encryption:     disabled
In use by VMs:  win777 (UUID: f4b63c38-9810-4f31-978f-3d96189d1c7f)

#####计算出要扩容的空间大小，这里是要扩展到50G(1024*1024*1024*50)。
ldx@ldx:~$ set /a 1024*50

#####扩展虚拟硬盘空间，必须是动态分配模式的硬盘不能是固定大小模式的，最后的数字替换成上面计算出的50G.(--resize 51200 单位是Ｍ)
ldx@ldx:~$ vboxmanage modifymedium --resize 51200 "/mnt/ldx/software/virtualbox/vms/win777/win777.vdi"
0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
ldx@ldx:~$ VBoxManage showmediuminfo "/mnt/ldx/software/virtualbox/vms/win777/win777.vdi" 
UUID:           8235534f-78a1-4774-a8f5-0d5a91e4da6c
Parent UUID:    base
State:          created
Type:           normal (base)
Location:       /mnt/ldx/software/virtualbox/vms/win777/win777.vdi
Storage format: VDI
Format variant: dynamic default
Capacity:       51200 MBytes
Size on disk:   2 MBytes
Encryption:     disabled
In use by VMs:  win777 (UUID: f4b63c38-9810-4f31-978f-3d96189d1c7f)
ldx@ldx:~$ 
```



## 十五、Install Python in ubuntu and update alternatives version for python

```

Ubuntu 16.04 comes with both Python 2.7 and Python 3.5 by default.

1.查看Ubuntu系统已安装的python版本及当前使用的python版本
$ ls /usr/bin/python*python--version

2. install Python 3.6 along with them via a third-party PPA by doing following steps:
step 1:
$ sudo add-apt-repository ppa:jonathonf/python-3.6
step 2:
$ sudo apt-get update
$ sudo apt-get install python3.6

3.设置update-alternatives  切换python版本
显示Python代替版本信息：
$ update-alternatives --list python 
执行后error显示：
update-alternatives: error: no alternatives for python 表示Python代替版本尚未安装

解决error执行以下命令安装：
update-alternatives: --install needs <link> <name> <path> <priority>
$ update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
print：update-alternatives: using /usr/bin/python2.7 to provide /usr/bin/python (python) in auto mode
$ update-alternatives --install /usr/bin/python python /usr/bin/python3.5 2
print：update-alternatives: using /usr/bin/python3.5 to provide /usr/bin/python (python) in auto mode
(or
$ sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.5 1
$ sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 2
)
查看默认版本：
$ python --version
注：上述命令中设置 /usr/bin/python3.5 设置的优先级为2，则3.5为默认版本。

切换版本update-alternatives --config python
$ sudo update-alternatives --config python 
(or 
$ sudo update-alternatives --config python3
$ python3 -V)
4.python3版本安装pip
$ sudo apt-get install python3-pip
安装完以后运行pip3 -V显示版本
升级pip版本:
$ pip3 install --upgrade pip
(or
$ python -m pip install --upgrade pip
)
```

**bug:**

```
1、升级pip版本出现以下错误(这是pip 10.0.0版本的BUG)：
Traceback (most recent call last): 
File “/usr/bin/pip3”, line 9, in 
from pip import main
解决办法:修改 /usr/bin/pip 文件
from pip import main
if __name__ == '__main__':
    sys.exit(main())
改为：
from pip import __main__
if __name__ == '__main__':
    sys.exit(__main__._main())
2、 recreate the symlink:
$ sudo rm /usr/bin/python3
$ sudo ln -s python3.5 /usr/bin/python3
```

