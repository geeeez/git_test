



### 前言

网上很多的教程，当然首先推荐的是廖雪峰的git教程。但是看廖雪峰的教程还是有点拖沓，不适合性急的人，所以写下这个，带你入门。这篇并不高深，菜鸟级别的git入门教程，或者说是我的入门笔记。

### git由来
linux在后期的开发过程中开发人员和代码量巨大，急需版本控制工具，刚开始有公司赞助给他们用，后来他们试图破解这个工具(确实很作死)，人家不给他用了，这些linux coder一气之下用C开发了初代的git，后来火了之后又在2008年做了github网站托管全球代码，从此git走入千家万户。
git是分布式的，但是我们普通人用起来和集中式没有区别。所以不用管这个概念。

### 安装git
linux :  sudo apt-get install git
mac:   安装homebrew - > 安装git   安装xcode - > preferences->downloads->Command Line Tools->install
window: <a href=https://git-scm.com/downloads>官方安装程序链接，推荐安装setup版而不是portable版</a>
安装完以后需要你进行设置标记下是谁的github ：

```bash
$ git config --global user.name "name or nick name"
$ git config --global user.email "email@example.com"
#git config 的 --global参数 表示此电脑所有git仓库都使用这个配置
```

### 版本库
版本库就是一个小小的仓库，这个仓库由git管理，里面任何东西的变动它都知道，在电脑上它就是一个文件夹。
我们来创建一个仓库（文件）

```bash
mkdir testforgit
cd testforgit
git init 
#上面三步我们创建了testforgit文件夹，然后初始化它为仓库交由git管理
#我们可以 ls -la 看下当前目录下有一个.git文件 这个文件就是用来跟踪记录这个仓库的。
#我们也可以初始化一个非空的仓库
#注意：git只能管控文本文档，所以图片，word等二进制文件他只能管控记录大小的更改，无法知道图片文件中的具体哪行被更改了。
#注意：初始化完成后会生成.git文件夹，我们最好不要手动去修改里面的东西，这就像是小偷一样，不跟管理员说搞走一个，git管理员可能自己混乱，仓库也被破坏了。
```
初始完以后我们需要尝试写一个文件，比如readme.txt 写完放入git管控的目录下，这样git才能找到它，然后我们两步提交此文件到服务器：
```bash
$ git add readme.txt
$ git commit -m "add 3 files."
# -m 参数是说明的意思，强烈建议每个commit写说明

# 为什么需要两步？因为这样你可以add多个文件，然后一起commit到服务器,其中git add是提交到暂存区，gitcommit把暂存区的所有文件一起提交
$ git add file1.txt 
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
#文件夹就是工作区，而 add 是提交文件到暂存区，这个概念很重要。一定要记住啊哦！
#如果你有一个文件readme.txt修改一次 git add了 然后它就进入了暂存区，此时本地再次修改readme.txt 再次git add，此时两次修改就被git合并存在暂存区，此时commit的就是最新的修改了！
#所以切记，commit只能提交存储在暂存区的文件
```

### 基础命令

##### 修改文件

我们修改readme文件，然后通过以下命令来查看区别：

```bash
$ git status
#会显示那些代码被修改了，我们画个垃圾草图
	          |  -> untracked files     #从没有git add过的文件，即不被git追踪的文件
文件status分为 | -> 红色 modify           #修改过，但是没有add
              |  -> 绿色 modify          #修改过 git add完了 但是还没有 commit
#如果当前没有任何改动，则会显示：
On branch master
nothing to commit, working tree clean

$ git diff readme.txt
#查看readme.txt和文件服务器上的版本有什么区别，可以清楚的知道改了哪些内容

#然后你就可以继续提交了
$ git add
$ git commit -m
```

##### 版本回退

修改完文件后我们后悔了，或者说我们修改完的代码崩溃了 调不过来了，我们想回退到以前能正常运行的版本。好，这在git是非常方便的：

