### c++11 内存初始化策略
#### A. 针对内置类型, 如float int
1. 使用`malloc`分配的内存里面的值是随机的
2. 使用`new`分配的内存里面的值是随机的 (`此项存疑`)
3. 使用`new()`的方式分配的内存初始化为`0`
4. 使用`new()`比直接`new`消耗更多的时间, 时间略等于使用`memset()`和遍历赋值消耗的时间
5. 栈内数据直接初始化为`0`

#### B. 针对自定义类
1. 初始化取决于类的构造函数(注意默认构造函数不做任何初始化工作)

### 如何优化内存初始化耗时(现在称主机端为host端, GPU端为device端)
#### A. 在GPU上初始化
在GPU上初始化, 并使用`cudaMemcpy()`返回初始化的数据

缺点: `cudaMemcpy()`非常耗时, 主要原因是`device`端传数据到`host`端需要经过`CPU`调度, 所以时间很慢.

#### B. 使用锁页内存(在host端new一块内存)
将host端申请的内存注册为锁页内存

```cpp
float* lockedPageMemory = new float[length];  // 在host端new一块内存
cudaSetDeviceFlags(cudaDeviceMapHost);  // 允许host内存映射到device
cudaHostRegister((void*)lockedPageMemory, length*sizeof(float), cudaHostRegisterMapped);  // 注册为锁页内存, cudaHostRegisterMapped标志将host内存映射到device
float* inDataMemory = nullptr;   // 映射到device内存的指针
cudaHostGetDevicePointer(&inDataMemory, lockedPageMemory, 0);   // 取得映射到device内存的指针, 此时不用在device中新建一块内存, 现在host与device的内存传输采用DMA的方式隐式传递
kernel_func<<<block, thread>>>(inDataMemory);   // do something
cudaDeviceSynchronize();   // 同步
cudaHostUnregister(lockedPageMemory);   // 取消注册
delete[] lockedPageMemory;    // 直接使用c++语法的释放操作
```

缺点: 此方法虽然使用了锁页内存和`DMA`, 但是`cudaHostRegister()`函数依然比较耗时

#### C. 使用锁页内存(在host端直接申请锁页内存)
在host端使用`cuda`函数直接申请锁页内存

```cpp
float* lockedPageMemory = nullptr;
float* inDataMemory = nullptr;
cudaSetDeviceFlags(cudaDeviceMapHost);   // 允许host内存映射到device
cudaHostAlloc((void**)&lockedPageMemory, length*sizeof(float), cudaHostAllocMapped);   // 直接申请锁页内存, cudaHostRegisterMapped标志将host内存映射到device
cudaHostGetDevicePointer(&inDataMemory, lockedPageMemory, 0);   // 取得映射到device内存的指针, 此时不用在device中新建一块内存, 现在host与device的数据传输采用DMA的方式隐式传递
kernel_func<<<block, thread>>>(inDataMemory);   // do something
cudaDeviceSynchronize();   // 同步
cudaFreeHost(lockedPageMemory);   // 使用cuda提供的释放操作
```

缺点: 此方法虽然使用了锁页内存和`DMA`, 但是`cudaHostRegister()`和`cudaFreeHost()`函数都比较耗时

#### D. 优化使用锁页内存的方法
1. 如果待初始化的内存每次初始化的时候大小都是固定的, 那么应该采用整个工程代码初始化的时候申请锁页内存或者注册锁页内存的方法, 在整个工程代码结束的时候释放内存, 这样就避免了频繁的调用或释放.
2. 如果`kernel`程序频繁读写`GPU`全局内存, 一种做法是结合锁页内存和常规的cuda程序写法, 因为访问`GPU`显存比通过`PCI`总线访问`host`端内存要快一些, 即:
```cpp
// ...已经申请了锁页内存, 并映射到了GPU
cudaMalloc();  // 在device端申请显存
cudaMemcpy();  // 将锁页内存copy到显存中
kernel_func<<<block, thread>>>();   // 减少频繁读写花费的时间
cudaMemcpy();  // 将结果copy回锁页内存中
```
这么做的一个原因是锁页内存会加速`cudaMemcpy()`的执行速度

### 其他
1. `cudaSetDeviceFlags(cudaDeviceMapHost)`函数只能在初始化的时候调用一次, 不能频繁调用
2. 一切以实际运行为准.

参考:<br>
https://devtalk.nvidia.com/default/topic/494425/does-anybody-have-experience-on-cudahostregister-zero-copy-memory/<br>
https://gist.github.com/lostsoulgb/2836941<br>
https://docs.nvidia.com/cuda/cuda-runtime-api/index.html<br>
https://zhuanlan.zhihu.com/p/53773183<br>
