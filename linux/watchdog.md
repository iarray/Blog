## [WatchDog](http://blog.csdn.net/liigo/article/details/9227205) 

Linux 自带了一个 watchdog 的实现，用于监视系统的运行，包括一个内核 watchdog module 和一个用户空间的 watchdog 程序。内核 watchdog 模块通过 /dev/watchdog 这个字符设备与用户空间通信。用户空间程序一旦打开 /dev/watchdog 设备（俗称“开门放狗”），就会导致在内核中启动一个1分钟的定时器（系统默认时间），此后，用户空间程序需要保证在1分钟之内向这个设备写入数据（俗称“定期喂狗”），每次写操作会导致重新设定定时器。如果用户空间程序在1分钟之内没有写操作，定时器到期会导致一次系统 reboot 操作（“狗咬人了”呵呵）。通过这种机制，我们可以保证系统核心进程大部分时间都处于运行状态，即使特定情形下进程崩溃，因无法正常定时“喂狗”，Linux系统在看门狗作用下重新启动（reboot），核心进程又运行起来了。**多用于嵌入式系统**。

### 1. 打开 /dev/watchdog 设备（“开门放狗”）：

	int fd_watchdog = open("/dev/watchdog", O_WRONLY);  
	if(fd_watchdog == -1) {  
	    int err = errno;  
	    printf("\n!!! FAILED to open /dev/watchdog, errno: %d, %s\n", err, strerror(err));  
	    syslog(LOG_WARNING, "FAILED to open /dev/watchdog, errno: %d, %s", err, strerror(err));  
	}  

### 2.每隔一段时间向 /dev/watchdog 设备写入数据（“定期喂狗”）：

	//feed the watchdog  
	if(fd_watchdog >= 0) {  
	    static unsigned char food = 0;  
	    ssize_t eaten = write(fd_watchdog, &food, 1);  
	    if(eaten != 1) {  
	        puts("\n!!! FAILED feeding watchdog");  
	        syslog(LOG_WARNING, "FAILED feeding watchdog");  
	    }  
	}  

### 3.关闭 /dev/watchdog 设备，通常不需要这个步骤：

	close(fd_watchdog);  

### 4. ioctl API
所有标准的驱动也应该支持一个ioctl API。

	//使用SETTIMEOUT ioctl命令改变watchdog的超时值,以秒为单位
	int timeout = 45;
	ioctl(fd_watchdog, WDIOC_SETTIMEOUT, &timeout);
	//如果设备的超时值的粒度只能到分钟，则这个例子可能实际打印
	//"The timeout was set to 60 seconds"。

	//可以查询超时值
	ioctl(fd_watchdog, WDIOC_GETTIMEOUT, &timeout);


更多

	#define    WDIOF_OVERHEAT       0x0001    /* Reset due to CPU overheat */
	#define    WDIOF_FANFAULT       0x0002    /* Fan failed */
	#define    WDIOF_EXTERN1        0x0004    /* External relay 1 */
	#define    WDIOF_EXTERN2        0x0008    /* External relay 2 */
	#define    WDIOF_POWERUNDER     0x0010    /* Power bad/power fault */
	#define    WDIOF_CARDRESET      0x0020    /* Card previously reset the CPU */
	#define    WDIOF_POWEROVER      0x0040    /* Power over voltage */
	#define    WDIOF_SETTIMEOUT     0x0080  /* Set timeout (in seconds) */
	#define    WDIOF_MAGICCLOSE     0x0100    /* Supports magic close char */
	#define    WDIOF_PRETIMEOUT     0x0200  /* Pretimeout (in seconds), get/set */
	#define    WDIOF_KEEPALIVEPING  0x8000    /* Keep alive ping reply */
	
	#define    WDIOS_DISABLECARD    0x0001    /* Turn off the watchdog timer */
	#define    WDIOS_ENABLECARD     0x0002    /* Turn on the watchdog timer */
	#define    WDIOS_TEMPPANIC      0x0004    /* Kernel panic on temperature trip */