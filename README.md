# multi_threading
《C++并发编程实战》的读书笔记，供以后工作中查阅。
## 第一章
- 何谓并发和多线程

并发：单个系统里同时执行多个独立的活动。

多线程：每个线程相互独立运行，且每个线程可以运行不同的指令序列。但进程中所有线程都共享相同的地址空间，并且从所有的线程中访问到大部分数据。

- 为什么要在应用程序中使用并发和多线程

关注点分离（DVD程序逻辑分离）和性能（加快程序运行速度）

- 一个简单的C++多线程程序是怎么样的

[清单1.1 一个简单的Hello,Cuncurrent World程序](https://github.com/xuyicpp/multi_threading/blob/master/chapter01/example1_1.cpp)

## 第二章
- 启动线程，以及让各种代码在新线程上运行的方法

多线程在分离detach的时候，离开局部函数后，会在后台持续运行，直到程序结束。如果仍然需要访问局部函数的变量（就会造成悬空引用的错误）。
[清单2.1 当线程仍然访问局部变量时返回的函数](https://github.com/xuyicpp/multi_threading/blob/master/chapter02/example2_1.cpp)
解决上述错误的一个常见的方式，使函数自包含，并且把数据复制到该线程中而不是共享数据。

std::thread是支持移动的，如同std::unique_ptr是可移动的，而非可复制的。以下是两个转移thread控制权的例子
[清单2.5 从函数中返回std::thread,控制权从函数中转移出](https://github.com/xuyicpp/multi_threading/blob/master/chapter02/example2_5.cpp)、[清单2.6 scoped_thread和示例用法,一旦所有权转移到该对象其他线程就不就可以动它了，保证退出一个作用域线程完成](https://github.com/xuyicpp/multi_threading/blob/master/chapter02/example2_6.cpp)
- 等待线程完成并让它自动运行

在当前线程的执行到达f末尾时，局部对象会按照构造函数的逆序被销毁，因此，thread_guard对象g首先被销毁。所以使用thread_guard类可以保证std::thread对象被销毁前，在thread_guard析构函数中调用join。
[清单2.3 使用RAII等待线程完成](https://github.com/xuyicpp/multi_threading/blob/master/chapter02/example2_3.cpp)

- 唯一地标识线程

线程标识符是std::thread::id类型的
1.通过与之相关联的std::thread对象中调用get_id()。
2.当前线程的标识符可以调用std::this_thread::get_id()获得。