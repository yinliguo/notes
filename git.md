# git

注：代码中的“\”应为“/”  

- **git init [--bare] <仓库名>**  

	初始化仓库  
    
	e.g.1  `git init myproject`  
	e.g.2  `git init --bare myproject` //生成的仓库没有工作区  
   
- **git clone <仓库地址> [新仓库名]**  

	克隆一个仓库  
    
	e.g.1 `git clone git:\\server\path\to\myproject.git`   
	e.g.2 `git clone http:\\server\path\to\myproject.git`  
	e.g.3 `git clone user@server\path\to\myproject.git hello`  
    
- **git status**  

	检查当前文件的状态  
    
    e.g.1 `git status`  
    
- **git add <文件名>**  

	把文件放入暂存区  
    
    e.g.1 `git add README.md`
    
- **git diff [--cached]**  

	比较文件  
    
    e.g.1 `git diff`  //比较当前文件与暂存区中的区别  
    e.g.2  `git diff --cached`  //比较暂存区与上次提交的文件的区别  
    
- **git commit [-a] [-m] [--amend]**   

	提交文件  
    
    e.g.1 `git commit -m "this is my first commit"` //给提交加上注释  
    e.g.2 `git commit -a`  //跳过暂存区，直接提交  
    e.g.3 `git commit --amend`  //将这次提交与上次提交合并  
    
- **git rm [-f] [--cached] <文件名>**

	移除文件  
    
    e.g.1 `git rm hello.c`  //删除hello.c  
    e.g.2 `git rm -f hello.c`  //当文件已提交到暂存区，则必须使用-f来删除  
    e.g.3 `git rm --cached hello.c`  //不跟踪hello.c了，但保留hello.c ，后面可以加入.gitignore中  
    
- **git mv <源文件名> <新文件名>**

	修改文件名  
    
    e.g.1 `git mv s.txt d.txt`  
    
- **git log [-p] [-num] [--stat] [--pretty] [--graph] [--since,until]**  

	查看提交历史  
    
    e.g.1 `git log`  
    e.g.2 `git log -p -2`  //-p表示展开内容差异，-2表示最近两次  
    e.g.3 `git log --stat`  //显示简要的信息  
    e.g.4 `git log --pretty=format:"%h - %an , %ar : %s"`  //以特定格式输出  
    e.g.5 `git log --graph`  //输出简单图形  
    e.g.6 `git log --since=2.weeks` //显示最近两周的提交  
    
- **git reset HEAD <文件名>**  

	把文件从暂存区中移除  
    
    e.g.1 `git reset HEAD hello.c`
    
- **git checkout -- <文件名>**  

	把修改的文件恢复为上次提交的状态  
    
   	e.g.1 `git checkout hello.c`
    
- **git remote [-v]**  

	查看当前的远程库  
    
    e.g.1 `git remote -a` //-a表示显示远程库的地址  
    
- **git fetch [远程仓库名]**  

	从远程仓库获取更新的数据，并不合并到本地  
    
    e.g.1 `git fetch origin`  
    e.g.2 `git fetch`  
    
- **git pull**  

	从远程仓库获取更新的数据，并合并到本地  
    
    e.g.1 `git pull`

- **git push [远程仓库名] [分支名]**  

	把本地的数据推送到远程库  
    
    e.g.1 `git push origin master`  
    
- **git remote show [远程库名]**  

	查看远程库信息  
    
    e.g.1 `git remote show origin`  
    
- **git remote rename <原仓库名> <新仓库名>**  

	对远程仓库重命名  
    
    e.g.1 `git remote rename pb paul`  
    
- **git remote rm <远程仓库名>**  

	删除远程仓库  
    
    e.g.1 `git remote rm paul`
    
- **git tag [-a] [-v]**  

	打标签  
    
    e.g.1 `git tag`
    e.g.2 `git tag -a 'v7.6' -m 'this is important'` //自定义标签和注释  
    e.g.3 `git tag -a v1.4 9fceb02`  //后期补充标签  
    
- **git push origin [标签名]**  

	默认情况下git push不会推送标签，所以需要手工推送  
    
    e.g.1 `git push origin v1.5`  
    e.g.2 `git push origin tags`  //一次推送所有的标签  
    
- **git branch [分支名]**

	新建分支  
    
    e.g.1 `git branch develop`
    
- **git checkout [分支名]**

	切换分支
    
    e.g.1 `git checkout develop`
    
- **git checkout -b [分支名]**

	新建并切换分支
    
    e.g.1 `git checkout -b develop`
    
- **git branch -d [分支名]**

	删除分支
    
    e.g.1 `git checkout -d hotfix`
    
- **git stash**

	保存当前分支的工作，用于切换分支
    
    e.g.1 `git stash`
    
- **git stash pop**
	
    弹出当前分支已保存的工作
    
    e.g.1 `git stash pop`