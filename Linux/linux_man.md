# Linux_man

## 一、Linux 系统启动过程

Linux系统的启动过程可以分为5个阶段：

```
- 内核的引导。
- 运行init。
- 系统初始化。
- 建立终端 。
- 用户登录系统。

```

**内核的引导**

当计算机打开电源后，首先是BIOS开机自检，按照BIOS中设置的启动设备（通常是硬盘）来启动。操作系统接管硬件以后，首先读入 /boot 目录下的内核文件。

**运行init**

init 进程是系统所有进程的起点，没有这个进程，系统中任何进程都不会启动。init 程序首先是需要读取配置文件 /etc/inittab。

​               **运行级别**

许多程序需要开机启动。它们在Windows叫做"服务"（service），在Linux就叫做"守护进程"（daemon）。

init进程的一大任务，就是去运行这些开机启动的程序。

但是，不同的场合需要启动不同的程序，比如用作服务器时，需要启动Apache，用作桌面就不需要。

Linux允许为不同的场合，分配不同的开机启动程序，这就叫做"运行级别"（runlevel）。也就是说，启动时根据"运行级别"，确定要运行哪些程序。

```
Linux系统有7个运行级别(runlevel)：

运行级别0：系统停机状态，系统默认运行级别不能设为0，否则不能正常启动
运行级别1：单用户工作状态，root权限，用于系统维护，禁止远程登陆
运行级别2：多用户状态(没有NFS)
运行级别3：完全的多用户状态(有NFS)，登陆后进入控制台命令行模式
运行级别4：系统未使用，保留
运行级别5：X11控制台，登陆后进入图形GUI模式
运行级别6：系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动
```

**系统初始化**

在init的配置文件中有这么一行： si::sysinit:/etc/rc.d/rc.sysinit　它调用执行了/etc/rc.d/rc.sysinit，而rc.sysinit是一个bash shell的脚本，它主要是完成一些系统初始化的工作，rc.sysinit是每一个运行级别都要首先运行的重要脚本。

它主要完成的工作有：激活交换分区，检查磁盘，加载硬件模块以及其它一些需要优先执行任务。

```
l5:5:wait:/etc/rc.d/rc 5
```

这一行表示以5为参数运行/etc/rc.d/rc，/etc/rc.d/rc是一个Shell脚本，它接受5作为参数，去执行/etc/rc.d/rc5.d/目录下的所有的rc启动脚本，/etc/rc.d/rc5.d/目录中的这些启动脚本实际上都是一些连接文件，而不是真正的rc启动脚本，真正的rc启动脚本实际上都是放在/etc/rc.d/init.d/目录下。

而这些rc启动脚本有着类似的用法，它们一般能接受start、stop、restart、status等参数。

/etc/rc.d/rc5.d/中的rc启动脚本通常是K或S开头的连接文件，对于以以S开头的启动脚本，将以start参数来运行。

而如果发现存在相应的脚本也存在K打头的连接，而且已经处于运行态了(以/var/lock/subsys/下的文件作为标志)，则将首先以stop为参数停止这些已经启动了的守护进程，然后再重新运行。

这样做是为了保证是当init改变运行级别时，所有相关的守护进程都将重启。

至于在每个运行级中将运行哪些守护进程，用户可以通过chkconfig或setup中的"System Services"来自行设定。

**建立终端** 

rc执行完毕后，返回init。这时基本系统环境已经设置好了，各种守护进程也已经启动了。

init接下来会打开6个终端，以便用户登录系统。在inittab中的以下6行就是定义了6个终端：

```
1:2345:respawn:/sbin/mingetty tty1
2:2345:respawn:/sbin/mingetty tty2
3:2345:respawn:/sbin/mingetty tty3
4:2345:respawn:/sbin/mingetty tty4
5:2345:respawn:/sbin/mingetty tty5
6:2345:respawn:/sbin/mingetty tty6

```

从上面可以看出在2、3、4、5的运行级别中都将以respawn方式运行mingetty程序，mingetty程序能打开终端、设置模式。

同时它会显示一个文本登录界面，这个界面就是我们经常看到的登录界面，在这个登录界面中会提示用户输入用户名，而用户输入的用户将作为参数传给login程序来验证用户的身份。

**用户登录系统**

```
一般来说，用户的登录方式有三种：

（1）命令行登录
（2）ssh登录
（3）图形界面登录
```

对于运行级别为5的图形方式用户来说，他们的登录是通过一个图形化的登录界面。登录成功后可以直接进入KDE、Gnome等窗口管理器。

而本文主要讲的还是文本方式登录的情况：当我们看到mingetty的登录界面时，我们就可以输入用户名和密码来登录系统了。

Linux的账号验证程序是login，login会接收mingetty传来的用户名作为用户名参数。

然后login会对用户名进行分析：如果用户名不是root，且存在/etc/nologin文件，login将输出nologin文件的内容，然后退出。

这通常用来系统维护时防止非root用户登录。只有/etc/securetty中登记了的终端才允许root用户登录，如果不存在这个文件，则root可以在任何终端上登录。

/etc/usertty文件用于对用户作出附加访问限制，如果不存在这个文件，则没有其他限制。

<p>在分析完用户名后，login将搜索/etc/passwd以及/etc/shadow来验证密码以及设置账户的其它信息，比如：主目录是什么、使用何种shell。如果没有指定主目录，将默认为根目录；如果没有指定shell，将默认为/bin/bash。</p>

