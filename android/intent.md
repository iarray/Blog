# Intent 意图
	Intent是不同组件之间相互通讯的纽带，封装了不同组件之间通讯的条件。

## Google Doc对Intent的解释

**当接收到ContentResolver发出的请求后，内容提供者被激活。而其它三种组件──activity、服务和广播接收器被一种叫做intent的异步消息所激活。**

* Intent是一个保存消息内容的类。
  

* 对于activity和服务来说，它指明了请求的操作名称以及作为操作对象的数据的URI和其它一些信息。比如说，它可以承载对一个activity 的请求，让它为用户显示一张图片，或者让用户编辑一些文本。通过传递一个Intent对象至 Context.startActivity()或Activity.startActivityForResult()以载入（或指定新工作给）一个activity。
  

* 对于广播接收器而言(**事件接收者Listener**)，Intent对象指明了声明的行为(**传递给事件的参数EventArgs**)。应用程序可以凭借将Intent对象传递给 Context.sendBroadcast() ，Context.sendOrderedBroadcast()， 以及Context.sendStickyBroadcast()和其它类似方法来产生一个广播(**事件触发FireEvent**)。
  

* 对于服务而言，它和activity类似，都是用来启动的时候传入一些消息内容，用法：通过传递一个Intent对象至Context.startService()将启动一个服务（或给予正在运行的服务以一个新的指令）。本质上：Android调用服务的onStart()方法并将Intent对象传递给它。

## Intent的传递方式
* 显式传递

	直接设置目标组件的ComponentName，用于一个应用内部的消息传递，比如启动另一个Activity或者一个services。通过Intent的`setComponent`和`setClass`来制定目标组件的ComponentName。
	例如：
		
		Intent intent = new Intent();  
        intent.putExtra("testIntent", "testExtra");  
        intent.setClass(MainActivity.this,SecondActivity.class);

	或者通过构造函数设值：

        Intent it = new Intent(MainActivity.this,SecondActivity.class);    

* 隐式传递

	通过在<activity>标签下配置<intent-filter>的内容，可以指定当前活动能够响应的action 和category，打开AndroidManifest.xml，添加如下代码： 

	    <activity android:name=".activity2" >   
          <intent-filter>   
              <action android:name="com.example.activitytest.ACTION_START" />   
              <category android:name="android.intent.category.DEFAULT" />   
			  <category android:name="com.example.activitytest.MY_CATEGORY"/>
          </intent-filter>   
		</activity>   

	在这之前需要注意的是 <action>和<category>中的内容**同时能够匹配**上Intent中指定的`action`和`category`时，这个活动才能响应该Intent。然后在MainActivity 中跳转时候写上： 

	    Intent intent= new Intent("com.example.activitytest.ACTION_START");   
		intent.addCategory("com.example.activitytest.MY_CATEGORY");  

	通过构造函数声明了`action`，并调用方法`addCategory`添加上类型

    使用隐式Intent，不仅可以启动自己程序内的活动，还可以启动其他程序的活动，这使得Android多个应用程序之间的功能共享成为了可能。比如调用浏览器打开一个页面：
		
		Intent intent = new Intent(Intent.ACTION_VIEW);   
		intent.setData(Uri.parse("http://www.baidu.com"));   
		
		
		startActivity(intent);    