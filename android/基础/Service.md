# Android Service

* 服务的两种启动方式 :
	* startService()
	* bindService()
	
## 1.生命周期
* 当启动一个Service的时候，会调用该Service中的`onCreate()`和`onStartCommand()`方法。
* `onCreate()`方法只会在Service第一次被创建的时候调用,多次调用`startService`启动Service并不会触发`onCreate`方法
* 调用`stopService`方法可以停止服务，如果服务没有绑定到Activity上，则会自动调用`onDestroy()`销毁方法


## 2.Activity和Service关联(bindService方式启动服务)
* ServiceConnection
	* `onServiceConnected(ComponentName name, IBinder binder)` 建立关联的时候调用
	* `onServiceDisconnected(ComponentName name)` 解除关联的时候调用
	
* `bindService(bindIntent, connection, BIND_AUTO_CREATE)`  将Activity和Service进行绑定

		这里传入BIND_AUTO_CREATE表示在Activity和Service建立关联后自动创建Service，
		这会使得Service中的onCreate()方法得到执行，但onStartCommand()方法不会执行。

* `unbindService（connection）` 解除Activity和Service的绑定

实现步骤:
  

1. 创建一个Binder类继承Binder类，里面可以实现一些方法，例如：
  
		class MyBinder extends Binder {   
		    public void startDownload() {  
		        new Thread(new Runnable() {  
		            @Override  
		            public void run() {  
		                // 执行具体的下载任务  
		            }  
		        }).start();  
		    }   
		}  

2. 创建一个类实现ServiceConnection接口，下面例子创建一个匿名类:

		private ServiceConnection connection = new ServiceConnection() {  
	  
	        @Override  
	        public void onServiceDisconnected(ComponentName name) {  
	        }  
	  
	        @Override  
	        public void onServiceConnected(ComponentName name, IBinder service) {  
	            myBinder = (MyService.MyBinder) service;  
	            myBinder.startDownload();  
	        }  
		};  
    
3. 服务里面重写onBind方法,返回binder的实例:

		@Override  
	    public IBinder onBind(Intent intent) {  
	        return mBinder;  
	    } 

4. 接下来通过调用`bindService`建立关联，建立成功后就会触发`onServiceConnected`方法，执行下载动作.


> Service的onStart() 只有采用Context.startService()方法启动服务时才会回调该方法。该方法在服务开始运行时被调用。多次调用startService()方法尽管不会多次创建服务，但onStart() 方法会被多次调用
> 
> Service的onBind()方法只有采用Context.bindService()方法启动服务时才会回调该方法。该方法在调用者与服务绑定时被调用，当调用者与服务已经绑定，多次调用Context.bindService()方法并**不会**导致该方法被多次调用。


## 3.如何销毁Service
如果我们既调用了`startService`，又调用了`bindService`会怎么样呢？这个时候你会发现，不管你是单独`stopService`还是`unbindService`，Service都不会被销毁，必要两个都调用，Service才会被销毁。也就是说`stopService`只会让Service停止，`unbindService`只会让Service和Activity解除关联，

> **一个Service必须要在既没有和任何Activity关联又处理停止状态的时候才会被销毁。**

## 4.Service和Thread
* **Service服务是运行在主线程上的，即UI线程**。如果在服务中执行长时间的同步操作，会阻塞UI线程,一般的做法是在服务上建立子线程。

* Android的后台就是指，它的运行是完全不依赖UI的。即使Activity被销毁，或者程序被关闭，只要进程还在，Service就可以继续运行。

* 既然在Service里也要创建一个子线程，那为什么不直接在Activity里创建呢？这是因为Activity很难对Thread进行控制，当Activity被销毁之后，就没有任何其它的办法可以再重新获取到之前创建的子线程的实例。而且在一个Activity中创建的子线程，另一个Activity无法对其进行操作。但是Service就不同了，**所有的Activity都可以与Service进行关联**，然后可以很方便地操作其中的方法，即使Activity被销毁了，之后只要重新与Service建立关联，就又能够获取到原有的Service中Binder的实例。因此，使用Service来处理后台任务，Activity就可以放心地finish，完全不需要担心无法对后台任务进行控制的情况。


## 5.远程Service 
	使用远程Service甚至可以实现Android跨进程通信的功能

