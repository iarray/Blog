JNI代码
1. 头文件
```c++
/* DO NOT EDIT THIS FILE - it is machine generated */
#include <jni.h>
/* Header for class com_example_liang_gpio_demo_Gpio */

#ifndef _Included_com_example_liang_gpio_demo_Gpio
#define _Included_com_example_liang_gpio_demo_Gpio
#ifdef __cplusplus
extern "C" {
#endif
/*
 * Class:     com_example_liang_gpio_demo_Gpio
 * Method:    Set_GPIO
 * Signature: ()I
 */
JNIEXPORT jint JNICALL Java_com_soben_hardware_Gpio_exportGpio(JNIEnv *env, jobject instance, jint gpio);
JNIEXPORT jint JNICALL Java_com_soben_hardware_Gpio_setGpioDirection(JNIEnv *env, jobject instance, jint gpio, jint direction);
JNIEXPORT jint JNICALL Java_com_soben_hardware_readGpioStatus(JNIEnv *env, jobject instance, jint gpio);
JNIEXPORT jint JNICALL Java_com_soben_hardware_writeGpioStatus(JNIEnv *env, jobject instance, jint gpio, jint value);
JNIEXPORT jint JNICALL Java_com_soben_hardware_unexportGpio(JNIEnv *env, jobject instance, jint gpio);
#ifdef __cplusplus
}
#endif
#endif


```

2.源码
```c++
#include"com_soben_hardware_Gpio.h"
#include <unistd.h>
#include"Logger.h"
#include <stdio.h>
#include <linux/input.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <string.h>

#define IN              0
#define OUT             1
#define LOW             0
#define HIGH            1
#define BUFFER_MAX      3
#define DIRECTION_MAX   48

/*
    导出IO口
*/
JNIEXPORT jint JNICALL Java_com_soben_hardware_Gpio_exportGpio(JNIEnv *env, jobject instance, jint gpio) {
    char buffer[BUFFER_MAX];
    int len;
    int fd;
    fd = open("/sys/class/gpio/export", O_WRONLY);
    if (fd < 0) {
        LOGE("Failed to open export for writing!\n");
        return(0);
    }
    len = snprintf(buffer, BUFFER_MAX, "%d", gpio);
    if (write(fd, buffer, len) < 0) {
        LOGE("Fail to export gpio!\n");
        return 0;
    }
    close(fd);
    return 1;
}

/*
    设置IO为输入或输出
 */
 JNIEXPORT jint JNICALL Java_com_soben_hardware_Gpio_setGpioDirection(JNIEnv *env, jobject instance, jint gpio, jint direction) {
    static const char dir_str[] = "in\0out";
    char path[DIRECTION_MAX];
    int fd;
    snprintf(path, DIRECTION_MAX, "/sys/class/gpio/gpio%d/direction", gpio);
    fd = open(path, O_WRONLY);
    if (fd < 0) {
        LOGE("failed to open gpio direction for writing!\n");
        return 0;
    }
    if (write(fd, &dir_str[direction == IN ? 0 : 3], direction == IN ? 2 : 3) < 0) {
        LOGE("failed to set direction!\n");
        return 0;
    }
    close(fd);
    return 1;
}

 /*
 * 读取IO口
 */
 JNIEXPORT jint JNICALL Java_com_soben_hardware_readGpioStatus(JNIEnv *env, jobject instance, jint gpio) {
    char path[DIRECTION_MAX];
    char value_str[3];
    int fd;
    snprintf(path, DIRECTION_MAX, "/sys/class/gpio/gpio%d/value", gpio);
    fd = open(path, O_RDONLY);
    if (fd < 0) {
        LOGE("failed to open gpio value for reading!\n");
        return -1;
    }
    if (read(fd, value_str, 3) < 0) {
        LOGE("failed to read value!\n");
        return -1;
    }
    close(fd);
    return (atoi(value_str));
 }

 /*
 * 输出IO
 */
 JNIEXPORT jint JNICALL Java_com_soben_hardware_writeGpioStatus(JNIEnv *env, jobject instance, jint gpio, jint value) {
    static const char values_str[] = "01";
    char path[DIRECTION_MAX];
    int fd;
    snprintf(path, DIRECTION_MAX, "/sys/class/gpio/gpio%d/value", gpio);
    fd = open(path, O_WRONLY);
    if (fd < 0) {
        LOGE("failed to open gpio value for writing!\n");
        return 0;
    }
    if (write(fd, &values_str[value == LOW ? 0 : 1], 1) < 0) {
        LOGE("failed to write value!\n");
        return 0;
    }
    close(fd);
    return 1;
 }

  /*
  * 取消导出
  */
  JNIEXPORT jint JNICALL Java_com_soben_hardware_unexportGpio(JNIEnv *env, jobject instance, jint gpio) {
    char buffer[BUFFER_MAX];
    int len;
    int fd;
    fd = open("/sys/class/gpio/unexport", O_WRONLY);
    if (fd < 0) {
        LOGE("Failed to open unexport for writing!\n");
        return 0;
    }
    len = snprintf(buffer, BUFFER_MAX, "%d", gpio);
    if (write(fd, buffer, len) < 0) {
        LOGE("Fail to unexport gpio!");
        return 0;
    }
    close(fd);
    return 1;
 }


```

