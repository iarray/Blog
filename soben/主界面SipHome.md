# 主界面SipHome
## onCreate
* 启动松本服务 SobenService
* 启动日志服务 LogService
* 启动QQ物联服务 TXDeviceService

## NotifyReceiver 广播通知接收
## dispatchKeyEvent 按键处理
* 包com.csipsimple.ui.dialpad中的DialerFragment负责呼叫的处理
* 呼叫处理方法 placeCallWithOption
	* 呼叫中心管理机
	* 呼叫普通门口机
	* 呼叫手Q