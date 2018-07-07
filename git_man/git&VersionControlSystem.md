# 一、git基本操作

# 1. git与版本控制系统

- git：分布式版本控制系统。
- svn：集中式版本控制系统。

无论是分布式还是集中式版本控制系统，都只能对纯文本文件进行版本控制，而对二进制文件（如MS Word、MS Excel文档等）却都是无能为力的。

注意一点：文本文件必须统一使用utf-8格式编码，千万不要使用gbk编码！

# 2. 安装git

## 1. Linux环境（以Ubuntu为例）

### (1) 查看当前有没有安装git

`git`

### (2) 安装git

`sudo apt-get install git`

### (3) 查看git版本

`git --version`

### (4) 查看git帮助文档

有两种方法：

`git`

`git --help`

## 2. Windows环境

到 [https://git-for-windows.github.io](https://git-for-windows.github.io/) 上下载EXE安装包安装，安装完成后会有一个git bash命令行，然后在git bash命令行中其他操作和Linux下一致。

# 3. 配置git

## 1. 全局配置：

`git config --global user.name "your_name"`

`git config --global user.email "your_email@example.com"`

## 2. 在当前目录下初始化一个git版本库

`git init`

创建成功后，在当前目录下使用`ls -al`命令，可以看到创建了一个新的隐藏目录：.git，这就是git的版本库，注意不要手动修改其中的任何内容！

# 4. git工作区和暂存区、分支的关系

初始化成功一个git版本库后，会自动创建一个默认版本分支：master，以及一个暂存区（stage）。那么工作区（即用`git init`命令初始化后的硬盘文件夹）、暂存区、分支三者之间的关系是什么呢？搞清楚这一点对后面学习git的很多用法都非常重要，一图胜千言，见下图：
![这里写图片描述](http://img.blog.csdn.net/20170426225751797?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZG54Ymp5ag==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

# 5. git 常用操作

准备：假如当前目录位于learngit文件夹，是一个空文件夹，首先在learngit目录初始化一个git版本库：

`git init`

这时发现在learngit文件夹下新建了一个隐藏目录：.git，然后在learngit目录下新建一个文本文件：readme.txt，并向其中任意添加一些内容。

## 1. 将readme.txt文件添加到版本库暂存区

`git add readme.txt`

附：`git add`的其他用法：

- 添加工作区的所有修改（包括新建、修改和删除文件这三种修改）：`git add -A`
- 添加工作区中新建和修改文件的改动到暂存区，但不包括删除文件的改动：`git add *`或 `git add .`
- 添加工作区修改和删除文件的改动到暂存区，但不包括新建文件的改动：`git add -u`
- 撤销单个或多个文件的add操作：`git reset 文件名1 文件名2...`
- 撤销当前所有add到暂存区的操作：`git reset`

## 2. 删除文件

`git rm 文件名1 文件名2...`

## 3. 从暂存区提交修改（包括`git add`和`git rm`操作）到主分支

`git commit -m "create a new file readme.txt"`

注：`git commit`操作只会提交已经add到暂存区的修改，而工作区还未被add进暂存区的修改是不会被提交的。

## 4. 查看工作区状态

`git status`

## 5. 查看工作区和当前版本库最新版本之间的差别

`git diff HEAD [-- 文件名1 文件名2...]`

## 6. 撤销暂存区的修改（包括git add和git rm操作）

`git reset HEAD [文件名1 文件名2...]`

## 7. 查看commit操作的历史记录

`git log [--pretty=oneline]`

注：`--pretty=oneline`参数是为了在一行显示一条历史记录。

## 8. HEAD的理解

HEAD其实相当于一个指针，它指向的版本号就是当前版本库的最新版本。

- HEAD：当前版本
- HEAD^ ：上一个版本
- HEAD^^ ：上上个版本
- ......
- HEAD~100：往前100个版本

HEAD的指针作用可以用如下示意图表示：
![这里写图片描述](http://img.blog.csdn.net/20170426230834333?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZG54Ymp5ag==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

从上图也可以看出，HEAD指针可以指向不同的版本，而这也正是下面要讲的版本回退和切换的原理。

## 9. 回退到某一个版本

- 回退到上一个版本：`git reset --hard HEAD^`
- 回退到某个版本：`git reset --hard 版本号`

注：版本号可通过`git log`命令查看，只需要写前几位即可，git会自动识别匹配。

## 10. 回到未来

假如回退到之前的某个版本后，又后悔了不想回退了，想要撤销回退（即想要回到回退前的版本），可以使用如下命令：

- 先查看commit和reset命令的所有操作历史记录：`git reflog`
- 找到想要回到的未来的某个版本号，回到未来：`git reset --hard 版本号`



# 二、git远程仓库

# 1. 添加本地SSH Key到GitHub

要向GitHub远程仓库推送代码之前，需要做一个认证，即需要让GitHub知道向它推送代码的电脑是一个可以信赖的电脑。这就需要往GitHub上添加一个可以标示出本地电脑的SSH Key，然后才能往GitHub上推送代码。

## 1. 在本地生成一个SSH Key公钥和私钥

在任意目录下执行下面这条命令，执行完之后一路按回车即可：

`ssh-keygen -t rsa -C "yourgit@test.com"`

注：["yourgit@test.com"即为注册的GitHub账号邮箱地址]()。如果要在Windows下执行这个命令，必须在Git Bash窗口中执行，在Windows自带的cmd窗口中是没有这个命令的。

## 2. 切换到本地用户主目录：`cd ~`

此时在～目录用`ls -al`命令可以看到 一个.ssh隐藏目录

在.ssh目录中有两个密钥文件：id_rsa（私钥）、id_rsa.pub（公钥）,用vim或gedit打开公钥文件，复制其中的文本内容。
注意：原模原样复制，不要加任何多余的空格或空行。

## 3. 在GitHub创建SSH Key

用第1步中的GitHub账号从浏览器登录GitHub，点击右上角——>settings——>SSH and GPG Keys页面，点击创建一个SSH Key，标题可以随意写，将刚刚复制的公钥内容复制到"key"文本框中，并点击"Add SSH Key“按钮，这样就添加好了SSH Key。同一台电脑往一个GitHub账号只需添加一个SSH Key即可。

注：

- （1）允许添加多个SSH Key到同一个GitHub账号，这样就允许比如说我在家写了代码想推送到自己的GitHub账号上，同时也允许我在公司写完代码也可以推送到我的GitHub账号上去（如果公司允许的话）。
- （2）在GitHub上托管的公开代码仓库任何人都可以看到内容，但不可做修改（只有添加了SSH Key才可以做修改）。
- （3）要想不让别人看到自己的GitHub仓库的内容，有2种方法：第一种是向GitHub付费申请私有仓库，第二种是自己搭建一个git服务器。对于第一种，我的理解是GitHub本来就是一个倡导开源的组织，要想免费使用它的平台就要贡献出来一些东西（即开源自己的代码），如果不想开源那就要收费了（因为你没有贡献出来什么东西给大家）。

# 2. 提交本地代码到远程库

假如本地已经有了一个git仓库：learngit，现在想要把这个仓库的代码推送到GitHub上去，步骤如下：

## 1. 在GitHub上创建一个远程库

点击页面右上角的一个"+"按钮，创建一个空的远程仓库：learngit。

## 2. 建立本地库与远程仓库的联系

在本地learngit仓库目录下执行命令：

`git remote add origin git@github.com:xxx/learngit.git`

注：这是用SSH的方式进行推送，其中origin为默认的远程仓库名，xxx为你的邮箱账号名（@之前的字符串），learngit为刚刚第1步创建的远程仓库名。除了SSH的推送方式外，还有HTTPS的方式，但SSH的方式速度更快。最后的'.git'是一定要带上的。

补充：

查看本地仓库和远程仓库的联系信息：`git remote -v`

删除本地库与远程库的联系：`git remote remove origin`

## 3.拉远程仓库代码到本地

如果远程仓库创建了README.md等文件，那么先要把远程的文件拉到本地，否则下一步push的时候不会成功：

`git pull origin master:master`

注：此命令表明把远程的master分支拉到本地的master分支，前一个master指的是远程的master分支。

## 4. 推送代码

建立和远程仓库的联系后，就可以将本地仓库当前分支的代码推送到远程仓库了。将本地仓库的master分支的内容（默认分支为master）推送到远程库对应的分支上（master分支）：

`git push -u origin master`

注：首次推送要加`-u`参数，后面再推送就可以不加了，因为这第一次已经建立了和远程仓库的追踪链接，所以可以直接用：`git push origin master`

# 3. 从远程仓库克隆到本地

## 1. 创建远程仓库

在github上创建一个远程仓库：gitskills，并在创建时候顺便初始化一个readme.md文件
注：在创建远程仓库的时候，同时也可以创建.gitignore文件（有现成的各种语言的.gitignore文件可以选），也可以创建license文件。

## 2. 克隆远程库到本地

在本地任一目录下执行：

`git clone git@github.com:xxx/gitskills.git`

## 3. 成功克隆远程代码到本地

此时发现当前目录下多了一个gitskills目录，进去之后就可以看到第1步中创建的readme.md文件了

## 4. 其他下载方式

git远程库的其他下载方式：https，zip压缩包，但还是属ssh方式速度最



# 三、git分支管理

## 分支的概念

所谓分支，可以理解成一个个相互独立的工作空间，在每一个分支上的改动不会影响到其他分支的代码。git默认的分支是master分支。

试想一下这样一个场景：

正在master分支写主干需求的代码，突然来了一个很紧急的临时需求，需要在一周之内完成。如果在master分支直接开发的话，可能会造成主干需求的代码出现问题，这个时候就可以新拉一个分支（比如叫dev），dev分支的初始代码和master分支的完全相同，这个时候就可以放心的在dev分支上开发新需求了，不用担心会影响到master分支。等到在dev分支开发完新需求并充分测试验证没问题之后，就可以合并到master分支了，合并之后就可以删掉dev分支。

## 创建与合并、删除分支

- 查看当前分支：`git branch`

会列出来所有分支，并且会在当前分支前面加上一个"*"

- 创建一个名为"dev"的分支：`git branch dev`
- 切换到名为"dev"的分支：`git checkout dev`
- 创建一个名为"dev"的分支并切换到dev：`git checkout -b dev`
- 假如当前在master分支，合并dev分支到当前分支：`git merge dev`
- 假如当前在master分支上，删除另外一个分支dev：`git branch -d dev`

注：如果当前在dev分支，那么是无法删除dev分支的。如果dev分支并没有和master分支合并，也是无法删除的，除非使用强制删除命令：`git branch -D dev`

## 查看分支合并历史记录

当合并分支出现冲突时，需要先解决冲突，再合并分支。使用`git log --graph`命令可以查看合并分支的历史记录。

注：要注意的是，`git merge <branch-name>`命令默认采用的是fast-forward模式，也就是如果合并某个分支（比如dev）到比如master分支后，把dev删掉，那么在这种模式下的历史记录里是看不到合并记录的。如果想要看到dev分支合并的历史记录，那么在合并时就要采用普通模式：`git merge --no-ff -m "merge with no-ff" <branch-name>`，这里加-m参数是因为本次合并要创一个新的commit.

## 保存与恢复工作现场

- 假如当前在dev分支上，保存工作现场命令：`git stash`，可保存多个工作现场。
- 查看保存的工作现场列表：`git stash list`
- 恢复并删除最后保存的工作现场：`git stash pop`
- 恢复指定的工作现场：`git stash apply stash@{0}`
- 删除指定的工作现场：`git stash drop stash@{0}`

## 操作远程仓库

- 查看远程库信息：`git remote -v`
- 在本地创建和远程分支相关联的分支：`git checkout -b <local-branch-name> origin/<remote-branch-name>`，本地和远程分支的名称最好相同。
- 把本地dev分支推送到远程库：`git push origin dev`

注：如果推送失败，可能本地代码不是最新的，可以先用`git pull`命令先把远程的代码拉到本地，如果有冲突处理一下冲突，然后再推送。不过`git pull`命令也有可能失败，这是因为没有建立本地分支和远程分支的关联，还需要先用这个命令建立关联：`git branch --set-upstream <local-branch-name> origin/<remote-branch-name>`

```
git branch --set-upstream-to=origin/<branch>
```



## 多人协作推荐的分支策略

- master分支放用于发布的代码，平时不要在master分支上进行日常开发。
- 建立dev分支用于平时的开发，在发布版本的时候把代码合并到master分支。
- 团队的每个人从dev分支建立以自己名字命名的分支，如：zhangsan,lisi,wangwu, 每天结束后都把各自代码合到dev分支上。
- 多人开发的新需要，可以从dev分支建立feature分支，在feature分支上进行开发，开发完成后合入dev分支。
- 当master分支出现bug时，可以基于master拉一个bug分支，在bug分支上修改并调试好bug后再合入master分支。

## 多人协作常见的协作模式

- 日常可以通过`git push origin <local-branch-name>`来往远程推送自己的修改。
- 如果推送失败，说明远程的代码比本地的要新，需要使用`git pull`先试图合并。
- 如果合并有冲突，则先解决冲突，然后在本地commit.
- 如果没有冲突，或解决完冲突后，再使用`git push origin <local-branch-name>`即可推送成功。
- 如果`git pull`命令提示`no tracking information`，则说明本地分支没有和远程分支建立关联，用这个命令建立关联：`git branch --set-upstream <local-branch-name> origin/<remote-branch-name>`
- 如果上面一条命令仍然执行不成功，说明本地仓库还没有和远程仓库建立连接，可以用如下命令建立连接：`git remote add origin git@github.com:xxx/xxx.git`

# 四、.gitignore文件

## 已有的.gitignore文件大全

链接：<https://github.com/github/gitignore>

针对各种语言的，可以直接拿来用。在github上创建远程仓库的时候，也可以直接指定选择哪些.gitignore文件。

## 自己创建.gitignore文件

- 在当前本地git仓库根目录下，创建一个名为".gitignore"的文件，并在其中按如下格式写入要忽略的文件/文件夹：

```
# i will ignore these files:
*.dll
*.class
*.pyc
debug/*
```

注：第1行"#"后面的是注释，第2~4行分别表示要忽略`*.dll、*.class、*.pyc`文件，最后一行表示忽略掉debug目录及目录的所有内容。

- 保存并提交该.gitignore文件。
- 用`git status`命令再查看状态，发现工作区的状态已经是clean了，没有再提示`*.dll、*.class、*.pyc`这些类型的文件和debug目录下的文件未提交了。

## 清除已经提交的文件

比如在配置.gitignore文件之前，就不小心提交了一些dll文件和debug目录下的文件，现在想清除仓库中的这些文件，那么可以这样办：

```
git rm *.dll 
git rm -r debug
git rm --cached *.dll
git rm –r --cached debug
git commit -m "清除缓存"
```

执行完之后发现代码库中就没有这些文件/文件夹了。

## 修改git的全局配置

上面添加了.gitignore文件之后，只会对当前仓库产生影响，那么如果想把这个.gitignore文件作为全局配置，该怎么办呢？

- 创建一个.gitignore_global文件，添加要忽略的文件/文件夹清单。
- 执行命令：`git config --global core.excludesfile .gitignore_global`即可。