```bash
#我们首先看下现在的reamde文件内容
$ cat readme.txt
t is a version control system
add2
add3

#用git log 查看我们一共提交了多少个版本
$ git log
commit 97f63044c14952fc5d13a08a8448aded163de489 (HEAD -> master)
Author: geeeez <gz54321.a@gmail.com>
Date:   Tue Nov 5 11:07:24 2019 +0800

    add3

commit 4aae9f796a8440a0a6eb882ed99596a5fb1b5a40
Author: geeeez <gz54321.a@gmail.com>
Date:   Tue Nov 5 11:06:58 2019 +0800

    add2

commit cedeb93177ea09827758cdf1491a7f009ac4ef25
Author: geeeez <gz54321.a@gmail.com>
Date:   Mon Nov 4 23:05:19 2019 +0800

    learn git

#我们一共修改了两次，分别加入了add2 和 add3 ，其中有HEAD -> master表示的代表当前版本，commit后面代表的是版本唯一id，下面就是一些修改内容，如果我们想回退上次版本(add2) 只需
$ git reset --hard HEAD^

#回退上上次
$ git reset --hard HEAD^^

#回退上一百次呢？难道要一直写^?当然不是，这不是git的哲学。我们可以~加次数
HEAD~100

#但是很多时候我们不会去属次数，所以还有一张方法，和docker一样 我们可以直接通过id进行回滚
$ git reset --hard 1094a

#现在我们回退到版本id为 cedeb（不需要写全，和docker一样）
$ git reset --hard cedeb
HEAD is now at cedeb93 learn git

#现在再来看看readme.txt的内容吧，可以看到add2 和 add3 都消失了
t is a version control system

#此时我们再看git log 发现 cedeb之后的版本全部消失了，那我们后悔了不想回退了，还想找回最新的版本怎么办？
#这在git也是可以的，我们有个命令git reflog 记录了你的所有操作
$ git reflog
cedeb93 (HEAD -> master) HEAD@{0}: reset: moving to cedeb
97f6304 HEAD@{1}: commit: add3
4aae9f7 HEAD@{2}: commit: add2
cedeb93 (HEAD -> master) HEAD@{3}: reset: moving to head^
d947e60 HEAD@{4}: commit: test
cedeb93 (HEAD -> master) HEAD@{5}: commit (initial): learn git

##通过上述的版本id，你可以恢复到任何你commmit过的版本。是不是很简单啊！

```

##### 撤销修改

有一天你在写代码，本来没啥事，结果等你第二天起来的时候发现昨晚写的工作区代码有很多bug，bug多到懒得调试，此时你可以一行一行的查找删除修改，也可以直接回滚版本，但是你发现出问题的代码仅存在在 login.php中，于是你想有没有一种办法回滚单个文件？嘿嘿 确实有
```bash
$ git checkout -- login.php
#命令git checkout -- login.php 意思就是，把login.php文件在工作区的修改全部撤销，这里有两种情况：

#一种是login.php自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
#一种是login.php已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

#总之，就是让这个文件回到最近一次git commit或git add时的状态。
```

上次逃过一劫，然后你就放松警惕了，某天晚上你又在login.php写了一堆漏洞，然后还git add到了暂存区，但是你还没有commit，这时候如果用上面的命令是没用的，因为上面的命令同样只能恢复到提交到暂存区的状态，于是你晚上大哭大闹，觉得自己真是stupid，不过git 可以帮你：
```bash
git reset HEAD <file>
#reset 可以回退版本 也可以把文件从暂存区回退回来，这样就没有git add了 我们又可以用 $ git checkout -- login.php 回滚到上个commit 的 login.php了 
```

嘿嘿，晚上终于可以早点睡觉了！

##### 删除文件

当你删除本地库的readme.txt文件时  git status 会显示和版本库的文件不一致 会显示你删除了那个文件。

这个时候我们可以直接删除版本库的文件 git rm readme.txt  然后再次commit	一下，这样版本库和本地库就同步了。文件就删除完成。

如果你是不小心删除的，那就是恢复功能了，直接用上面的git checkout -- readme.txt

### git核心功能远程仓库来了

我们掌握了git的基础命令后发现git和svn也确实没啥大区别，但是为啥那么多人用git呢？接下来就是git方便的地方了，同时通过这个也可以知道为啥github那么受欢迎。

git远程仓库可以搭建在任意一台电脑上，大家都以它为仓库来拉取和提交代码，但是对于小公司来说没有必要自己去搭建一个git服务器，我们只需要把github当作一个云端电脑即可。（省时省力省钱）

