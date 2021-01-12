# Git

> Git 命令太多记不住？这篇笔记送给你。

### 概念
- 工作区：工作目录，工作文件
- 暂存区：临时存储目录
- 本地仓库：实际存储位置，和工作区，暂存区都位于开发机器上
- 远程仓库：位于另外一台电脑上（充当git服务器）只是作为中转代码


### 使用Git
1. 初始化仓库
新建目录，使用git init初始化仓库，会生成.git隐藏文件
```bash
	git init
```
2.添加到暂存区
	git add
**注意添加到暂存区之后如果想要从暂存区移除有两种情况**
	- 该文件没有提交过：可以使用 git rm –cached 文件名
	- 如果该文件之前commit过（此时状态为modify） 则需要使用git checkout – 文件名

```bash
	git add .  	// 添加所有改动文件到暂存区
	git add a.txt  	// 添加a.txt到暂存区
```
3.从暂存区提交到本地仓库
	git commit -m '备注'
```bash
	git commit -m '第一次提交'
```
4.在Github上新建仓库，把本地代码推送到github上面的远程仓库
	- 新建远程仓库，复制地址
	- 第一次推送需要配置远程地址
```bash
	git remote add origin 	// 你复制的远程仓库地址	
	git push -u origin master
	
	// 以后无需配置直接 git push 即可完成推送
```

### 版本管理
1. 查看工作区，暂存区状态
```bash
	git status
	// 红色 代表有修改但是没有提交到暂存区
    // 绿色 暂存区有内容没有提交到本地仓库
```
2. 撤销修改内容
```bash
	git  checkout –文件名
```
3. 撤销暂存区到工作区
```bash
	git reset HEAD 文件名
```
4. 查看仓库日志
```bash
	git log
```
5. 查看所有版本
```bash
 	git reflog
```
6. 回档到指定版本
```bash
 	 git reset version
```
7. 使用HEAD 可以回退到上一个/ 上几个版本
```bash
	git reset –hard HEAD^
	git reset –hard HEAD^^^
	git reset –hard HEAD~n
```
### 用户信息配置
1. 全局用户身份
位于C盘用户Administrator目录下方.gitconfg
```bash
	git config --global user.email *****@***.com
	git config --global user.name userName
```
2. 项目用户身份
位于项目文件夹下方.git目录 config文件
```bash
	git config user.email *****@****.com
	git config user.name userName
```
### 分支
1. 查看分支
```bash
	git branch
```
2. 创建分支
```bash
	git branch <name>
```
3. 切换分支
```bash
	git checkout <name>
	// 或
	git switch <name>
```
4. 创建并合并分支
```bash
	git checkout -b <name>
	// 或
	git switch -c <name>
```
5. 合并某分支到当前分支
```bash
	git merge <name>
```
6. 删除分支
```bash
	git branch -d <name>
```