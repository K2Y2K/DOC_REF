

## Linux下安装QT

```
Qt官网http://www.qt.io/download/,像下载地址：
1. 所有Qt版本下载地址：
http://download.qt.io/archive/qt/
2. 所有Qt Creator下载地址：
http://download.qt.io/archive/qtcreator/
3. 所有Qt VS开发插件下载地址:
http://download.qt.io/archive/vsaddin/
4. Qt相关下载大全
http://download.qt.io/
```

```

Ubuntu系统下安装	QT
方法一：终端安装（待验证）
sudo apt-get install qt4-dev-tools #开发包
sudo apt-get install qtcreator #IDE
sudo apt-get install qt4-doc #开发帮助文档
sudo apt-get install qt4-qtconfig #配置工具
sudo apt-get install qt4-demos #DEMO源码

方法二：从Qt官方网站下载.run格式的安装包，Windows下直接双击安装，Linux下进入安装包所在目录后用 ./ 安装。
1、下载完，添加可执行权限 
chmod +x qt-opensource-linux-x64-5.8.0.run （qt框架库，自带qt-creator工具）
（注：chmod a+x qt-creator-linux-x86_64-opensource-2.5.2.bin（qt集成环境开发工具，安了这个需要再下载qt库））
2、然后输入以下命令安装bin文件：
./qt-creator-linux-x86_64-opensource-2.5.2.bin
3、设置环境变量
sudo gedit /etc/profile
在文件末尾添加以下代码，
# add path Qt create
export QTDIR=/home/lee/SoftPackages/QT/Qt5.11.0/Tools/QtCreator 
export PATH=$QTDIR/bin:$PATH 
export LD_LIBRARY_PATH=$QTDIR/lib:$LD_LIBRARY_PAT
然后在执行一下：source /etc/profile 重新执行刚修改的初始化文件，使之立即生效。
注意：
 #Ubuntu下运行QT出现cannot find -lGL 运行以下任意一个命令都可以
{sudo apt-get install libqt4-dev   
sudo apt-get install libgl1-mesa-dev    
sudo apt-get libgl1-mesa-dev  
sudo apt-get libglu1-mesa-dev    
}
常用快捷键：
1）帮助文件：F1 （光标在函数名字或类名上，按 F1 即可跳转到对应帮助文档，查看其详细用法）
2）.h 文件和对应.cpp 文件切换：F4
3）编译运行：Ctrl + R
4）函数声明和定义切换：F2
5）代码注释取消注释：Ctrl + / （选中代码再按快捷键）
6）字体变大变小：Ctrl + 鼠标滚轮向上向下


```

## Linux下编译qt程序

```
1、在Linux下的命令行编辑程序：
[root@localhost root]# mkdir hello
//mkdir命令创建一个hello目录
[root@localhost root]# cd hello
//cd命令切换到刚才创建的hello目录
[root@localhost hello]# vim main.cpp
//在hello目录中用vi创建一个main.cpp文件  将下面的代码输入到main.cpp文件中
#include <QApplication>
#include <QLabel>
int main(int argc,char *argv[])
{       
QApplication app(argc,argv);       
QLabel *label = new QLabel(“Hello Qt”);       
label->show();       
return  app.exec();
}
或
#include <QtGui>
#include <QApplication>
#include <QLabel>
int main(int argc,char *argv[])
{       
QApplication app(argc,argv);       
QDialog *dd=new QDialog();
QLabel *label = new QLabel(dd);       
label->setText("hello world");
dd->show();       
return  app.exec();
}
2、然后在命令行编译程序：
[root@localhost hello]# qmake –project
//执行qmake –project,因为目录是hello,因此在hello目录下生成一个与平台无关的项目文件hello.pro，
[root@localhost hello]# qmake hello.pro
 //执行qmake hello.pro项目文件后，在hello目录下生成一个与平台有关的Makefile文件。
[root@localhost hello]# make
 //执行make进行编译源代码，并生成main.o目标文件及hello执行文件。
[root@localhost hello]# ./hello
//执行hello，就会弹出Hello Qt窗口，到此说明成功了。

```

