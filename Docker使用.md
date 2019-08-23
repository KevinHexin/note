### 安装docker环境
`docker`类似虚拟机, 使用`docker`开启一个`image`镜像就类似使用虚拟机开启一个系统, 这个系统运行在`docker`容器内.<br>

### 下载与打包镜像
1. `docker pull repository:tag`
根据`repository`与`tag`下载远程仓库的镜像(如`docker hub`)<br>
2. `docker save -o /your_path/docker_image.tar image_id`
根据镜像`id`打包保存到本地<br>
3. `docker load --input /your_path/docker_image.tar`
重新加载打包的本地镜像<br>

### 启动一个容器
`docker run --runtime=nvidia -itd --name xxx -v /local_path/:/mount_path/ repository:tag /bin/bash`<br>
启动一个后台容器, 返回`container`容器`id`<br>
`docker ps`<br>
查看所有正在后台运行的容器<br>
`docker exec -it container_id /bin/bash`<br>
根据容器id进入一个容器<br>
1. `run` 指令是`docker`启动一个容器的命令
2. `--runtime=nvidia` 让容器支持调用本机`GPU`资源而不必在意容器内`cuda`版本等
3. `-i` 以交互模式运行
4. `-t` 启动一个虚拟`tty`终端
5. `-d` 在后台启动一个`docker`容器并保持`run`状态, 如果不加此选项, 则直接进入容器
6. `--name` 为启动的容器命名
7. `-v` 绑定挂载地址. `:`前面是本机挂载地址, `:`后面是容器内挂载地址
8. `repository:tag` 启动`docker`容器需要指定一个`image`镜像
9. `/bin/bash` 指定`shell`解释器

### 退出容器
1. 在容器内`exit`, 如果是一个后台运行的容器, 则退出后容器`status`状态依然是`up`即挂起状态(*对此容器的任何更改依然保存*)<br>
2. 在容器内`exit`, 如果不是一个后台容器, 则退出后容器`status`状态是`exited`即退出状态(*对此容器的任何更改依然保存*)<br>

### 启动一个退出的容器或关闭一个挂起的容器
1. `docker container start container_id`
根据容器`id`启动`status`为`exited`的容器为`up`状态
2. `docker container stop container_id`
根据容器`id`关闭`status`为`up`的容器为`exited`状态

### 删除容器与镜像
1. `docker rm container_id`
根据容器`id`删除一个容器<br>
*注意此操作后在此容器内的任何更改将不再存在*<br>
2. `docker rmi image_id`
根据镜像`id`删除一个镜像<br>

### 其他
1. `docker images`
查看所有镜像<br>
2. `docker tag image_id new_repository:new_tag`
给某个镜像重命令<br>
3. `docker ps -a`
查看所有容器, 包括`status`为`up`和`exited`的容器<br>
4. `docker rm $(docker ps -aq)`
批量删除容器, `$()` 取出`()`内的执行结果，`shell`语法<br>