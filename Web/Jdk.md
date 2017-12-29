### Ubuntu 16.04 安装 JDK 和 Open Jdk

------

Ubuntu 16.04 安装 JDK 和 Open Jdk默认JDK 安装相对比较轻松，但如果想调整 OpenSdk 版本可就有点儿麻烦， 特别是 OpenJdk7 。常规安装 见这里 [java _install](http://openjdk.java.net/install/)

**默认JDK 安装：**

```
sudo apt-get update
sudo apt-get install default-jre
```

**Oracle JDK 安装：**

**设置 PPA**

```
 sudo add-apt-repository ppa:webupd8team/java
 sudo apt-get update 
```

**JDK6 ：**

```
 sudo apt-get install oracle-java6-installer
```

**JDK 7：**

```
  sudo apt-get install oracle-java7-installer
```

**JDK 8：**

```
sudo apt-get install oracle-java8-installer
```

**OpenJdk 7安装：**

```
sudo add-apt-repository ppa:openjdk-r/ppa  
sudo apt-get update
sudo apt-get install openjdk-7-jdk  
```

**版本之间切换**

```
update-alternatives --config java //查看java版本,选择对应版本
update-alternatives --config javac
```

**更新open JDK1.6&1.7&1.8**

	1)安装JDK1.6  
		 a. sudo cp     #文件路径/jdk-6u27-linux-x64.bin /opt
	    b. cd /opt     #进入opt文件下
	    c. sudo chmod a+x jdk-6u27-linux-x64.bin      #给bin文件添加可执行权限
	    d. sudo ./jdk-6u27-linux-x64.bin        #运行安装
	2)更新open JDK1.7
		a. sudo mkdir -p /mtkoss/jdk/1.6.0_45-ubuntu-10.04/x86_64/   	（请查看KK工程根目录下的mbldenv.sh）
		b. cd /mtkoss/jdk/1.6.0_45-ubuntu-10.04/x86_64/
		c. sudo ln -s /opt/jdk1.6.0_27/*  ./          
		d. sudo mkdir -p /mtkoss/jdk/jdk1.6.0_23/	（请查看JB工程根目录下的mbldenv.sh）
		e. cd /mtkoss/jdk/jdk1.6.0_23/
		f. sudo ln -s /opt/jdk1.6.0_27/*  ./       //将/opt/jdk1.6.0_27/下所有内容与当前目录建立链接（相当于快捷方式）
		g. sudo apt-get update（联网状态）
		h. sudo apt-get install openjdk-7-jdk（联网状态）
		i. 重启
		j. java -version
		   显示：java version "1.7.0_65"
			OpenJDK Runtime Environment (IcedTea 2.5.3) (7u71-2.5.3-0ubuntu0.12.04.1)
			OpenJDK 64-Bit Server VM (build 24.65-b04, mixed mode)
		k.如何测试安装成功？
			编译JB，KK，82L代码看是否会报错(支持mm模块编译)
	3)更新openJDK 1.8
		1. sudo add-apt-repository ppa:openjdk-r/ppa
		2. sudo apt-get update
		3. sudo apt-get install openjdk-8-jdk
		4. sudo update-alternatives --config java	//查看java版本,选择对应版本
		   sudo update-alternatives --config javac	//查看javac版本,选择对应版本
&安装OpenJDK，两个都要装的(JB代码需求1.6，KK/L/M代码需求1.7，N代码需求1.8)

        5.1 安装jdk1.8：
            16.04默认集成了jdk1.8的源，可直接执行以下安装；
            sudo apt-get install openjdk-8-jdk
    
        5.2 安装jdk1.7：
            因为Ubuntu16.04的安装源已经默认没有openjdk7了，所以要自己手动添加仓库，如果严格按照3中的步骤可以直接执行第三条安装命令进行安装；
            sudo add-apt-repository ppa:openjdk-r/ppa     #添加源
            sudo apt-get update                           #更新源
            sudo apt-get install openjdk-7-jdk            #安装OpenJDK1.7
            注意，安装JDK1.7的源比较不稳定，如果失败的话请再试一次...
    
        5.3 安装jdk1.6：
            这部分是12.04装机文档上的步骤，因时间紧张，本人暂时没去尝试，理论上应该还是有效的，后期如果有问题欢迎补充；
            a. sudo cp     #文件路径/jdk-6u27-linux-x64.bin /opt
            b. cd /opt     #进入opt文件下
            c. sudo chmod a+x jdk-6u27-linux-x64.bin      #给bin文件添加可执行权限
            d. sudo ./jdk-6u27-linux-x64.bin              #运行安装
【手动安装jdk  配置 /etc/profile

```
sudo gedit /etc/profile；source /etc/profile

{export JAVA_HOME=/opt/Java/java版本号
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin

```

}】