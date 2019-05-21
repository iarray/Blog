# BroadCast 广播
	在Android中广播其实就是观察者模式的实现

## 关键的两个方法
* registerReceiver(BroadcastReceiver,IntentFilter) 事件订阅
* sendBroadcast(Intent) 发布事件 

## 事件接收处理的实现
1. 继承类`BroadcastReceiver`,并重写`onReceive(Context context, Intent intent)`方法。
 
2. 当事件被发布的时候，就会调用订阅了该事件的BroadcastReceiver类的onReceive方法

## 代码实现
* 事件接收者

		class BroadcastHandler extends BroadcastReceiver {
			@Override
			public void onReceive(Context context, Intent intent) {
				String action = intent.getAction(); 
				if (action.equalsIgnoreCase("MyEvent")){
					//这里处理事件				
				}
			}
		}

* 事件订阅
	
		BroadcastHandler mBroadcastHandler = new BroadcastHandler();
		IntentFilter filter = new IntentFilter();
		filter.addAction("MyEvent");
		registerReceiver(mBroadcastHandler, filter);


以上完成了事件监听的实现，下面就是如何触发事件

* 事件发布
	
		Intent intent = new Intent();
        intent.setAction("MyEvent"); 
		sendBroadcast(intent);

广播其实没有什么复杂的东西,需要注意的是`onReceive`方法是运行在主线程上的，注意不要阻塞UI