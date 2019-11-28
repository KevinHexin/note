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

### 更新到本地
1. 更新远程仓库的更改到本地<br>
`git pull origin/dev dev`<br>

### 分支
1. 查看本地所有分支<br>
`git branch`<br>
2. 切换分支(如果本地Git库有`dev`分支)<br>
`git checkout dev`<br>
切换到`dev`分支<br>
3. 新建本地分支(如果本地Git库没有`dev`分支)<br>
`git checkout -b dev`<br>
此命令在本地Git库新建一个`dev`分支, `-b` 选项表示进入`dev`分支<br>
4. 新建并关联远程分支<br>
`git checkout -b dev origin/dev`<br>
在本地新建一个`dev`分支, 与远程仓库中的`dev`分支对应起来, 并切换到`dev`分支<br>
5. 重命名本地分支<br>
`git branch -m old-branch new-branch`<br>
6. 删除本地分支<br>
`git branch -d dev`<br>
7. 删除远程仓库分支<br>
`git push origin --delete dev`<br>
8. 合并本地分支<br>
`git merge dev`
先进入另一个分支, 如`master`分支, 然后合并`dev`分支到`master`分支<br>

### 其他
1. 本地仓库里会有一个隐藏的`.git`文件夹，里面记录了远程仓库的相关信息(包括`branch`的信息)
2. `git remote` 查看远程仓库名(一般叫`origin`)
3. `git branch` 查看本地分支(本地主分支一般叫`master`, 带`×`号的是当前正在操作的分支)
4. `git branch -r` 查看远程仓库的分支信息和`head`, `master`指针的指向
5. `git status` 查看当前状态
6. `git diff origin/dev dev` 比较远程`dev`仓库和本地`dev`仓库的区别
7. `git log` 查看提交记录  `--stat` 仅显示简要的行数统计
7. 操作本地git仓库前, 最好 `sudo chmod 777 -R ./`

>ref<br>
https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424<br>
