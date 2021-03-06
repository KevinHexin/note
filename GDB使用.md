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
可见，使用行号设置断点是更方便的做法<br>

4. 设置触发条件<br>
`break file:line if var >= 1` 在`file`文件`line`行处打上断点, 同时当变量`var`的值大于等于`1`时才会触发<br>

5. 跳过`n`次触发, 在第`n+1`次触发断点时才进入断点<br>
`ignore break n` 断点`break`前`n`次触发都跳过, 在`n+1`次触发断点时才进入断点<br>
`info break` 可以查看断点`break`被触发了多少次<br>

### 汇编调试
1. 先正常打一个断点, 以跳到关心的函数处<br>

2. `disas func_name` 显示反汇编, 这时会看到一个双杠箭头, 指向了当前汇编指令位置<br>

3. `break *0x00005621833359a2` 即在指令地址处打上断点<br>

### 执行
1. `gdb attach pid`的方式进入已经运行的进程, 进入后，程序将被阻塞，直到:<br>
A. `continue` 继续运行<br>
B. `run` 重新启动程序<br>

2. `gdb program`的方式<br>
会先加载符号表，然后run开始启动程序<br>

3. `set scheduler-locking off|on|step`, 在使用`step`或者`continue`命令调试当前被调试线程的时候，其他线程也是同时执行的，怎么只让被调试程序执行呢？通过这个命令就可以实现这个需求:<br>
A. `off`不锁定任何线程，也就是所有线程都执行，这是默认值<br>
B. `on`只有当前被调试程序会执行<br>
C. `step`在单步的时候，除了`next`过一个函数的情况(熟悉情况的人可能知道，这其实是一个设置断点然后`continue`的行为)以外，只有当前线程会执行<br>

4. `info threads`会打印所有的线程, 同时每个线程都会有一个`被括号包起来的线程号`, `线程号最小`的是第一个创建的线程(主线程), 线程号从小到大依次是先后创建的线程<br
如果是`gdb program core`即gdb加载的`core`文件, 会有一个`小点来标识`崩溃在哪个线程<br

### 打印变量print
1. `print variable`或简写`p variable`将直接打印变量`variable`内存储的值<br>

2. 如果出现`No symbol "variable" in current context`:<br>
A. 编译时是否加入`-g`选项<br>
B. `variable`变量是否在当前栈帧中, 如果不是, 执行`up 1`向上跳到正确的栈帧处<br>
参考:<br>
https://blog.csdn.net/sj_djw/article/details/91958035<br>

3. 如果需要打印虚表指针值, `print *obj`即可打印对象指针虚表指针信息<br>

### next/step/print等更多用法，参见
`/word/gdb-tutorial-handout.pdf`<br>

### GDB break无法触发的情况
1. 编译程序/动态库/静态库时未开启`-g`选项<br>

2. 开启了子线程，但是主线程没有调用`thread.join()`或`while(1)`操作, 导致主线程退出，此时可能无法触发断点<br>

3. 给动态库打断点的时候，`gdb program` / `gdb attach pid` 没有在动态库所在文件夹执行，而是在其他文件夹执行gdb命令<br>