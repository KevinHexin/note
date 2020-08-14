### 初始化
1. 生成`ssh-key`<br>
`ssh-keygen`<br>
运行上述命令会确认密钥的存储位置, 默认情况下, `linux`系统ssh密钥在`~/.ssh`目录下<br>
查看`ssh`公钥, 添加到Git远程服务器上<br>
`cat ~/.ssh/id_rsa.pub`<br>

2. 在安装Git工具后, 需要配置用户信息<br>
`git config --global user.name "kevin"`<br>
`git config --global user.email "kevin@example.com"`<br>
由于添加了`--global`选项, 那么在每次Git提交的时候都会读取该信息, 如果想针对某个项目使用不同的用户信息, 去掉`--global`选项<br>
注意, 此信息并不是密码类的信息, 意味着可以配置任意信息<br>

### 关联一个远程仓库
关联远程仓库的信息记录到`.git`隐藏目录下<br>
1. 在远程仓库(如`GitHub/GitLab`)新建一个工程<br>
2. 选择下载远程仓库到本地<br>
`git clone git@xx.xx.xx.xx:kevin/project.git`<br>
此时, 本地的`project`文件夹就关联到了远程仓库<br>
3. 针对第2步, 也可以在本地新建一个工程, 然后<br>
`git init`<br>
`git remote add origin git@xx.xx.xx.xx:kevin/project.git`<br>
此时, 本地的`project`文件夹就关联到了远程仓库<br>
4. 如果本地文件夹已经关联到了另一个远程仓库<br>
`git remote rename origin old-origin`<br>
`git remote add origin git@xx.xx.xx.xx:kevin/project.git`<br>
此时, 本地的`project`文件夹就关联到了远程仓库<br>

### 本地提交
1. 将本目录以及下级目录所有文件添加到暂存区<br>
`git add .`<br>
2. 记录本次提交相对于上次提交文件的更改<br>
`git commit -m "info"`<br>
`-m` 添加本次提交的注释(必须添加注释)<br>
3. 提交, 如果远程仓库没有`dev`分支, 则在远程仓库新建一个`dev`分支<br>
`git push origin dev`<br>
远程仓库名为`origin`<br>

### 分支管理
1. 查看本地所有分支<br>
`git branch`<br>
更多`branch`命令参看下面`其他`部分<br>
2. 切换分支(如果本地Git库有`dev`分支)<br>
`git checkout dev`<br>
切换到`dev`分支<br>
3. 新建本地分支(如果本地Git库没有`dev`分支)<br>
`git checkout -b dev`<br>
此命令在本地Git库新建一个`dev`分支, `-b` 选项表示进入`dev`分支<br>
4. 新建并关联远程分支<br>
`git checkout -b dev origin/dev`<br>
在本地新建一个`dev`分支, 与远程仓库中的`dev`分支对应起来, 并切换到`dev`分支<br>
5. 新建了本地分支, 并与远程分支关联起来<br>
`git branch --set-upstream-to=origin/dev dev`<br>
6. 重命名本地分支<br>
`git branch -m old-branch new-branch`<br>
7. 删除本地分支<br>
`git branch -d dev`<br>
8. 删除远程仓库分支<br>
`git push origin --delete dev`<br>
9. 合并本地分支<br>
`git merge dev`
先进入另一个分支, 如`master`分支, 然后合并`dev`分支到`master`分支<br>

### 协作开发
1. 拉取远程分支<br>
`git pull origin dev`<br>
注意, 在此之前一定要新建并关联远程分支, 然后再拉取远程分支, 切勿在已经存在的本地分支上拉取, 因为这样会直接合并代码<br>
2. 合并分支<br>
`git fetch origin dev` 拉取远程`dev`分支到本地`origin`存放区<br>
`git merge origin/dev` 合并远程`dev`分支到本地当前分支<br>
3. 解决冲突<br>
`git status` 查看冲突文件<br>
`vim xxx.c` 编辑冲突文件(注意也要删除 `<<<HEAD====DEV>>>` 等信息)<br>
`git add xxx.c` 将解决冲突后的文件添加到暂存区<br>
解决完所有冲突文件<br>
`git commit`<br>
`git push origin dev`<br>
4. `merge`回退<br>
`git reset merge`回退到合并前的状态<br>
<br>
关于这一节, 可以参考:<br>
https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6<br>

### 回滚
`git log` 查看提交记录<br>
`git reset --hard commitid` 回滚到某个提交上去<br>

### 其他
1. 本地仓库里会有一个隐藏的`.git`文件夹，里面记录了远程仓库的相关信息(包括`branch`的信息)
2. `git remote` 查看远程仓库名(一般叫`origin`)
3. `git branch` 查看本地分支(本地主分支一般叫`master`, 带`×`号的是当前正在操作的分支)
`git branch -r` 查看远程仓库的分支信息和`head`, `master`指针的指向<br>
`git branch -v` 查看各个本地仓库最后一次提交的`commit`信息<br>
4. `git status` 查看当前状态
5. `git diff origin/dev dev` 比较`origin-dev`仓库和本地`dev`仓库的区别
`git diff` 不带任何参数时, 默认列出工作区和暂存区的区别<br>
`git diff --name-only` 只列出变更文件名<br>
`git diff --name-status` 列出变更文件状态(添加, 修改, 删除等)<br>
关于`diff`和工作区等的讨论见:<br>
https://www.liaoxuefeng.com/wiki/896043488029600/897271968352576<br>
6. `git log` 查看提交记录  `--stat` 仅显示简要的行数统计, 
此命令可以在`push`之前查看有哪些文件将要提交, 避免提交无意义的文件
`-2` 只查看最近两次的commit, 完整示例:
`git log --stat -2`
7. 操作本地git仓库前, 最好 `sudo chmod 777 -R ./`
8. 当`gitignore`不起作用时, 一般是因为`vscode`为我们自动记录了文件更改信息
因此我们需要删除`.vscode`在工作区的缓存, 键入`git rm -r --cached dir`即可<br>
更好的方式是在项目创建的时候将`.vscode`添加到`.gitignore`中<br>
9. `git config -l`查看自己配置的用户名和邮箱
10. `git tag -a tag_name -m "info about this tag"`  给某个分支打上标签
11. `git push origin tag_name` 提交`tag` 

><br>
https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424<br>
