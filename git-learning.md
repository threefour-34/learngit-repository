## 分布式版本控制系统VS集中式版本控制系统  
1. 无需联网
2. 安全

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
2. git commit -m"本次提交说明"
   成功执行后：X file changed(X个文件被改动)    
   N insertions(插入了N行内容)    

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

10.30进度：廖雪峰的官方网站-工作区和暂存区 
