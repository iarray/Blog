# ADB 常用指令
* ADB可以用于安卓和PC机连接交互，比如安卓程序，拷贝文件等等下面介绍一些常用的ADB命令

## 1.查看设备信息
	adb devices	
	
	结果:
	emulator-5554  devices

## 2.拷贝文件到设备
	adb push 源文件路径 目标文件路径

	如果电脑连接了多个设备，需要指定设备号
	adb -s emulator-5554 push C:\1.txt \sdcard\1.txt

## 3.从设备拷贝文件到电脑
	用法和上面的push命令相似

	adb pull 源文件路径 目标文件路径

3,4操作如果提示read-only, 请先挂载一下,即执行 `adb remount`

## 4.安装APK到设备上
	adb install apk文件路径
	
	强制安装
	adb install -r apk文件路径  
	 
## 5.卸载应用
	adb uninstall packagename

## 6.重启ADB

* 有时候ADB和设备会失去连接，这个情况下可以重启adb服务，做法如下：

		adb kill-server  : 结束adb服务器
		adb start-server : 开启adb服务

## 7.进入设备Linux环境
	adb shell
	
	进入指定设备
	adb -s emulator-5554 shell

	查看设备目录结构
	adb shell ls -l

	查看系统运行日志
	adb shell logcat

* 可以通过adb后面加上 -s 设备号，指定对哪台设备执行adb指令

## 8.双清
	1.adb shell
	2.su
	3.wipe data
	4.wipe cache
	5.exit
	6.exit
	7.adb reboot

## 9.重启到Recovery界面
	adb reboot recovery

## 10.重启到bootloader界面
	adb reboot bootloader

## 11. 修改分辨率
	adb shell wm size 480x800 
以上不能用试下
	adb shell am display-size 720x1280

重置分辨率

	adb shell wm size reset

## 12. 查找文件	
	
	adb shell
	
	busybox find . -name "filename"
