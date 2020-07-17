### 安装docker环境
`docker`类似虚拟机, 使用`docker`开启一个`image`镜像就类似使用虚拟机开启一个系统, 这个系统运行在`docker`容器内.<br>

>ref:<br>
安装docker:<br>
https://docs.docker.com/install/linux/docker-ce/ubuntu/<br>
支持NVIDIA GPU:<br>
https://github.com/NVIDIA/nvidia-docker<br>

---
### 下载与打包镜像
1. `docker pull repository:tag`
根据`repository`与`tag`下载远程仓库的镜像(如`docker hub`)<br>
2. `docker save -o /your_path/docker_image.tar image_id`
根据镜像`id`打包保存到本地<br>
3. `docker load --input /your_path/docker_image.tar`
重新加载打包的本地镜像<br>

---
### 启动一个容器
A. `docker版本 < 19.03`, 键入`--runtime=nvidia`以支持`nvidia GPU`<br>
`docker run --runtime=nvidia -itd --name xxx -v /local_path/:/mount_path/ repository:tag /bin/bash`<br>
B. `docker版本 >= 19.03`, 键入`--gpus all`以支持`nvidia GPU`<br>
`docker run --gpus all -itd --name xxx -v /local_path/:/mount_path/ repository:tag /bin/bash`<br>
启动一个后台容器, 返回`container`容器`id`<br>
`docker ps`<br>
查看所有正在后台运行的容器<br>
`docker exec -it container_id /bin/bash`<br>
根据容器id进入一个容器<br>
1. `run` 指令是`docker`启动一个容器的命令
2. `--runtime=nvidia` 让容器支持调用本机`GPU`资源而不必在意容器内`cuda`版本等
3. `-i` 以交互模式运行
4. `-t` 启动一个虚拟`tty`终端
5. `-d` 在后台启动一个`docker`容器并保持`run`状态, *如果不加此选项, 则直接进入容器*
6. `--name` 为启动的容器命名
7. `-v` 绑定挂载地址. `:`前面是本机挂载地址, `:`后面是容器内挂载地址
8. `repository:tag` 启动`docker`容器需要指定一个`image`镜像
9. `/bin/bash` 进入容器后执行的命令, 可以替换成其他命令
10. `--rm` `exit`后删除容器(不与`-d`共用)
11. `--privileged ` 给予更多的权限(例如使用perf或则生成core)，但是慎用
12. `--net=host` 让容器与主机共享同一个ip地址(慎用, 可能会造成端口冲突)

---
### 退出容器
1. 在容器内`exit`, 如果是一个后台运行的容器, 则退出后容器`status`状态依然是`up`即挂起状态(*对此容器的任何更改依然保存*)<br>
2. 在容器内`exit`, 如果不是一个后台容器, 则退出后容器`status`状态是`exited`即退出状态(*对此容器的任何更改依然保存*)<br>
3. 保存对容器的修改: `docker commit container_id your_image_name`, 这样会保存对容器的修改, 并生成一个`your_image_name`名字的镜像

---
### 启动一个退出的容器或关闭一个挂起的容器
1. `docker container start container_id`
根据容器`id`启动`status`为`exited`的容器为`up`状态
2. `docker container stop container_id`
根据容器`id`关闭`status`为`up`的容器为`exited`状态

---
### 删除容器与镜像
1. `docker rm container_id`
根据容器`id`删除一个容器<br>
*注意此操作后在此容器内的任何更改将不再存在*<br>
2. `docker rmi image_id`
根据镜像`id`删除一个镜像<br>

---
### 启用sshd服务
1. `apt install openssh-server`安装ssh server端<br>
2. `vim /etc/ssh/sshd_config`进入配置文件, 找到`PermitRootLogin`项, 添加一行`PermitRootLogin yes`, 保存退出<br>
2. 查看`/etc/init.d/ssh`是否存在, 如果存在, 使用`/etc/init.d/ssh start`启动服务<br>
3. 使用`ps`或`netstat`查看服务是否启动<br>
4. 一般docker登录后默认是root用户且没有设置过密码, 这里需要使用`passwd root`给root用户设置密码<br>
5. 在物理机使用`ssh root@127.0.2.1`登录ssh-server, 并输入刚设置的密码即可<br>
6. 注意启用容器时是否开启`--net=host`或`-p 22:22`进行过端口映射<br>
7. 如果在centos中`/etc/init.d/ssh`不存在, 先键入`yum install initscripts`安装`service`模块, 再重新安装`openssh-server`<br>

---
### 其他
1. `docker images`
查看所有镜像<br>
2. `docker tag image_id new_repository:new_tag`
给某个镜像重命令<br>
3. `docker ps -a`
查看所有容器, 包括`status`为`up`和`exited`的容器<br>
4. `docker rm $(docker ps -aq)`
批量删除容器, `$()` 取出`()`内的执行结果，`shell`语法<br>