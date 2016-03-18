title : iOS学习笔记之GCD
date : 2016-03-18 09:25:00 
tags : 
		- iOS
		- GCD
		- 学习笔记  

		

---

### Serial Diapatch Queue 串行队列
任务相互依赖，具有明显的先后顺序的时候

```  
var queue1 : dispatch_queue_t = dispatch_queue_create("com.y.queue", DISPATCH_QUEUE_SERIAL)
```

### Concurrent Diapatch Queue 并发队列 
不会存在任务间的相互依赖，并发执行

```
var queue2 : dispatch_queue_t = dispatch_queue_create("com.y.queue", DISPATCH_QUEUE_CONCURRENT)
```

### Global Queue & Main Queue
这是系统为我们准备的2个队列：
* Global Queue 就是系统创建的Concurrent Diapatch Queue
* Main Queue 就是系统创建的位于主线程的Serial Diapatch Queue

```
var queue1 = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
var queue2 = dispatch_get_main_queue()
```

### 常用的队列方法
----
#### dispatch_set_target_queue
dispatch_set_target_queue 可以指定2个队列的优先级：

```  
dispatch_set_target_queue(queue2,queue1);
```

#### dispatch_sync
同步执行队列，会在当前线程执行队列，并且阻塞当前线程中之后运行的代码。

```
dispatch_sync(dispatch_get_main_queue()) { () -> Void in
  print("我执行完了才执行后面的代码");
}
```

#### dispatch_aync
异步执行队列，不会阻塞当前线程中之后运行的代码。

```
dispatch_sync(dispatch_get_main_queue()) { () -> Void in
  print("我不影响代码继续执行");
}
```

#### dispatch_after 
dispatch_after 用于异步延迟执行，如在主线程中延迟10秒执行。

```
dispatch_after(dispatch_time(DISPATCH_TIME_NOW, Int64(10 * Double(NSEC_PER_SEC))), dispatch_get_main_queue()) { () -> Void in
	print("我延迟10秒执行");
}
```

#### dispatch_apply
dispatch_apply, 作用是把指定次数指定的block添加到queue中, 第一个参数是迭代次数，第二个是所在的队列，第三个是当前索引.

```
dispatch_apply(10, dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0)) { (index) -> Void in
  print(index);
}
```

#### dispatch_group_*
队列组，当我们需要监听一个并发队列中，所有任务都完成了，就可以用到这个group，因为并发队列你并不知道哪一个是最后执行的，所以以单独一个任务是无法监听到这个点的，如果把这些单任务都放到同一个group，那么，我们就能通过dispatch_group_notify方法知道什么时候这些任务全部执行完成了。

```
var queue : dispatch_queue_t = dispatch_queue_create("com.y.queue", DISPATCH_QUEUE_CONCURRENT)
var group : dispatch_group_t = dispatch_group_create();
dispatch_group_async(group, queue) { () -> Void in
  print("0");
}
dispatch_group_async(group, queue) { () -> Void in
  print("1");
}
dispatch_group_async(group, queue) { () -> Void in
  print("2");
}
dispatch_group_notify(group, dispatch_get_main_queue()) { () -> Void in
  print("组里的所有任务都执行完了");
}
```

#### dispatch_suspend & dispatch_resume
队列挂起和恢复。

```  
dispatch_suspend(queue);
sleep(3)
dispatch_async(dispatch_get_main_queue()) { () -> Void in
   dispatch_resume(queue);
}
```

#### 其他
dispatch_barrier_async / dispatch_semaphore_* / dispatch_once 等。





