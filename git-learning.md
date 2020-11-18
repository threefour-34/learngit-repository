## 分布式版本控制系统VS集中式版本控制系统  
1. 无需联网
2. 安全

***

## 首次操作先登录 邮箱和用户名

$ cd ~  切换到用户的主目录
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

## git checkout命令  
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

  ## git clone命令
  $ git clone https://github.com/GitHub账号名/远程仓库名.git  
  克隆一个远程仓库到本地仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。  
  再用  
  $ cd 远程仓库名  
  $ ls  
  可以列出文件  
  打开本地仓库，就可以看到本地仓库中有一个名字为远程仓库名的文件夹，里面就是远程仓库所有文件   
  PS:Git支持多种协议，包括https(速度慢)，但ssh协议速度最快。  
  要删除直接在本地仓库删除文件或文件夹就可以了

## git pull命令
* git pull <远程主机名> <远程分支名>:<本地分支名>
?????

## 分支管理  
  1. 创建与合并分支概念  
     主分支：master  
    a. HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支  
      b. 一开始的时候，master分支是一条线，Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点：  
      c. 每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长。  
      d. 当我们创建新的分支，例如dev时，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上：  
      e. 从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，dev指针往前移动一步，而master指针不变：  
      f. 假如我们在dev上的工作完成了，就可以把dev合并到master上。Git怎么合并呢？最简单的方法，就是直接把master指向dev的当前提交，就完成了合并  
      g. 合并完分支后，甚至可以删除dev分支。删除dev分支就是把dev指针给删掉，删掉后，我们就剩下了一条master分支：  

  2.  创建dev分支
    $ git checkout -b dev  
    Switched to a new branch 'dev'
    //git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：  
    $ git branch dev  
    $ git checkout dev  
    Switched to branch 'dev' 
  3. git branch命令查看当前分支  
   git branch命令会列出所有分支，当前分支前面会标一个*号。
  4. 切换回master分支
  $ git checkout master  
Switched to branch 'master' //无法查看分支上提交/修改的文件
  5. 合并分支
   git merge命令用于合并指定分支到当前分支。  
   $ git merge dev//当前分支为master  
   还有其他合并方式  
  6. 删除dev分支
    $ git branch -d dev  
  7. 再查看branch
   $ git branch    
  * master//只剩master了
  8. 切换分支 git checkout branch//撤销为git checkout --file,一个命令两个作用不科学  
  用switch更科学  
  a. 创建并切换到新的dev分支，可以使用：  
  $ git switch -c dev  
  b. 直接切换到已有的master分支，可以使用：  
  $ git switch master
  9. **总结** 
   Git鼓励大量使用分支：  
查看分支：git branch  
创建分支：git branch name  
切换分支：git checkout name或者git switch name  
创建+切换分支：git checkout -b name或者git switch -c name  
合并某分支到当前分支：git merge name  
删除分支：git branch -d name  

## 解决冲突
解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。  
用git log --graph命令可以看到分支合并图。  
https://www.liaoxuefeng.com/wiki/896043488029600/900004111093344

---

## 分支管理策略  
合并分支时，加上--no-ff参数(表示禁止使用Fast forward)就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。  
$ git merge --no-ff -m "merge with no-ff" dev//合并dev到master上

***

## git stash
隐藏工作区内容(git add但没有commit的文件)  
git stash list查看隐藏的工作区内容    
git stash apply恢复，但不删除内容  
git stash pop 恢复的同时删除stash内容  

## git cherry-pick commit ID
复制一个特定的提交到**当前分支**  

## git branch -D name命令  
强行删除或丢弃一个没有被合并过的分支  

## 多人协作
* 查看远程库的信息 
  * git remote
  * git remote -v 显示更详细的信息
* 推送分支
  * git push origin master
  * 不是所有分支都要推送
https://www.liaoxuefeng.com/wiki/896043488029600/900375748016320

## git rebase
rebase操作可以把本地未push的分叉提交历史整理成直线；

rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

## 标签管理
* 发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。
* 首先，切换到需要打标签的分支上：
* 敲命令git tag <name>
* 用命令git tag查看所有标签：
* 默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？
* 方法是找到历史提交的commit id，然后打上就可以了，还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：git tag -a <name> -m"message" commit-id
* 注意，标签不是按时间顺序列出，而是按字母排序的。可以用git show <tagname>查看标签信息：

* 删除标签
  * delete tag 'name'
  * 因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。
  * 如果要推送某个标签到远程，使用命令git push origin <tagname>：git push origin --tags
  * 删除远程标签
    * 本地删除
    * 远程删除$ git push origin :refs/tags/<tagname> 
     To github.com:username/repositoryname.git   
     - [deleted]         <tagname>
    * 注意origin和冒号之间有个空格
* 强制删除

## 使用GitHub
* 如何参与一个开源项目呢？比如人气极高的bootstrap项目，这是一个非常强大的CSS框架，你可以访问它的项目主页https://github.com/twbs/bootstrap，点“Fork”就在自己的账号下克隆了一个bootstrap仓库，然后，从自己的账号下clone：
* 一定要从自己的账号下clone仓库，这样你才能推送修改。如果从bootstrap的作者的仓库地址git@github.com:twbs/bootstrap.git克隆，因为没有权限，你将不能推送修改。
* 如果你想修复bootstrap的一个bug，或者新增一个功能，立刻就可以开始干活，干完后，往自己的仓库推送。
* 如果你希望bootstrap的官方库能接受你的修改，你就可以在GitHub上发起一个pull request。当然，对方是否接受你的pull request就不一定了。

## 国内托管平台Gitee
既关联GitHub远程仓库又关联Gitee
* 我们先删除已关联的名为origin的远程库：
  * git remote rm origin
* git remote add github git@github.com:用户名/仓库名.git
* git remote add gitee git@github.com:用户名/仓库名.git

## 忽略特殊文件
* 忽略某些文件时，需要编写.gitignore；
* .gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！

## 配置别名
* $ git config --global alias.co checkout  //用co 表示checkout
$ git config --global alias.ci commit  
$ git config --global alias.br branch  
* --global参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用。

## GUI工具
例：Source Tree

## Git Cheat Sheet




  



