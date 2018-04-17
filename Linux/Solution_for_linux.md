# **Solution_for_linux**

**Problem :**

**Description:**

**Solution:**

### 1、apt-get install 报错

**Problem :** dpkg: error processing samba4 (--configure) ; E: Sub-process /usr/bin/dpkg returned an error code (1)错误

**Description:**

应该是我的dpkg有问题，源于上一次的apt-get upgrade中途被人工阻断；
解决的办法是删掉/var/lib/dpkg/info这个文件夹并重新创建它。

**Solution:**

```
1.$ sudo mv /var/lib/dpkg/info /var/lib/dpkg/info_old //现将info文件夹更名
2.$ sudo mkdir /var/lib/dpkg/info //再新建一个新的info文件夹
3.$ sudo apt-get update -f ; sudo apt-get -f install //不用解释了吧
4.$ sudo mv /var/lib/dpkg/info/* /var/lib/dpkg/info_old //执行完上一步操作后会在新的info文件夹下生成一些文件，现将这些文件全部移到info_old文件夹下
5.$ sudo rm -rf /var/lib/dpkg/info //把自己新建的info文件夹删掉
6.$ sudo mv /var/lib/dpkg/info_old /var/lib/dpkg/info //把以前的info文件夹重新改
```