Java封装代码
```Java
package com.soben.hardware;

import com.soben.hardware.tools.ShellTools;

/**
 *  Auth: HPH
 *  Date: 2018-12-7
 *
 * 1. 查看GPIO状态
 *  cat /sys/kernel/debug/gpio
 * 2. 导出IO口
 *  echo 引脚号>/sys/class/gpio/export
 * 3. 设置输入输出
 *  echo out>/sys/class/gpio/gpio254/direction      设为输出
 *  echo in>/sys/class/gpio/gpio254/direction       设为输入
 * 4. 设值
 *  echo 1>/sys/class/gpio/gpio254/value/1
 *
 * 5.如果上面操作不通过,先设置权限
 *  chmod 0666 /sys/class/gpio/export
 *  chmod 0666 /sys/class/gpio/unexport
 *  chmod 0666 /sys/class/gpio/gpio引脚号/direction
 *  chmod 0666 /sys/class/gpio/gpio引脚号/value
 *
 *  引脚号说明:
 *      如果使想用GPIO8_9
 *      那么GPIO8_9 Number就是 8 x 32 + 9 = 265
 */
public class Gpio
{
    static {
        System.loadLibrary("gpio_jni");
    }

    public static final int IN = 0;
    public static final int OUT= 1;


    /**
     * 导出Gpio
     * @param gpio
     * @return
     */
    public final static native int exportGpio(int gpio);

    /**
     * 设置输入/输出
     * @param gpio
     * @param direction 0输入 1输出
     * @return
     */
    public final static native int setGpioDirection(int gpio, int direction);

    /**
     * 读取IO口状态
     * @param gpio
     * @return
     */
    public final static native int readGpioStatus(int gpio);

    /**
     * 写入IO状态
     * @param gpio
     * @param value
     * @return
     */
    public final static native int writeGpioStatus(int gpio, int value);

    /**
     * 取消导出
     * @param gpio
     * @return
     */
    public final static native int unexportGpio(int gpio);


    private int pin;
    private Process mProcess;

    public Gpio(int pin)
    {
        this.pin = pin;
        ShellTools.execCmd("chmod 777 /sys/class/gpio/export", true);
        ShellTools.execCmd("echo "+pin+">/sys/class/gpio/export", true);
    }


    public boolean setGpioDirectionByShell(int direction)
    {
         ShellTools.CommandResult result = ShellTools.execCmd("echo " + (direction == 0?"in":"out") +">/sys/class/gpio/gpio"+pin+"/direction", true);
         return result.result == 0;
    }

    public boolean writeByShell(int value)
    {
        ShellTools.CommandResult result = ShellTools.execCmd("echo " + value + ">/sys/class/gpio/gpio" + pin + "/value", true);
        return result.result == 0;
    }

    /**
     * 读取Gpio值, 返回-1则读取失败
     * @return
     */
    public int read()
    {
        ShellTools.CommandResult result = ShellTools.execCmd("cat /sys/class/gpio/gpio" + pin + "/value", true);
        if(result.result != 0){
            return -1;
        }
        else {
            try {
                int value = Integer.valueOf(result.successMsg);
                return value;
            }
            catch (Exception e){
                return -1;
            }
        }
    }
}



```
