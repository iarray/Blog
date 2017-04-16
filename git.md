## Git
	下面示例的fileName就是文件的路径,如果是目录下的所有文件,可以用*号代替

### 1.创建仓库
	git init

### 2.添加文件(从工作区添加到暂存区)
	git add fileName

### 3.提交修改(从暂存区提交到分支)
	git commit -m "备注信息"

用`git diff HEAD -- fileName`命令可以查看工作区和版本库里面最新版本的区别

### 4.撤销修改(工作区)
	git checkout -- fileName

注意不要漏了--

### 5.从暂存区撤回工作区
	git reset HEAD fileName

如果文件已经添加到暂存区,想要撤销修改需要先做第5步操作,再做第4步的操作.

### 4.添加远程仓库
	git remote add origin 仓库地址

* `origin`是远程库的名字,git默认的仓库名称也是这个
* 关联一个远程库后，使用命令`git push -u origin master`第一次推送master分支的所有内容
* 此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改
* master是分支名称,git默认的第一个分支叫master

### 5.从远程库克隆
	git clone url

### 分支管理
* 创建分支

		git checkout -b name

	或者(第一条命令等价于下面的两条命令)
	
		git branch name
		git checkout name


* `git branch`命令查看当前分支
* `git checkout master` 可以切换分支,这里是切换回主分支
* `git merge name` 命令用于合并指定分支到当前分支。当前分支用`git branch`查询
* `git branch -d name` 删除分支
* `git log --graph`命令可以看到分支合并图。
因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。

### 6.查看状态
	git status