将一个普通的Service转换成远程Service其实非常简单，只需要在注册Service的时候将它的android:process属性指定成:remote就可以了
	  
	<?xml version="1.0" encoding="utf-8"?>  
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"  
        package="com.example.servicetest"  
        android:versionCode="1"  
        android:versionName="1.0" >  
      
        ......  
          
        <service  
            android:name="com.example.servicetest.MyService"  
            android:process=":remote" >  
        </service>  
      
    </manifest>  

上面讲到服务是运行在主线程上的，如果在服务中进行耗时操作，会导致UI线程（主线程）阻塞,过一段时间并会弹出ANR对话框。之前我们提到过，应该在Service中开启线程去执行耗时任务。这里我们建立一个远程Service并且在`OnCreate()`方法里面执行`Thread.sleep(60000);` 延时60秒。

上面我们知道Service是运行在主线程的，延时60秒的操作是会阻塞主线程的。但是奇怪的现象出现了，界面并没有出现“没有响应”的对话框，这是为什么呢？

这是由于，使用了远程Service后，MyService已经在**另外一个进程当中运行了**，所以只会阻塞该进程中的主线程，并不会影响到当前的应用程序。

我们分别在MainActivity的onCreate()方法和MyService的onCreate()方法里加入一行日志，打印出各自所在的进程id，如下所示：
	    
	Log.d("TAG", "process id is " + Process.myPid());
	输出结果:
	process id is 31172
	process id is 31231  

从结果可知服务运行在另一个进程上了。注意：远程Service不能通过上面的bindService方法将Activity和服务关联，原因很简单Activity和Service不在同一进程上，那要如何关联？这就要使用AIDL来进行跨进程通信了（IPC）。

## 6.服务跨进程通信
AIDL（Android Interface Definition Language）是Android接口定义语言的意思，它可以用于让某个Service与多个应用程序组件之间进行跨进程通信，从而可以实现多个应用程序共享同一个Service的功能。

首先需要新建一个AIDL文件，在这个文件中定义好**Activity需要与Service进行通信的方法**。新建MyAIDLService.aidl文件，代码如下所示：

	package com.example.servicetest;  
	interface MyAIDLService {  
	    int add(int a, int b);  
	    String toUpperCase(String str);  
	} 

如果想要让Activity与Service之间建立关联，需要调用bindService()方法，并将Intent作为参数传递进去，在Intent里指定好要绑定的Service但是在另一个应用程序中去绑定Service的时候并没有MyService这个类，这时就必须使用到[隐式Intent](https://iarray.github.io/blog/android/intent)

然后创建一个新的Android项目，起名为ClientTest，我们就尝试在这个程序中远程调用MyService中的方法。

ClientTest中的Activity如果想要和MyService建立关联其实也不难，首先需要将`MyAIDLService.aidl`文件从上面的项目中拷贝过来，**注意要将原有的包路径一起拷贝过来**。新建一个按钮来执行服务关联，代码如下:
	
	Button bindService = (Button) findViewById(R.id.bind_service);  
        bindService.setOnClickListener(new OnClickListener() {  
            @Override  
            public void onClick(View v) {  
                Intent intent = new Intent("com.example.servicetest.MyAIDLService");  
                bindService(intent, connection, BIND_AUTO_CREATE);  
            }  
    });  

匿名类实现服务连接接口：

    private ServiceConnection connection = new ServiceConnection() {  
        @Override  
        public void onServiceDisconnected(ComponentName name) {  
        }  
  
        @Override  
        public void onServiceConnected(ComponentName name, IBinder service) {  
            myAIDLService = MyAIDLService.Stub.asInterface(service);  
            try {  
                int result = myAIDLService.add(50, 50);  
                String upperStr = myAIDLService.toUpperCase("comes from ClientTest");  
                Log.d("TAG", "result is " + result);  
                Log.d("TAG", "upperStr is " + upperStr);  
            } catch (RemoteException e) {  
                e.printStackTrace();  
            }  
        }  
    };  

由于这是在不同的进程之间传递数据，Android对这类数据的格式支持是非常有限的，基本上只能传递Java的基本数据类型、字符串、List或Map等。那么如果我想传递一个自定义的类该怎么办呢？这就必须要让这个类去实现**Parcelable**接口，并且要给这个类也定义一个同名的AIDL文件。

总结步骤:
  
1. 定义接口服务间通信的接口AIDL,并把文件aidl拷贝到另一个项目中
  
2. mainfest对服务添加属性android:process=":remote",并为其增加一个action
     
3. 在另一个程序上通过创建隐式Intent，调用`bindService(intent)`将隐式意图传进来，完成关联.