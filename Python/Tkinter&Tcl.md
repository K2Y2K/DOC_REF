# Tkinter&Tcl

## Tcl

#### 1、tcl小程序

step1：创建.tcl文件

```
$ vim hello.tcl
```

step2：编辑.tcl文件

```
#!/usr/bin/wish
grid [ttk::button .mybutton -text "hello"]
```

step3：执行

```
$ wish hello.tcl
```



## Tkinter

配置环境

```
$ sudo apt install python-tk
```

测试小程序：

```
#!/usr/bin/python
#-*-coding:utf-8 -*-  #允许中文输入
import Tkinter as tk 
import random
window = tk.Tk() #创建窗口对象
window.geometry('200x150') #设置窗口个大小
window.title(u"随机测试")
window.resizable(False,False) #窗口大小不可改变
def randomNoun():
    nouns = ["cats", "hippos", "cakes"]
    noun = random.choice(nouns)
    return noun

def randomVerb():
    verbs = ["eats", "likes", "hates", "has"]
    verb = random.choice(verbs)
    return verb

def buttonClick():
    name = nameEntry.get()
    verb = randomVerb()
    noun = randomNoun()
    sentence = name + " " + verb + " " + noun
    result.delete(0, tk.END)
    result.insert(0, sentence)

nameLabel = tk.Label(window, text="Name:")
nameEntry = tk.Entry(window)
button = tk.Button(window, text="Generate", command=buttonClick)
result = tk.Entry(window)

nameLabel.pack() #pack()快管理
nameEntry.pack()
button.pack()
result.pack()
window.mainloop() # 进入消息循环
```

