The terms concurrency and parallelism are often used in relation to multithreaded programs. But what exactly does concurrency and parallelism mean, and are they the same terms or what?

The short answer is "no". They are not the same terms, although they appear quite similar on the surface. It also took me some time to finally find and understand the difference between concurrency and parallelism. Therefore I decided to add a text about concurrency vs. parallelism to this Java concurrency tutorial.  

==术语并发和并行经常用于与多线程程序关联。但是并发和并行到底意为着什么呢？它们是不是一样的呢或者怎样？==  
==简而言之，它们是不同的。它们不是相同的术语，尽管表面看起来它们很相似。我也花费了写时日去找它们的不同。一次我决定在java并发系列中添加并发并行对比的文章。==

### Concurrency
Concurrency means that an application is making progress on more than one task at the same time (concurrently). Well, if the computer only has one CPU the application may not make progress on more than one task at exactly the same time, but more than one task is being processed at a time inside the application. It does not completely finish one task before it begins the next.  
### 并发
==并发是一个应用同时处理多个任务。好，如果单个CPU的计算机也许不能同时执行多个任务，但是在应用内部多个任务被处理，当开始一个新任务前，并没有完成当前任务。==
![image](http://tutorials.jenkov.com/images/java-concurrency/concurrency-vs-parallelism-1.png)  

### Parallelism
Parallelism means that an application splits its tasks up into smaller subtasks which can be processed in parallel, for instance on multiple CPUs at the exact same time.    
### 并行
==并行是应用把任务分割成可以并行处理的子任务。例如，在多CPU上同时执行==
![image](http://tutorials.jenkov.com/images/java-concurrency/concurrency-vs-parallelism-2.png)  

### Concurrency vs. Parallelism In Detail
As you can see, concurrency is related to how an application handles multiple tasks it works on. An application may process one task at at time (sequentially) or work on multiple tasks at the same time (concurrently).  
### 并发与并行细节对比
==如你所见，并发与程序如何处理多任务有关。应用也许处理一个任务（顺序地），也许同时处理多个任务（并发地）==

Parallelism on the other hand, is related to how an application handles each individual task. An application may process the task serially from start to end, or split the task up into subtasks which can be completed in parallel.  
==另一方面，并行与应用如何处理独立的任务有关。应用也许从头到尾连续地处理，或者被分割成可以被并行完成的子任务。==
As you can see, an application can be concurrent, but not parallel. This means that it processes more than one task at the same time, but the tasks are not broken down into subtasks.  
==你看到了，一个程序可以并发，但不能够并行。这意味着应用同时处理多个任务，但是不能分割任务为子任务。==

An application can also be parallel but not concurrent. This means that the application only works on one task at a time, and this task is broken down into subtasks which can be processed in parallel.  
==应用也可以并行的而非并发的。在某一时刻，应用只执行一个任务，并且该任务被分解成并行执行的子任务。==

Additionally, an application can be neither concurrent nor parallel. This means that it works on only one task at a time, and the task is never broken down into subtasks for parallel execution.  
==此外，程序可以既不是并发也不是并行的。意思是程序在在某一个时刻只处理一个任务，这个任务不能分解为并行的子任务。==

Finally, an application can also be both concurrent and parallel, in that it both works on multiple tasks at the same time, and also breaks each task down into subtasks for parallel execution. However, some of the benefits of concurrency and parallelism may be lost in this scenario, as the CPUs in the computer are already kept reasonably busy with either concurrency or parallelism alone. Combining it may lead to only a small performance gain or even performance loss. Make sure you analyze and measure before you adopt a concurrent parallel model blindly.  
==最后，应用可是即是并发的，有时并行的，并发并行在同时处理多个任务，也可以把每个任务分解为子任务以便并行执行。然而，这种方案并发与并行的某些优点会丢失掉，由于计算机中的cpu已经合理地忙于并发性或并行性。组合它可能只会带来一个小的性能增益，甚至是性能损失。在盲目采用并行并行模型之前，一定要分析和度量。==
