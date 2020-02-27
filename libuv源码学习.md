### epoll_wait()中的timeout值的设置
1. linux系统调用`epoll_wait()`函数中，通过设置`timeout`值以控制函数返回的时机(0:立即返回，>0:延时一段时间后返回，-1:一直阻塞，直到有事件触发)<br>
一般系统设计如下：<br>
```cpp
while(1)
{
    n = epoll_wait();
    if(n > 0)
    {
        // do something
    }
    other_things_func();   // do other things
}
```

2. 在下面这篇文章中，讨论了`epoll_wait()`与系统设计间效率的关系：<br>
https://zhuanlan.zhihu.com/p/29292114<br>
A. 如果`epoll_wait()`中`timeout`设置为0, 则有cpu空转的问题<br>
B. 如果`timeout`设置为大于0的值, 则可能请求/数据可读/数据可写不能及时处理<br>
C. 没有说明`timeout=-1`的情况<br>

3. 如果不考虑`other_things_func()`, 则`timeout=-1`似乎能够解决上述`A B点`问题, 如果考虑`other_things_func()`, 则使用链接文章推荐的做法可行<br>

### libuv中的timeout值的设置
1. 查看源码`uv_run::uv__io_poll()`函数：<br>
```cpp
void uv__io_poll(uv_loop_t* loop, int timeout)
{
    // ...
    nfds = epoll_wait(loop->backend_fd,
                      events,
                      ARRAY_SIZE(events),
                      timeout);
    // ...
}
```
可以看到`timeout`设置到了`epoll_wait()`函数<br>

1. 设置`uv_idle`后, 我们知道`uv_loop`会不停的返回, 查看源码中`uv_run::uv_backend_timeout()`函数(返回值为`timeout`值):<br>
```cpp
int uv_backend_timeout(const uv_loop_t* loop)
{
  if (loop->stop_flag != 0)
    return 0;
  if (!uv__has_active_handles(loop) && !uv__has_active_reqs(loop))
    return 0;
  if (!QUEUE_EMPTY(&loop->idle_handles))   // idle设置后
    return 0;
  if (!QUEUE_EMPTY(&loop->pending_queue))
    return 0;
  if (loop->closing_handles)
    return 0;

  return uv__next_timeout(loop);
}
```
`if(!QUEUE_EMPTY(&loop->idle_handles))`会返回`true`, 则`timeout=0`<br>

2. 如果没有设置`uv_idle`(函数中其它if语句也不会触发返回0)，则查看源码`uv_run::uv_backend_timeout::uv__next_timeout()`函数：<br>
```cpp
int uv__next_timeout(const uv_loop_t* loop)
{
  const struct heap_node* heap_node;
  const uv_timer_t* handle;
  uint64_t diff;
  heap_node = heap_min(timer_heap(loop));
  if (heap_node == NULL)   // 如果没有设置定时器
    return -1; /* block indefinitely */
  handle = container_of(heap_node, uv_timer_t, heap_node);
  if (handle->timeout <= loop->time)
    return 0;
  diff = handle->timeout - loop->time;
  if (diff > INT_MAX)
    diff = INT_MAX;

  return (int) diff;
}
```
如果没有设置定时器，则`timeout=-1`, 则`uv_loop`会一直阻塞直到有`IO事件`触发<br>
