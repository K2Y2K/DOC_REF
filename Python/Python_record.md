Python_record

## 一、Introduction

Python官网:<https://www.python.org/>

beautifulsoup4:https://www.crummy.com/software/BeautifulSoup/

beautifulsoup4 doc:https://www.crummy.com/software/BeautifulSoup/bs4/doc/

pypi:https://pypi.org/

## 二、problem for install

**Problem :**

**Description:**

**Solution:**

### 1、Missing parentheses in call to 'print'

**Problem :**

 python代码：

```
print "www.baidu.com"
```

报错：

```
SyntaxError: Missing parentheses in call to 'print'
```

**Description:**

**Solution:**

python版本2和3，python2系列可以支持 print “xxxx” ，python系列需要使用print("xxx")

### 2、python 3.5.2报错：No module named 'urllib2' 解决方法

**Problem :**

python代码：

```
import urllib2  
response = urllib2.urlopen('http://www.baidu.com/')  
html = response.read()  
print html 
```

报错：

```
Traceback (most recent call last):
  File "test_urllib2.py", line 2, in <module>
    import urllib2
ImportError: No module named 'urllib2'
```

**Description:**

在python3里面，用urllib.request代替urllib2，另外python3之后，不能再用`print html`  ，而是用print ()。

**Solution:**

代码转换成：

```
import urllib.request
resp=urllib.request.urlopen('http://www.baidu.com')
html=resp.read()
print(html)
```

### 3、python3错误 之NameError: name 'cookielib' is not defined

**Problem :**

python代码：

```
# coding:utf8
import urllib.request
import cookielib
url="http://www.baidu.com"

cj=cookielib.CookieJar()
opener =urllib.request.build_opener(urllib.request.HTTPCookieProcessor(cj))
urllib.request.install_opener(opener)
response3=urllib.request.urlopen(url)
print(response3.getcode())
print(cj)
print(response3.read())

```

报错：

```
Traceback (most recent call last):
  File "u.py", line 3, in <module>
    import cookielib
ImportError: No module named 'cookielib'
or
Traceback (most recent call last):
  File "u.py", line 18, in <module>
    cj=cookielib.CookieJar()
NameError: name 'cookielib' is not defined
```

**Description:**

**Solution:**

代码转换成：

```
import cookielib------>import http.cookiejar
cj=cookielib.CookieJar()------>cj=http.cookiejar.CookieJar()
```

### 4、Python2问题：SyntaxError: Non-ASCII character '\xe6' in file 

**Problem :**

```
 File "bs.py", line 27
SyntaxError: Non-ASCII character '\xe6' in file bs.py on line 27, but no encoding declared; see http://python.org/dev/peps/pep-0263/ for details

```

**Description:**

**Solution:**

在文件头追加：

```
# -*- coding: cp936 -*-
or
# -*- coding: utf-8 -*  
Or 
# coding:utf8 
```

若执行`./bs.py`需要在文件头加入`#!/usr/bin/python`。