##### 免费的git仓库

我们只需要注册一个github就获得了一个免费的github仓库，下面我们需要配置这个仓库使得我们本地和云端github仓库安全传输代码和数据。

1、首先我们创建SSH key，在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
```bash
$ ssh-keygen -t rsa -C "yourmail@mail.com"

```
剩下的一路回车选择默认值即可，当然如果你想给你的key设置密码也可以（过程中直接输入密码），不过我这里就不设置了。

然后我们就创建成功了，这时候你的用户主目录下有.ssh文件夹，里面分别是id_rsa和id_rsa.pub。这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

2、登录github添加公钥。登录后点击头向下拉菜单找到settings,然后点击SSH and GPG keys。然后new一个新的SSH keys：![1575614058950](C:\Users\ThinkPad、\AppData\Roaming\Typora\typora-user-images\1575614058950.png)
为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

最后友情提示，在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。

如果你不想让别人看到Git库，有两个办法，一个是交点保护费，让GitHub把公开的仓库变成私有的，这样别人就看不见了（不可读更不可写）。另一个办法是自己动手，搭一个Git服务器，因为是你自己的Git服务器，所以别人也是看不见的。这个方法我们后面会讲到的，相当简单，公司内部开发必备。

确保你拥有一个GitHub账号后，我们就即将开始远程仓库的学习。

##### 添加远程库

现在你需要登录自己的github在右上角找到“Create a new repo“ 创建一个仓库，创建完成后复制仓库的地址,回到本地的lear-git文件夹(就是你本地仓库的文件夹)，把这两个仓库关联并把本地的文件push上去：
```
# 关联两个仓库
$ git remote add origin git@github.com:aeexxxz/git_cacatest.git 
# 将本地文件push到远程仓库
$ git push -u origin master
# 把本地仓库的内容推到远程用git push命令 实际上是把master分支推送到了远程
# 第一次推送时记得加上 -u 这样 git不仅会推送master到远程，还会把本地master分支和远程master分支关联起来。这样以后推送的时候就可以简化命令了。
```

##### 从远程库克隆

生活中大多数情况是我们从零开始就已经准备使用github仓库，这时候我们肯定先创建远程仓库，然后拉取到本地，而不是从本地仓库再上传到远程仓库。

创建的github仓库的步骤和上面是一样的，我们最后仅仅需要找个合适的地方：
```bash
git clone 地址
```
如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。

此外我们还发现了git也支持https 但是https速度慢，而且必须要每次推送输入口令，这种就很麻烦，除非再公司内部只开放http的情况下使用，其他情况下我们更多的用 ssh


### 分支管理

为什么会有分支？

我们上线了一个版本，然后我们现在要开发下一个版本，这时候我们创建一个分支进行开发，然后忽然客户说上线的环境有bug，那我们可以在上线版本上再搞一个分支出来去修复bug，这样互不耽误，改bug、开放新版本、添加新需求可以同步进行，最后上线的时候再根据需要统一合并。

为什么git创建、删除、切换分支都能在一秒内完成？

因为git的分支是通过改变指针的指向来完成，无论你的分支有多少文件，对git来说，它只是把指向其他分支的指针重新指向master分支。

##### 创建和合并分支

git里主分支是master，head指向master,切换分支时head指向对应分支，我们的工作分支就是head所指向的分支。

创建分支：
```bash
$ git checkout -b dev #创建并切换到dev分支,功能等同于下面两条命令
$ git branch dev #创建dev
$ git checkout dev #切换到dev分支

# 同时为了更好记忆新版git添加了switch命令进行切换
git switch -c name #创建并切换到dev分支
$ git switch <name> #切换分支
```
查看当前分支
```bash
$ git branch
```
合并分支
```bash
$ git merge 分支名称
```
删除废弃的分支
```bash
$ git branch -d dev
```

##### 解决冲突

如果两个文件名相同内容不同且都在当前节点提交，就会变成这样：
![1575622688980](C:\Users\ThinkPad、\AppData\Roaming\Typora\typora-user-images\1575622688980.png)

这时候我们进行 git merge+分支名称 会提示冲突






