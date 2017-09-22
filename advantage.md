The reason multithreading is still used in spite of its challenges is that multithreading can have several benefits. Some of these benefits are:  

- Better resource utilization.  
- Simpler program design in some situations.
- More responsive programs.  

==尽管多线程有很多挑战，但是多线程仍然被使用因为是有很多优点，一些优点：==
- ==更好的资源利用==
- ==某些情形下更简单的程序设计==
- ==更快速相应的程序==



### Better resource utilization

Imagine an application that reads and processes files from the local file system. Lets say that reading af file from disk takes 5 seconds and processing it takes 2 seconds. Processing two files then takes    
### 更好地利用资源
==想象一个读取、处理本地文件系统的应用。假设从磁盘读取视频文件需要5秒，处理需要2秒。处理两个文件消耗==

```
  5 seconds reading file A
  2 seconds processing file A
  5 seconds reading file B
  2 seconds processing file B
-----------------------
 14 seconds total

```

When reading the file from disk most of the CPU time is spent waiting for the disk to read the data. The CPU is pretty much idle during that time. It could be doing something else. By changing the order of the operations, the CPU could be better utilized. Look at this ordering:  
==当从硬盘读取文件时，CPU等待读取数据上花费了大部分时间。在这段时间内，CPU是很空闲的。它（CPU）可以做一些其它事情，通过改变操作顺序，这样CPU就可以得到更好的利用。看这个顺序：==

```
  5 seconds reading file A
  5 seconds reading file B + 2 seconds processing file A
  2 seconds processing file B
-----------------------
 12 seconds total
```

The CPU waits for the first file to be read. Then it starts the read of the second file. While the second file is being read, the CPU processes the first file. Remember, while waiting for the file to be read from disk, the CPU is mostly idle.  

==CPU等待第一个文件的读取，接着它开始读取第二个文件。当第二个文件正在读取，CPU处理第一个文件。记住，当等待从磁盘读取文件时，CPU几乎是空闲的==

In general, the CPU can be doing other things while waiting for IO. It doesn't have to be disk IO. It can be network IO as well, or input from a user at the machine. Network and disk IO is often a lot slower than CPU's and memory IO.    
==一般，CPU可以做其他事在等待IO时。不必一定是操作磁盘的IO操作，也可以是网络IO操作，或者在一台机器上输入用户。网络和磁盘IO通常比CPU的IO和内存IO慢很多==  

### Simpler Program Design
If you were to program the above ordering of reading and processing by hand in a singlethreaded application, you would have to keep track of both the read and processing state of each file. Instead you can start two threads that each just reads and processes a single file. Each of these threads will be blocked while waiting for the disk to read its file. While waiting, other threads can use the CPU to process the parts of the file they have already read. The result is, that the disk is kept busy at all times, reading from various files into memory. This results in a better utilization of both the disk and the CPU. It is also easier to program, since each thread only has to keep track of a single file.

==如果你在单线程应用中手工处理上述的读取和处理顺序，你必须记录每一个文件的读取和处理的状态。代替地你可以开启两个线程，分别只读取和处理单个文件。每一个线程都将被阻塞当等待读取磁盘文件。在等待时，其他线程可以使用CPU处理它早已读取的部分文件。结果是磁盘一直处于繁忙状态，读取各种文件到内存中。可以更好的利用磁盘和CPU。对于程序来说它也更加容易，每个线程只需记下单个文件记录==

#### More responsive programs
Another common goal for turning a singlethreaded application into a multithreaded application is to achieve a more responsive application. Imagine a server application that listens on some port for incoming requests. when a request is received, it handles the request and then goes back to listening. The server loop is sketched below:  

### 更快响应的程序
==另外的相同把但单线程改为多线程应用的目地是实现一个更快速响应的应用。假设一个服务应用监听某个端口发送来的请求，当请求到达，处理这个请求然后返回监听。服务的循环可以这样写：==
```
  while(server is active){
    listen for request
    process request
  }
```

This way the server thread will be back at listening sooner. Thus more clients can send requests to the server. The server has become more responsive.  
==这样的话服务器就可以更早地恢复监听。因此更多的客户端可以发送请求道服务端。服务就可以更快响应。==

The same is true for desktop applications. If you click a button that starts a long task, and the thread executing the task is the thread updating the windows, buttons etc., then the application will appear unresponsive while the task executes. Instead the task can be handed off to a worker thread. While the worker thread is busy with the task, the window thread is free to respond to other user requests. When the worker thread is done it signals the window thread. The window thread can then update the application windows with the result of the task. The program with the worker thread design will appear more responsive to the user.  
==对于桌面应用是相同的。如果你点击了一个开启耗时任务的按钮，正在执行的线程更新窗口、按钮等。紧接着，应用无法响应在任务执行的过程中。相反地，这个任务可以转化为一个工作线程。当工作线程忙于处理任务时，窗口时空闲的去响应用户的其他请求。当工作线程完成它通知窗口线程。窗口线程可以使用该任务的结果更新应用窗口。使用工作线程的程序设计可以更加快速响应用用户的操作。==
