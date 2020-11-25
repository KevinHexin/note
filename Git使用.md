### 初始化
1. 生成`ssh-key`<br>
`ssh-keygen`<br>
运行上述命令会确认密钥的存储位置, 默认情况下, `linux`系统ssh密钥在`~/.ssh`目录下<br>
查看`ssh`公钥, 添加到Git远程服务器上<br>
`cat ~/.ssh/id_rsa.pub`<br>

2. 在安装Git工具后, 需要配置用户信息, 否则不能`git commit`<br>
`git config --global user.name "kevin"`<br>
`git config --global user.email "kevin@example.com"`<br>
由于添加了`--global`选项, 那么在每次Git提交的时候都会读取该信息, 如果想针对某个项目使用不同的用户信息, 去掉`--global`选项<br>
注意, 此信息并不是密码类的信息, 意味着可以配置任意信息<br>

3. 修改`git commit`默认编辑器为`vim`<br>
`git config --global core.editor vim`<br>

### 初始化关联一个远程仓库
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

### 提交本地代码
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
在本地新建一个`dev`分支, 与远程仓库中的`dev`分支对应起来, 并切换到`dev`分支<br>

4. 新建了本地分支, 并与远程分支关联起来<br>
`git branch --set-upstream-to=origin/dev dev`<br>

5. 重命名本地分支<br>
`git branch -m old-branch new-branch`<br>

6. 删除本地分支<br>
`git branch -d dev`<br>

7. 删除远程仓库分支<br>
`git push origin --delete dev`<br>

8. 合并本地分支<br>
`git merge dev`
先进入另一个分支, 如`master`分支, 然后合并`dev`分支到`master`分支<br>

### 拉取远程新分支
1. 查看本地仓库是否与远程仓库建立连接<br>
`git remote -v`<br>
如果正常, 会显示`fetch`和`push`的远程仓库地址<br>
如果没有, 添加地址:<br>
`git remote add origin xxx`<br>

2. 查看本地`origin`仓库是否包含远程仓库上的目标分支<br>
`git branch -r`<br>

3. 如果本地`origin`仓库没有目标分支, 拉取分支<br>
`git fetch origin dev`<br>

4. 新建本地分支并切换到远程分支<br>
`git checkout -b dev origin/dev`<br>

### submodules
1. 引用一个远程仓库<br>
`git submodule add git_addr`<br>
这时在项目目录下会添加一个`.submodules`的文件, 里面记录了一些引用仓库的信息

2. 切换引用仓库分支<br>
`cd your_submodule` && `git checkout -b dev origin/dev`<br>
即进入到引用仓库目录, 进行正常的分支切换就可以了<br>
未切换分支时进入引用仓库键入`git branch`看到的是一个`commit id`, 即初始时分支会`checkout`到`HEAD`的`commit id`上<br>

3. `clone`一个带有引用分支的仓库<br>
`git clone git_addr` 即常规`clone`后, 项目目录下只会存在一个引用仓库的空目录, 里面为空<br>
需要使用`git clone git_addr --recursive`即加上`--recursive`参数即可下载引用仓库<br>
或则`git submodule init` && `git submodule update` 即可下载引用仓库<br>

4. 引用仓库的更新<br>
如果引用仓库所属的远程仓库发生了更新, 想要同步更新到本地, 只需要进入到引用仓库目录, <br>
并进行正常的git仓库更新步骤(`pull or fetch&&merge`)即可<br>

4. 修改并提交对引用仓库的更改<br>
如果对引用仓库进行了修改, 并想要上传修改, 只需要进入到引用仓库内,<br>
并进行正常的`git add .` && `git commit` && `git push`操作即可, 更新将上传到引用的远程仓库中.<br>
提交引用仓库后, 在主仓库`git status`将显示引用仓库的`subproject commit id`发生了更改, 这时需要在主仓库再次提交一次更改.<br>


### 协作开发(合并远程仓库的代码到本地)
1. 直接合并远程分支代码到本地(`不推荐的做法`)<br>
`git pull origin dev`<br>
我们知道`pull` = `fetch`+`merge`, 且此命令会直接合并`origin`代码到当前本地分支, <br>
所以不建议用此命令来合并分支<br>

2. 合并分支<br>
`git fetch origin dev` 拉取远程`dev`分支到本地`origin`存放区<br>
`git merge origin/dev` 合并远程`dev`分支到本地当前分支<br>

3. 解决冲突<br>
`git status` 查看冲突文件<br>
`vim xxx.c` 编辑冲突文件(注意也要删除 `<<<HEAD====DEV>>>` 等信息)<br>
`git add xxx.c` 将解决冲突后的文件添加到暂存区<br>

4. 提交合并结果
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

### 查看提交记录
1. `git log` 查看所有提交记录<br>
`git log -p` 查看详细提交记录, 以`diff`方式列出<br>
`git log -2` 查看最近两条提交记录<br>
`git log --stat` 仅显示统计信息<br>

2. `git show commit_id` 根据`git log`显示的`commit_id`来查看某次提交详细的记录, 类似`git log -p`<br>
`git show commit_id --stat` 仅显示统计信息<br>

### 其他
1. 本地仓库里会有一个隐藏的`.git`文件夹，里面记录了远程仓库的相关信息(包括`branch`的信息)

2. `git remote` 查看远程仓库名(一般叫`origin`)

3. `git branch` 查看本地分支(本地主分支一般叫`master`, 带`×`号的是当前正在操作的分支)
`git branch -r` 查看远程仓库的分支信息和`head`, `master`指针的指向<br>
`git branch -v` 查看各个本地仓库最后一次提交的`commit`信息<br>

4. `git status` 查看当前状态

5. `git diff origin/dev dev` 比较`origin-dev`仓库和本地`dev`仓库的区别
`git diff` 不带任何参数时, 默认列出工作区和暂存区的区别<br>
`git diff HEAD` 比较`origin`区和暂存区的区别<br>
`git diff --name-only` 只列出变更文件名<br>
`git diff --name-status` 列出变更文件状态(添加, 修改, 删除等)<br>

关于`diff`和工作区等的讨论见:<br>
https://www.liaoxuefeng.com/wiki/896043488029600/897271968352576<br>

6. 当`gitignore`不起作用时, 一般是因为`vscode`为我们自动记录了文件更改信息<br>
因此我们需要删除`.vscode`在工作区的缓存, 键入`git rm -r --cached dir`即可<br>
更好的方式是在项目创建的时候将`.vscode`添加到`.gitignore`中<br>

7. `git config -l`查看自己配置的用户名和邮箱

8. `git tag -a tag_name -m "info about this tag"`  给某个分支打上标签

9. `git push origin tag_name` 提交`tag` 

><br>
https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424<br>