**图形模式与文字模式的切换方式**

Linux预设提供了六个命令窗口终端机让我们来登录。

默认我们登录的就是第一个窗口，也就是tty1，这个六个窗口分别为tty1,tty2 … tty6，你可以按下`Ctrl + Alt + F1 ~ F6` 来切换它们。

如果你安装了图形界面，默认情况下是进入图形界面的，此时你就可以按`Ctrl + Alt + F1 ~ F6`来进入其中一个命令窗口界面。

当你进入命令窗口界面后再返回图形界面只要按下`Ctrl + Alt + F7` 就回来了。

如果你用的vmware 虚拟机，命令窗口切换的快捷键为 `Alt + Space + F1~F6`. 如果你在图形界面下请按`Alt + Shift + Ctrl + F1~F6` 切换至命令窗口。

![bg2013081707](https://img.w3cschool.cn/attachments/uploads/2014/06/bg2013081707.png)

​        **关机命令**

正确的关机流程为：sysnc > shutdown > reboot > halt

```
sync 将数据由内存同步到硬盘中。

shutdown 关机指令，可以用man shutdown 来看一下帮助文档。例如可以运行如下命令关机：

shutdown –h 10 ‘This server will shutdown after 10 mins’ 这个命令告诉大家，计算机将在10分钟后关机，并且会显示在登陆用户的当前屏幕中。

shutdown –h now 立马关机

shutdown –h 20:25 系统会在今天20:25关机

shutdown –h +10 十分钟后关机

shutdown –r now 系统立马重启

shutdown –r +10 系统十分钟后重启

reboot 就是重启，等同于 shutdown –r now

halt 关闭系统，等同于shutdown –h now 和 poweroff
```



## 二、Linux　系统下的截图快捷键

(system setting －>  keyboard －>screenshots）

也可以在终端中输入"gnome-screenshot -h"回车，可以看到截图命令；
shift+alt+s:抓取屏幕的一个区域　而不是整个屏幕；（自定义时命令输入：gnome-screenshot　-a）;
shift+alt+s：抓取窗口，不是整个屏幕;
print screen：全屏；

## 三、ubuntu 使用gedit打开文档

sudo gedit /etc/profile  //打开设置全局环境脚本文档
source /etc/profile　　　　//保存后同步更新文档

{配置在/etc/profile（所有用户有效）
~/.bashrc（当前用户有效） }

## 四、ubuntu解压与压缩文件

【解压.zip文件】

Ubuntu中貌似已经安装了unzip软件，解压命令如下：

unzip  ./FileName.zip
【（
 zip -r data.zip data　解释：将data文件夹压缩成了data.zip格式。
unzip data.zip　解释：将data.zip文件解压到当前文件夹。　
unzip data.zip -d ./b　（将data.zip文件解压到当前文件下的b文件夹中，如果b不存在则自动创建））
】

如果没安装unzip的话，可以通过如下命令安装：
sudo apt-get install unzip

 zip -r myfile.zip ./*
将当前目录下的所有文件和文件夹全部压缩成myfile.zip文件,－r表示递归压缩子目录下所有文件.

【解压.rar文件】 

安装unrar软件

sudo apt-get install unrar

卸载unrar软件

sudo apt-get remove unrar

解压.rar文件

unrar x ./FileName.rar 
【
unrar x vpsyou.rar //解压 vpsyou.rar 到当前目录

rar a vpsyou.rar ./vpsyou.com/ 　（将 vpsyou.com 目录打包为 vpsyou.rar）

unrar e file.rar //只会把压缩包里的文件解压出来，文件包没有了，解压rar
】

【其他文件】
【解压】

​        tar zxvf xxx.tar.gz -C /usr/xxx　　　// 解压xxx.tar.gz到/usr/xxx 路径下

　　tar –xvf file.tar //解压 tar包

　　tar -xzvf file.tar.gz //解压tar.gz

　　tar -xjvf file.tar.bz2 //解压 tar.bz2

　　tar –xZvf file.tar.Z //解压tar.Z
   unrar x vpsyou.rar //解压 vpsyou.rar 到当前目录 
   unzip data.zip -d ./b　（将data.zip文件解压到当前文件下的b文件夹中，如果b不存在则自动创建））

【压缩】

　　tar –cvf jpg.tar *.jpg //将目录里所有jpg文件打包成tar.jpg

　　tar –czf jpg.tar.gz *.jpg //将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz

　　tar –cjf jpg.tar.bz2 *.jpg //将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2

　　tar –cZf jpg.tar.Z *.jpg //将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z

　　rar a jpg.rar *.jpg //rar格式的压缩，需要先下载rar for linux

　　zip jpg.zip *.jpg //zip格式的压缩，需要先下载zip for linux
【tar 命令详解】

　　-c: 建立压缩档案

　　-x：解压

　　-t：查看内容

　　-r：向压缩归档文件末尾追加文件

　　-u：更新原压缩包中的文件

　　这五个是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但只能用其中一个。下面的参数是根据需要在压缩或解压档案时可选的。

　　-c: 建立压缩档案

　　-x：解压

　　-t：查看内容

　　-r：向压缩归档文件末尾追加文件

　　-u：更新原压缩包中的文件

　　下面的参数-f是必须的

　　-f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名。

##   五、文件的创建、拷贝和移动、重命名

  **文件的创建：**

```
mkdir [-mp] 目录名称
  -m ：配置文件的权限！
  -p ：帮助你直接将所需要的目录(包含上一级目录)递回创建起来！
rmdir [-p] 目录名称
  -p ：连同上一级『空的』目录也一起删除
```

  **文件的拷贝：**

```
cp [-adfilprsu] 来源档(source) 目标档(destination)
cp [options] source1 source2 source3 .... directory

cp a.txt  ~/Downloads/b
cp -i /opt/a   /opt/b   //将opt/a目录中所有文件及子目录拷贝到目录/opt/下并重名为b。
cp -r /opt/a   /opt/b   //将opt/a目录中所有文件及子目录拷贝到目录/opt/b中。

选项与参数：

-a ：相当於 -pdr 的意思，至於 pdr 请参考下列说明；(常用)
-d ：若来源档为连结档的属性(link file)，则复制连结档属性而非文件本身；
-f ：为强制(force)的意思，若目标文件已经存在且无法开启，则移除后再尝试一次；
-i ：若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用)
-l ：进行硬式连结(hard link)的连结档创建，而非复制文件本身；
-p ：连同文件的属性一起复制过去，而非使用默认属性(备份常用)；
-r ：递回持续复制，用於目录的复制行为；(常用)
-s ：复制成为符号连结档 (symbolic link)，亦即『捷径』文件；
-u ：若 destination 比 source 旧才升级 destination ！
```

  **文件的移动：**

```
  mv ~/Downloads/a   /opt/b/  //将/a目录移动到/opt/b/下
  mv ~/Downloads/a   /opt/b //将/a目录移动到/opt下，并重命名为b

  文件的重命名：

  mv A B   //把目录Ａ改名为Ｂ
  
 mv [-fiu] source destination
 mv [options] source1 source2 source3 .... directory
 -f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；
 -i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！
 -u ：若目标文件已经存在，且 source 比较新，才会升级 (update)
 
```

**文件的删除：**

```
rm [-fir] 文件或目录
-f ：就是 force 的意思，忽略不存在的文件，不会出现警告信息；
-i ：互动模式，在删除前会询问使用者是否动作
-r ：递回删除！

rm  a.txt   /删除a.txt目录
rm -r  /opt/a   //删除a文件及其子目录
```

**文件内容查看：**

```
cat  由第一行开始显示文件内容
tac  从最后一行开始显示，可以看出 tac 是 cat 的倒著写！
nl   显示的时候，顺道输出行号！
more 一页一页的显示文件内容
less 与 more 类似，但是比 more 更好的是，他可以往前翻页！
head 只看头几行
tail 只看尾巴几行
```



```
[root@www ~]# pwd [-P]
选项与参数：
-P  ：显示出确实的路径，而非使用连结 (link) 路径。

[root@www ~]# ls [-aAdfFhilnrRSt] 目录名称
选项与参数：
-a ：全部的文件，连同隐藏档( 开头为 . 的文件) 一起列出来(常用)
-d ：仅列出目录本身，而不是列出目录内的文件数据(常用)
-l ：长数据串列出，包含文件的属性与权限等等数据；(常用)

```



##   六、Ubuntu 16.04查看软件安装位置


	1、find命令
	lee@lee:~$ sudo find / -name virtualbox
	/etc/init.d/virtualbox
	/etc/default/virtualbox
	/usr/share/unity-scopes/virtualbox
	/usr/share/virtualbox
	/usr/share/lintian/overrides/virtualbox
	/usr/share/doc/virtualbox
	/usr/lib/virtualbox
	/usr/bin/virtualbox
	find: ‘/run/user/1000/gvfs’: Permission denied
	/var/lib/dkms/virtualbox
	/home/lee/Documents/环境搭建/2-3-环境搭建-2/虚拟机/virtualbox-4.3_4.3.22-98236~Ubuntu~precise_amd64/usr/share/virtualbox
	/home/lee/Documents/环境搭建/2-3-环境搭建-2/虚拟机/virtualbox-4.3_4.3.22-98236~Ubuntu~precise_amd64/usr/lib/virtualbox
	/home/lee/Documents/环境搭建/2-3-环境搭建-2/虚拟机/virtualbox-4.3_4.3.22-98236~Ubuntu~precise_amd64/usr/bin/virtualbox
	 
	从中判断出执行文件安装位置为："/usr/bin/virtualbox"
	
	2、aptitude show packagename(可以看到packagename软件一系列信息)
	lee@lee:~$ aptitude show sublime-text-installer
	
	3、which命令（只在PAHT变量里面寻找可执行文件的位置；执行 which virtualbox输出 /usr/bin/virtualbox 这个一般是一个软连接.通过执行:ll /usr/bin/virtualbox）
	lee@lee:~$ which virtualbox
	/usr/bin/virtualbox
	lee@lee:~$ ll /usr/bin/virtualbox
	    lrwxrwxrwx 1 root root 27 5月   3  2017 /usr/bin/virtualbox -> ../share/virtualbox/VBox.sh*
	
	lee@lee:~$ dpkg -L virtualbox(对于有些软件的可执行文件路径，没有在PATH路径下保存，通过该命令寻找安装目录)
	{lee@lee:~$ dpkg -L  sublime-text-installer
	 /usr/bin/subl
	{lee@lee:~$ gedit /usr/bin/subl
	看到重定向：exec /opt/sublime_text/sublime_text "$@"
	可知sublime安装在了/opt/sublime_text目录下}
	
	4、dpkg -l 命令(软件使用apt-get install命令安装的,列出软件的安装信息)
	lee@lee:~$ dpkg -l virtualbox
	Desired=Unknown/Install/Remove/Purge/Hold
	| Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend
	|/ Err?=(none)/Reinst-required (Status,Err: uppercase=bad)
	||/ Name               Version        Architecture   Description
	+++-==================-==============-==============-=========================================
	ii  virtualbox         5.0.40-dfsg-0u amd64          x86 virtualization solution - base binari
	5、whereis命令
	lee@lee:~$ whereis virtualbox
	virtualbox: /usr/bin/virtualbox /usr/lib/virtualbox /usr/share/virtualbox /usr/share/man/man1/virtualbox.1.gz
	 
	6、type命令（查看执行文件的位置）
	 
	lee@lee:~$ type virtualbox
	virtualbox is /usr/bin/virtualbox
	
	7、locate命令（查找出所有匹配文件）
	 
	lee@lee:~$ locate virtualbox
	/etc/default/virtualbox
	/etc/init.d/virtualbox
	............

## 七、卸载命令

```
1.dpkg --list//浏览已安装的程序。要查看已安装的软件包列表，请输入以下命令。请注意你希望卸载的软件包的名称。

2.sudo apt-get --purge remove <programname>//卸载程序和所有配置文件。在终端中输入以下命令，把<programname>替换成你希望完全移除的程序。

3.sudo apt-get remove <programname> //只卸载程序。如果你移除程序但保留配置文件。

```

## 八、添加权限

身份

`u` — 拥有文件的用户（所有者）

`g` — 所有者所在的组群

`o` — 其他人（不是所有者或所有者的组群）

`a` — 每个人或全部（`u`、`g`、和 `o`）

权限

`r` — 读取权

`w` — 写入权

`x` — 执行权

行动

`+` — 添加权限

`-` — 删除权限

`=` — 使它成为唯一权限

常用例子：

- `g+w` — 为组群添加写入权
- `o-rwx` — 删除其它人的所有权限
- `u+x` — 允许文件所有者执行这个文件
- `a+rw` — 允许每个人读取并写入文件
- `ug+r` — 允许所有者和组群读取文件
- `g=rx` — 只允许组群读取和执行（不能写入）

通过添加 `-R` 选项，你可以为整个目录树改变权限。

因为你不能象执行程序一样地“执行”目录，当你为目录添加或删除执行权限时，你实际上是在允许（或拒绝）在目录中搜索的权限。

使用数字来改变权限

每种权限设置都可以用一个数值来代表：

- r = 4
- w = 2
- x = 1
- \- = 0

当这些值被加在一起，它的总和便用来设立特定的权限。譬如，如果你想有读取和写入的权限，你会得到一个值为 6 的总和；4（读取）+ 2（写入）= 6。

所有者的总和为六，组群的总和为六，其他人的总和为四。这个权限设置读作 `664`。

```
chmod 664 sneakers.txt
ls -l sneakers.txt
rw-r--r-- 1 test test 39 3月 11 12:04 sneakers.txt
```

这里是一个某些常用设置、数值、以及它们的含义的列表：

```

-rw------- (600) — 只有所有者才有读取和写入的权限。

-rw-r--r-- (644) — 只有所有者才有读取和写入的权限；组群和其他人只有读取的权限。

-rwx------ (700) — 只有所有者才有读取、写入、和执行的权限。

-rwxr-xr-x (755) — 所有者有读取、写入、和执行的权限；组群和其他人只有读取和执行的权限。

-rwx--x--x (711) — 所有者有读取、写入、和执行权限；组群和其他人只有执行权限。

-rw-rw-rw- (666) — 每个人都能够读取和写入文件。（请谨慎使用这些权限。）

-rwxrwxrwx (777) — 每个人都能够读取、写入、和执行。（再重申一次，这种权限设置可能会很危险。）

下面列举了一些对目录的常见设置：

drwx------ (700) — 只有所有者能在目录中读取、写入。

drwxr-xr-x (755) — 每个人都能够读取目录，但是其中的内容却只能被所有者改变。
```

**chgrp:更改文件属组**

```
chgrp [-R] 属组名文件名
-R：递归更改文件属组，就是在更改某个目录文件的属组时，如果加上-R的参数，那么该目录下的所有文件的属组都会更改。
```

**chown：更改文件属主，也可以同时更改文件属组**

```
chown [–R] 属主名 文件名
chown [-R] 属主名：属组名 文件名

[root@www ~]# chown lee install.log
[root@www ~]# ls -l
-rw-r--r--  1 lee  users 68495 Jun 25 08:53 install.log
[root@www ~]# chown root:root install.log
[root@www ~]# ls -l
-rw-r--r--  1 root root 68495 Jun 25 08:53 install.log
```

**chmod：更改文件9个属性**

```
[root@www ~]# ls -al .bashrc
-rwxr-xr-x  1 root root 395 Jul  4 11:45 .bashrc
[root@www ~]# chmod  a+w  .bashrc
[root@www ~]# ls -al .bashrc
-rwxrwxrwx  1 root root 395 Jul  4 11:45 .bashrc
[root@www ~]# chmod  a-x  .bashrc
[root@www ~]# ls -al .bashrc
-rw-rw-rw-  1 root root 395 Jul  4 11:45 .bashrc
```

在 more 这个程序的运行过程中，你有几个按键可以按的：

- 空白键 (space)：代表向下翻一页；
- Enter         ：代表向下翻『一行』；
- /字串         ：代表在这个显示的内容当中，向下搜寻『字串』这个关键字；
- :f            ：立刻显示出档名以及目前显示的行数；
- q             ：代表立刻离开 more ，不再显示该文件内容。
- b 或 [ctrl]-b ：代表往回翻页，不过这动作只对文件有用，对管线无用

less运行时可以输入的命令有：

- 空白键    ：向下翻动一页；
- [pagedown]：向下翻动一页；
- [pageup]  ：向上翻动一页；
- /字串     ：向下搜寻『字串』的功能；
- ?字串     ：向上搜寻『字串』的功能；
- n         ：重复前一个搜寻 (与 / 或 ? 有关！)
- N         ：反向的重复前一个搜寻 (与 / 或 ? 有关！)
- q         ：离开 less 这个程序；

## 九、cat 命令

```
cat [-AbEnTv]
-A ：相当於 -vET 的整合选项，可列出一些特殊字符而不是空白而已；
-b ：列出行号，仅针对非空白行做行号显示，空白行不标行号！
-E ：将结尾的断行字节 $ 显示出来；
-n ：列印出行号，连同空白行也会有行号，与 -b 的选项不同；
-T ：将 [tab] 按键以 ^I 显示出来；
-v ：列出一些看不出来的特殊字符
```

//给qunzu.sh由1开始对对所有行编号

cat -n qunzu.sh  

cat主要有三大功能：
1.一次显示整个文件。$ cat filename
2.从键盘创建一个文件。$ cat > filename  
  只能创建新文件,不能编辑已有文件.
3.将几个文件合并为一个文件： $cat file1 file2 > file

cat file1 file2 > file1:将file1中的内容被file2内容替换。

参数：
-n 或 --number 由 1 开始对所有输出的行数编号
-b 或 --number-nonblank 和 -n 相似，只不过对于空白行不编号
-s 或 --squeeze-blank 当遇到有连续两行以上的空白行，就代换为一行的空白行
-v 或 --show-nonprinting
例：
把 textfile1 的档案内容加上行号后输入 textfile2 这个档案里
cat -n textfile1 > textfile2

把 textfile1 和 textfile2 的档案内容加上行号（空白行不加）之后将内容附加到 textfile3 里。
cat -b textfile1 textfile2 >> textfile3

把[**test**](http://www.cnblogs.com/perfy/admin/).txt文件赋空值
cat /dev/null > ~/Downloads/test.txt  



```
1、追加文件
# cat << EOF >> test.sh  内容  EOF
---将内容追加到 test.sh 的后面，不会覆盖掉原有的内容
2、换一种写法
# cat > test.sh << EOF 内容  EOF（会覆盖掉原有的内容）
```



## 十、vi/vim命令

键盘图：

![img](https://img.w3cschool.cn/attachments/day_161011/201610111458038045.gif)

工作模式



![img](https://www.w3cschool.cn/attachments/uploads/2014/07/vim_model.png)



## 十一、grep 命令



```
-a 不要忽略二进制数据。
-A<显示列数> 除了显示符合范本样式的那一行之外，并显示该行之后的内容。
-b 在显示符合范本样式的那一行之外，并显示该行之前的内容。
-c 计算符合范本样式的列数。
-C<显示列数>或-<显示列数>  除了显示符合范本样式的那一列之外，并显示该列之前后的内容。
-d<进行动作> 当指定要查找的是目录而非文件时，必须使用这项参数，否则grep命令将回报信息并停止动作。
-e<范本样式> 指定字符串作为查找文件内容的范本样式。
-E 将范本样式为延伸的普通表示法来使用，意味着使用能使用扩展正则表达式。
-f<范本文件> 指定范本文件，其内容有一个或多个范本样式，让grep查找符合范本条件的文件内容，格式为每一列的范本样式。
-F 将范本样式视为固定字符串的列表。
-G 将范本样式视为普通的表示法来使用。
-h 在显示符合范本样式的那一列之前，不标示该列所属的文件名称。
-H 在显示符合范本样式的那一列之前，标示该列的文件名称。
-i 忽略字符大小写的差别。
-l 列出文件内容符合指定的范本样式的文件名称。
-L 列出文件内容不符合指定的范本样式的文件名称。
-n 在显示符合范本样式的那一列之前，标示出该列的编号。
-q 不显示任何信息。
-R/-r 此参数的效果和指定“-d recurse”参数相同。
-s 不显示错误信息。
-v 反转查找。
-w 只显示全字符合的列。
-x 只显示全列符合的列。
-y 此参数效果跟“-i”相同。
-o 只输出文件中匹配到的部分。

#显示匹配某个结果之后的3行
~$ seq 10|cat manifest_s.xml |grep -e "wifi" -A 3
#显示匹配某个结果之前的3行
~$ seq 10|cat manifest_s.xml |grep -e "wifi" -B 3
#显示匹配某个结果的前3行和后3行
~$ seq 10|cat manifest_s.xml |grep -e "wifi" -C 3
#如果匹配结果有多个，会用“--”作为各匹配结果之间的分隔符：
~$ echo -e "a\nb\nc\na\nb\nc" | grep a -A 1

#不会输出任何信息，如果命令运行成功返回0，失败则返回非0值。一般用于条件测试:
grep -q "wifi" filename

#使用0值字节后缀的grep与xargs;测试文件：
echo "aaa" > file1
echo "bbb" > file2
echo "aaa" > file3
#执行后会删除file1和file3，grep输出用-Z选项来指定以0值字节作为终结符文件名（\0），xargs -0 读取输入并用0值字节终结符分隔文件名，然后删除匹配文件，-Z通常和-l结合使用:
grep "aaa" file* -lZ | xargs -0 rm

#只在目录中所有的.php和.html文件中递归搜索字符"main()"
grep "main()" . -r --include *.{php,html}
#在搜索结果中排除所有README文件
grep "main()" . -r --exclude "README"
#在搜索结果中排除filelist文件列表里的文件
grep "main()" . -r --exclude-from filelist

grep -R --exclude-dir=node_modules 'some pattern' /path/to/search

# .表示当前目录:
grep "wifi" . -r -n
or
grep "wifi" ./ -r -n

#忽略匹配样式中的字符大小写：
echo "hello world" | grep -i "HELLO"

#选项 -e 制动多个匹配样式：
echo this is a text line | grep -e "is" -e "line" -o
is
line
or
#也可以使用-f选项来匹配多个样式，在样式文件中逐行写出需要匹配的字符。
cat patfile
aaa
bbb
echo aaa bbb ccc ddd eee | grep -f patfile -o

#在文件里搜索字符串，返回一个包含“match_pattern”的文本行：
grep match_pattern file_name
or
grep "match_pattern" file_name
#在多个文件中查找：
grep "match_pattern" file_1 file_2 file_3 ...
#输出除之外的所有行 -v 选项：
grep -v "match_pattern" file_name
#标记匹配颜色 --color=auto 选项：
grep "match_pattern" file_name --color=auto
#使用正则表达式 -E 选项：
grep -E "[1-9]+"
or
egrep "[1-9]+"
#只输出文件中匹配到的部分 -o 选项：
echo this is a test line. | grep -o -E "[a-z]+\."
line.
echo this is a test line. | egrep -o "[a-z]+\."
line.
#统计文件或者文本中包含匹配字符串的行数 -c 选项：
grep -c "text" file_name
#输出包含匹配字符串的行数 -n 选项：
grep "text" -n file_name
or
cat file_name | grep "text" -n
#多个文件
grep "text" -n file_1 file_2
#打印样式匹配所位于的字符或字节偏移：
(#一行中字符串的字符便宜是从该行的第一个字符开始计算，起始值为0。选项 -b -o 一般总是配合使用)
echo gun is not unix | grep -b -o "not"
7:not
#搜索多个文件并查找匹配文本在哪些文件中：
grep -l "text" file1 file2 file3...



#--line-buffered 打开buffering 模式，针对搜索动态文件（不断地添加信息到文件的尾部）：
[root@localhost ~]#tail -f file | grep --line-buffered your_pattern 


```

## 十二、ps命令

```
ps [选项]

下面对命令选项进行说明：

-e   显示所有进程。
-f    全格式。
-h   不显示标题。
-l    长格式。
-w  宽输出。
a    显示终端上的所有进程，包括其他用户的进程。
r    只显示正在运行的进程。
u 　以用户为主的格式来显示程序状况。
x     显示所有程序，不以终端机来区分。

ps -ef 显示出的结果：
    1.UID       用户ID
    2.PID        进程ID
    3.PPID      父进程ID
    4.C           CPU占用率
    5.STIME     开始时间
    6.TTY         开始此进程的TTY----终端设备
    7.TIME       此进程运行的总时间
    8.CMD       命令名

//查看运行的进程
ps -e | grep apt
ps -aux |grep "xxx"
```

## 十三、shell运算符

###### 算术运算符

下表列出了常用的算术运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符  | 说明                        | 举例                        |
| ---- | ------------------------- | ------------------------- |
| +    | 加法                        | `expr $a + $b` 结果为 30。    |
| -    | 减法                        | `expr $a - $b` 结果为 -10。   |
| *    | 乘法                        | `expr $a \* $b` 结果为  200。 |
| /    | 除法                        | `expr $b / $a` 结果为 2。     |
| %    | 取余                        | `expr $b % $a` 结果为 0。     |
| =    | 赋值                        | a=$b 将把变量 b 的值赋给 a。       |
| ==   | 相等。用于比较两个数字，相同则返回 true。   | [ $a == $b ] 返回 false。    |
| !=   | 不相等。用于比较两个数字，不相同则返回 true。 | [ $a != $b ] 返回 true。     |

```
注意：条件表达式要放在方括号之间，并且要有空格，例如: [$a==$b] 是错误的，必须写成 [ $a == $b ]。
```

###### 关系运算符

关系运算符只支持数字，不支持字符串，除非字符串的值是数字。

下表列出了常用的关系运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符  | 说明                               | 举例                      |
| ---- | -------------------------------- | ----------------------- |
| -eq  | 检测两个数是否相等，相等返回 true。             | [ $a -eq $b ] 返回 false。 |
| -ne  | 检测两个数是否相等，不相等返回 true。            | [ $a -ne $b ] 返回 true。  |
| -gt  | 检测左边的数是否大于右边的，如果是，则返回 true。      | [ $a -gt $b ] 返回 false。 |
| -lt  | 检测左边的数是否小于右边的，如果是，则返回 true。关系运算符 | [ $a -lt $b ] 返回 true。  |
| -ge  | 检测左边的数是否大于等于右边的，如果是，则返回 true。    | [ $a -ge $b ] 返回 false。 |
| -le  | 检测左边的数是否小于等于右边的，如果是，则返回 true。    | [ $a -le $b ] 返回 true。  |

###### 布尔运算符

下表列出了常用的布尔运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符  | 说明                                 | 举例                                    |
| ---- | ---------------------------------- | ------------------------------------- |
| !    | 非运算，表达式为 true 则返回 false，否则返回 true。 | [ ! false ] 返回 true。                  |
| -o   | 或运算，有一个表达式为 true 则返回 true。         | [ $a -lt 20 -o $b -gt 100 ] 返回 true。  |
| -a   | 与运算，两个表达式都为 true 才返回 true。         | [ $a -lt 20 -a $b -gt 100 ] 返回 false。 |

###### 逻辑运算符

以下介绍 Shell 的逻辑运算符，假定变量 a 为 10，变量 b 为 20:

| 运算符  | 说明      | 举例                                      |
| ---- | ------- | --------------------------------------- |
| &&   | 逻辑的 AND | [[ $a -lt 100 && $b -gt 100 ]] 返回 false |
| \|\| | 逻辑的 OR  | [[ $a -lt 100 || $b -gt 100 ]] 返回 true  |

###### 字符串运算符

下表列出了常用的字符串运算符，假定变量 a 为 "abc"，变量 b 为 "efg"：

| 运算符  | 说明                      | 举例                    |
| ---- | ----------------------- | --------------------- |
| =    | 检测两个字符串是否相等，相等返回 true。  | [ $a = $b ] 返回 false。 |
| !=   | 检测两个字符串是否相等，不相等返回 true。 | [ $a != $b ] 返回 true。 |
| -z   | 检测字符串长度是否为0，为0返回 true。  | [ -z $a ] 返回 false。   |
| -n   | 检测字符串长度是否为0，不为0返回 true。 | [ -n $a ] 返回 true。    |
| str  | 检测字符串是否为空，不为空返回 true。   | [ $a ] 返回 true。       |

###### 文件测试运算符

文件测试运算符用于检测 Unix 文件的各种属性，描述如下：

| 操作符     | 说明                                       | 举例                     |
| ------- | ---------------------------------------- | ---------------------- |
| -b file | 检测文件是否是块设备文件，如果是，则返回 true。               | [ -b $file ] 返回 false。 |
| -c file | 检测文件是否是字符设备文件，如果是，则返回 true。              | [ -c $file ] 返回 false。 |
| -d file | 检测文件是否是目录，如果是，则返回 true。                  | [ -d $file ] 返回 false。 |
| -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 | [ -f $file ] 返回 true。  |
| -g file | 检测文件是否设置了 SGID 位，如果是，则返回 true。           | [ -g $file ] 返回 false。 |
| -k file | 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。   | [ -k $file ] 返回 false。 |
| -p file | 检测文件是否是有名管道，如果是，则返回 true。                | [ -p $file ] 返回 false。 |
| -u file | 检测文件是否设置了 SUID 位，如果是，则返回 true。           | [ -u $file ] 返回 false。 |
| -r file | 检测文件是否可读，如果是，则返回 true。                   | [ -r $file ] 返回 true。  |
| -w file | 检测文件是否可写，如果是，则返回 true。                   | [ -w $file ] 返回 true。  |
| -x file | 检测文件是否可执行，如果是，则返回 true。                  | [ -x $file ] 返回 true。  |
| -s file | 检测文件是否为空（文件大小是否大于0），不为空返回 true。          | [ -s $file ] 返回 true。  |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回 true。             | [ -e $file ] 返回 true。  |

**实例**

变量 file 表示文件"/var/www/w3cschool/test.sh"，它的大小为100字节，具有 rwx 权限。下面的代码，将检测该文件的各种属性：

```
#!/bin/bash
file="/var/www/w3cschool/test.sh"
if [ -r $file ]
then
   echo "文件可读"
else
   echo "文件不可读"
fi
if [ -w $file ]
then
   echo "文件可写"
else
   echo "文件不可写"
fi
if [ -x $file ]
then
   echo "文件可执行"
else
   echo "文件不可执行"
fi
if [ -f $file ]
then
   echo "文件为普通文件"
else
   echo "文件为特殊文件"
fi
if [ -d $file ]
then
   echo "文件是个目录"
else
   echo "文件不是个目录"
fi
if [ -s $file ]
then
   echo "文件不为空"
else
   echo "文件为空"
fi
if [ -e $file ]
then
   echo "文件存在"
else
   echo "文件不存在"
fi
```

执行脚本，输出结果如下所示：

```
文件可读
文件可写
文件可执行
文件为普通文件
文件不是个目录
文件不为空
文件存在
```

###### **特殊运算符：**

| 参数处理 | 说明                                       |
| ---- | ---------------------------------------- |
| $#   | 传递到脚本的参数个数                               |
| $*   | 以一个单字符串显示所有向脚本传递的参数。如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。 |
| $$   | 脚本运行的当前进程ID号                             |
| $!   | 后台运行的最后一个进程的ID号                          |
| $@   | 与$*相同，但是使用时加引号，并在引号中返回每个参数。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。 |
| $-   | 显示Shell使用的当前选项，与[set命令](https://www.w3cschool.cn/linux/linux-comm-set.html)功能相同。 |
| $?   | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。          |

```
注意：$10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数。

shell 函数：
1、可以带function fun() 定义，也可以直接fun() 定义,不带任何参数。
2、参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。 return后跟数值n(0-255）。

```

###### shell输入输出重定向：

| 命令              | 说明                             |
| --------------- | ------------------------------ |
| command > file  | 将输出重定向到 file。                  |
| command < file  | 将输入重定向到 file。                  |
| command >> file | 将输出以追加的方式重定向到 file。            |
| n > file        | 将文件描述符为 n 的文件重定向到 file。        |
| n >> file       | 将文件描述符为 n 的文件以追加的方式重定向到 file。  |
| n >& m          | 将输出文件 m 和 n 合并。                |
| n <& m          | 将输入文件 m 和 n 合并。                |
| << tag          | 将开始标记 tag 和结束标记 tag 之间的内容作为输入。 |

```
需要注意的是文件描述符 0 通常是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。
2>&1 表示将标准错误重定向到标准输出 。
标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。
command 2 > file  //将stderr 重定向到file.
command 2 >> file  //将stderr追加到file文件末尾.
command >file 2>&1  or command >> file 2>&1//将 stdout 和 stderr 合并后重定向到 file.
command < file1 >file2  //对 stdin 和 stdout 都重定向,将 stdin 重定向到 file1，将 stdout 重定向到 file2.

Here Document 是 Shell 中的一种特殊的重定向方式，用来将输入重定向到一个交互式 Shell 脚本或程序。

command > /dev/null //如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null.
(/dev/null 是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到。但是 /dev/null 文件非常有用，将命令的输出重定向到它，会起到"禁止输出"的效果。)

command > /dev/null 2>&1  //屏蔽 stdout 和 stderr
```

wc命令

```
wc命令用来计算数字。利用wc指令我们可以计算文件的Byte数、字数或是列数，若不指定文件名称，或是所给予的文件名为“-”，则wc指令会从标准输入设备读取数据。
wc(选项)(参数)
（选项）-c或--bytes或——chars：只显示Bytes数；
-l或——lines：只显示列数；
-w或——words：只显示字数。
（参数）文件：需要统计的文件列表。
lee@lee:~/Downloads$ wc -l use
23 use
lee@lee:~/Downloads$ wc -l <use
23
第一个例子，会输出文件名；第二个不会，因为它仅仅知道从标准输入读取内容。

command1 < infile > outfile
同时替换输入和输出，执行command1，从文件infile读取内容，然后将输出写入到outfile中。
```



##  tee command

```
功能说明：读取标准输入的数据，并将其内容输出成文件。
语　　法：tee [-ai][--help][--version][文件...]
补充说明：tee指令会从标准输入设备读取数据，将其内容输出到标准输出设备，同时保存成文件。
参　　数：
　-a或--append 　附加到既有文件的后面，而非覆盖它．
　-i-i或--ignore-interrupts 　忽略中断信号。
　--help 　在线帮助。
　--version 　显示版本信息。
```

Example:

```

lee@lee:~/Downloads$ who |tee who.out
lee tty7         2018-01-13 17:18 (:0)
lee@lee:~/Downloads$ cat who.out
lee tty7         2018-01-13 17:18 (:0)
lee@lee:~/Downloads$ pwd |tee -a who.out
/home/lee/Downloads
lee@lee:~/Downloads$ cat who.out
lidongxue tty7         2018-01-13 17:18 (:0)
/home/lidongxue/Downloads
lee@lee:~/Downloads$ 
lee@lee:~/Downloads$ la |tee out.txt |cat -n
lee@lee:~/Downloads$ ls | tee -    //输出到标准输出两次,同时保存到-中。
lee@lee:~/Downloads$ ls | tee file -   //输出到标准输出两次，同时保存到file1和-中。
lee@lee:~/Downloads$ ls | tee file1 file2 -  //输出到标准输出两次，同时保存到file1和file2和-中。
lee@lee:~/Downloads$ rm - file
```

## 十四、touch命令

**touch命令**有两个功能：一是用于把已存在文件的时间标签更新为系统当前的时间（默认方式），它们的数据将原封不动地保留下来；二是用来创建新的空文件。

参数：

```
-a：或--time=atime或--time=access或--time=use  只更改存取时间；
-c：或--no-create  不建立任何文件；
-d：<时间日期> 使用指定的日期时间，而非现在的时间；
-f：此参数将忽略不予处理，仅负责解决BSD版本touch指令的兼容性问题；
-m：或--time=mtime或--time=modify  只更该变动时间；
-r：<参考文件或目录>  把指定文件或目录的日期时间，统统设成和参考文件或目录的日期时间相同；
-t：<日期时间>  使用指定的日期时间，而非现在的时间；
--help：在线帮助；
--version：显示版本信息。
```
十五、shell命令

```
-e filename 如果 filename存在，则为真
-d filename 如果 filename为目录，则为真 
-f filename 如果 filename为常规文件，则为真
-L filename 如果 filename为符号链接，则为真
-r filename 如果 filename可读，则为真 
-w filename 如果 filename可写，则为真 
-x filename 如果 filename可执行，则为真
-s filename 如果文件长度不为0，则为真
-h filename 如果文件是软链接，则为真

if (!-e "../script/clone.sh")  #clone.sh不存在时为true。

```

十五、echo命令

```

window的bat脚本
用@echo off 就能关闭echo命令的输入显示
rem和::都起到注释的作用 

linux的shell脚本
stty -echo ：关闭回显
stty echo  ：开启回显

```

