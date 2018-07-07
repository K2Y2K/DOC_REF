## shell&bat

### shell之pause功能

1.创建文件pause.sh

```
#!/bin/bash  
get_char()  
{  
  SAVEDSTTY=`stty -g`  
  stty -echo  
  stty raw  
  dd if=/dev/tty bs=1 count=1 2> /dev/null  
  stty -raw  
  stty echo  
  stty $SAVEDSTTY  
}   
if [ -z "$1" ]; then  
    echo '请按任意键继续...'  
else  
    echo -e "$1"  
fi   
get_char  
```

2.修改权限

```
chmod 0755 pause.sh
```

3.执行脚本

```
sh pause.sh
```



```
#!/bin/bash
echo 按任意键继续
read -n 1
echo 继续运行
read -p "按回车键继续"
```



```

window的bat脚本
用@echo off 就能关闭echo命令的输入显示
rem和::都起到注释的作用 

linux的shell脚本
stty -echo ：关闭回显
stty echo  ：开启回显
```

