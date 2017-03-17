# Android Service

## 1.生命周期
* 当启动一个Service的时候，会调用该Service中的`onCreate()`和`onStartCommand()`方法。
* `onCreate()`方法只会在Service第一次被创建的时候调用,多次调用`startService`启动Service并不会触发`onCreate`方法
* 调用`stopService`方法可以停止服务，如果服务没有绑定到Activity上，则会自动调用`onDestroy()`销毁方法


## 2.Activity和Service关联
* ServiceConnection
	* `onServiceConnected()` 建立关联的时候调用
	* `onServiceDisconnected()` 解除关联的时候调用
	
* `bindService(bindIntent, connection, BIND_AUTO_CREATE)`  将Activity和Service进行绑定

		这里传入BIND_AUTO_CREATE表示在Activity和Service建立关联后自动创建Service，
		这会使得Service中的onCreate()方法得到执行，但onStartCommand()方法不会执行。

* `unbindService（connection）` 解除Activity和Service的绑定

## 3.如何销毁Service
如果我们既调用了`startService`，又调用了`bindService`会怎么样呢？这个时候你会发现，不管你是单独`stopService`还是`unbindService`，Service都不会被销毁，必要两个都调用，Service才会被销毁。也就是说`stopService`只会让Service停止，`unbindService`只会让Service和Activity解除关联，

> **一个Service必须要在既没有和任何Activity关联又处理停止状态的时候才会被销毁。**

## 4.Service和Thread
* **Service服务是运行在主线程上的，即UI线程**。如果在服务中执行长时间的同步操作，会阻塞UI线程,一般的做法是在服务上建立子线程。

* Android的后台就是指，它的运行是完全不依赖UI的。即使Activity被销毁，或者程序被关闭，只要进程还在，Service就可以继续运行。

* 既然在Service里也要创建一个子线程，那为什么不直接在Activity里创建呢？这是因为Activity很难对Thread进行控制，当Activity被销毁之后，就没有任何其它的办法可以再重新获取到之前创建的子线程的实例。而且在一个Activity中创建的子线程，另一个Activity无法对其进行操作。但是Service就不同了，**所有的Activity都可以与Service进行关联**，然后可以很方便地操作其中的方法，即使Activity被销毁了，之后只要重新与Service建立关联，就又能够获取到原有的Service中Binder的实例。因此，使用Service来处理后台任务，Activity就可以放心地finish，完全不需要担心无法对后台任务进行控制的情况。