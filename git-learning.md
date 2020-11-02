## 分布式版本控制系统VS集中式版本控制系统  
1. 无需联网
2. 安全

## 首次操作先登录 邮箱和用户名

在进行任何Git操作之前，都要切换到Git仓库目录下，也就是说要先切换到项目的文件夹目录下

## 改变git默认用户目录  
在电脑设置中找到编辑账户的环境变量中的用户变量，添加一个"Home"变量，把值改为目标目录

## 创建一个仓库(版本库 repository)  
1. 选择一个合适的地方，创建一个空目录  
    $ mkdir learngit  
    $ cd learngit  
    $ pwd//显示当前目录  
    /Users/michael/learngit  
2. git init//将这个目录变成Git可以管理的仓库  
   该目录下多了一个.git目录，用来管理仓库，不要随意修改里面的文件  若没有看见.git，则表示目录默认为隐藏的，用ls-ah命令  
   
## 把文件放到Git仓库  
1. git add 文件.格式(尽量用英文命名)  
   可以add多个文件(若添加成功，则无任何提示)  
   把要提交的所有修改放到暂存区(Stage)  
2. git commit -m"本次提交说明"
   成功执行后：X file changed(X个文件被改动)    
   N insertions(插入了N行内容)  
   一次性把暂存区的所有修改提交到分支  
   一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的(nothing to commit)    

## git status命令    
让我们时刻掌握仓库当前的状态  例：已修改，但还未提交

## git diff命令    
git diff 文件.格式  
看看具体修改了什么内容(如果git status告诉你有文件被修改过，用git diff可以查看修改内容。)  

## git log命令
查看历史记录  
git log命令显示从最近到最远的提交日志  
如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数  
将会输出一大串数字字母组成的commit id(版本号)

## git reset命令
回到文件之前的版本  
git reset--hard HEAD^(回到上一个版本 HARD^^上上个版本……HARD~100上100个版本)  
git reset--hard commit id(任意某个版本的commit id，写前几位即可)
  
## git reflog命令
现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的commit id怎么办？  
Git提供了一个命令git reflog用来记录你的每一次命令  

## Git管理的是修改，而不是文件

## git checkou命令  
git checkout -- 文件.格式(file)  
丢弃工作区中的修改    
命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
1. 一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
2. 一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。  
总之，就是让这个文件回到最近一次git commit或git add时的状态。
3. git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。  
4. (从来没有被添加到版本库就被删除的文件，是无法恢复的！)

## 小结
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。  
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。  
场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

## git rm命令
从版本库中删除文件  
命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。  

先 git rm 文件.格式  
再 git commit -m "tips"

## 远程仓库
1. 创建SSH Key钥匙  
   **$ ssh-keygen -t rsa -C "youremail@example.com"**  
   一路回车，使用默认值  
   如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，**id_rsa.pub**是公钥，可以放心地告诉任何人。
2. 登录GitHub，打开Account settings的SSH Key页面  
   然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容   
3. 提醒：在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）    
   如果你不想让别人看到Git库，有两个办法：
   1. 交点保护费，让GitHub把公开的仓库变成私有的，这样别人就看不见了（不可读更不可写）。
   2. 自己动手，搭一个Git服务器，因为是你自己的Git服务器，所以别人也是看不见的。这个方法我们后面会讲到的，相当简单，公司内部开发必备。
4. 添加远程仓库(GitHub上有提示)  
   **$ git remote add origin https://github.com/GitHub账号名/本地仓库名.git**//添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。
5. 把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。  
   由于远程库是空的，我们第一次推送master分支时，加上了-u参数(即 git push -u origin master)，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
   从现在起，只要本地作了提交，就可以通过命令：
  **$ git push origin master**
  把本地master分支的最新修改推送至GitHub
6. SSH警告
  当你第一次使用Git的clone或者push命令连接GitHub时，会得到一个警告：  
  这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入yes回车即可。  
  Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：
  Warning: Permanently added 'github.com' (RSA) to the list of known hosts.  
  这个警告只会出现一次，后面的操作就不会有任何警告了。