### 启动
1. 在`docker`环境中, 启动`docker`时需要添加`--privileged`参数<br>

2. `gdb mytest` 在一个gdb会话中加载mytest程序, 注意, 此时mytest程序并未启动, 只是加载进一个`gdb会话`中<br>

3. `gdb attach PID` 根据进程ID号进入gdb<br>

### 设置断点
1. 编译时添加-g选项<br>
`gcc/g++ -g`选项控制`GDB调试级别`<br>
如果不打开`-g或者-ggdb(GDB专用)调试`开关，GCC编译时不会加入调试信息，因为这会增大生成代码的体积。GCC采用了分级调试，通过在-g选项后附加数字1、2或3来指定在代码中加入调试信息量。默认的级别是2（-g2），此时调试信息包括扩展的符号表、行号、局部或外部变量信息。级别3（- g3）包含级别2中的调试信息和源代码中定义的宏。级别1（-g1）不包含局部变量和与行号有关的调试信息，只能用于回溯跟踪和堆栈转储之用。[ 回溯跟踪指的是监视程序在运行过程中的函数调用历史，堆栈转储则是一种以原始的十六进制格式保存程序执行环境的方法，两者都是经常用到的调试手段。<br>

2. 函数是类方法，需要<br>
`break namespace::class::func` 给func函数添加断点<br>
即需要完整的命名空间+类名+方法名<br>

3. 如果是行号，需要<br>
`break file.cpp:30` 给file.cpp的第30行打上断点<br>
注意，文件名和行号之间是单冒号`":"`<br>

4. 可见，使用行号设置断点是更方便的做法<br>

### 执行
1. `gdb attach pid`的方式进入已经运行的进程<br>
A. `continue` 继续运行<br>
B. `run` 重新启动程序<br>

2. `gdb program`的方式<br>
会先加载符号表，然后run开始启动程序<br>

### next/step/print等更多用法，参见
`/word/gdb-tutorial-handout.pdf`<br>

### GDB break无法触发的情况
1. 未开启-g选项<br>

2. 开启了子线程，但是主线程没有调用thread.join()或while(1)操作, 导致主线程退出，此时可能无法触发断点<br>

3. 给动态库打断点的时候，gdb program / gdb attach pid 没有在动态库所在文件夹执行，而是在其他文件夹执行gdb命令<br>