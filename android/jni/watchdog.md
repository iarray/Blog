JNI代码:
```c++

#include "com_android_hardware_watchdog.h"
#define  LOG_TAG    "watch_dog"
#define  LOGI(...)  __android_log_print(ANDROID_LOG_INFO,LOG_TAG,__VA_ARGS__)
#define  LOGE(...)  __android_log_print(ANDROID_LOG_ERROR,LOG_TAG,__VA_ARGS__)

JNIEXPORT jint JNICALL Java_com_android_hardware_WatchDog_init(JNIEnv *env, jobject obj, jint interval, jint margin)
{
    LOGI("started (interval %d, margin %d)!\n", interval, margin);

    int fd = open(DEV_NAME, O_RDWR|O_CLOEXEC);
    if (fd == -1) {
        LOGE("Failed to open %s: %s\n", DEV_NAME, strerror(errno));
        return fd;
    }

    int timeout = interval + margin;
    int ret = ioctl(fd, WDIOC_SETTIMEOUT, &timeout);
    if (ret) {
        LOGE("Failed to set timeout to %d: %s\n", timeout, strerror(errno));
        ret = ioctl(fd, WDIOC_GETTIMEOUT, &timeout);
        if (ret) {
            LOGE("Failed to get timeout: %s\n", strerror(errno));
        } else {
            if (timeout > margin) {
                interval = timeout - margin;
            } else {
                interval = 1;
            }
            LOGI("Adjusted interval to timeout returned by driver:"
                    " timeout %d, interval %d, margin %d\n",
                    timeout, interval, margin);
        }
    }
    return fd;
}

JNIEXPORT void JNICALL Java_com_android_hardware_WatchDog_feedDog (JNIEnv *env, jobject obj, jint fd)
{ 
    write(fd, "", 1);
}

```

Android.mk代码
```makefile
LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)
LOCAL_MODULE    := watchdog_jni
LOCAL_LDLIBS += -L$(SYSROOT)/usr/lib -llog
LOCAL_LDLIBS += "-L$(NDK_PATH)/platforms/android-22/arch-arm/usr/lib" -lc -lgcc -lOpenSLES -llog -lGLESv2 -lEGL -landroid
LOCAL_SRC_FILES += watchdog.cpp
LOCAL_ARM_MODE := arm
include $(BUILD_SHARED_LIBRARY)

```


Java代码
```java

package com.android.hardware;

public class WatchDog {
    /**
     *  初始化看门狗(interval+margin秒内不喂狗就重启)
     * @param interval interval:看门狗时间如5秒
     * @param margin margin 时间间隙如3秒
     * @return
     */
    public final static native int init(int interval, int margin);

    /**
     * 喂狗
     * @param fd
     * @return
     */
    public final static native int feedDog(int fd);
    static{
        System.loadLibrary("watchdog_jni");
    }
}


```

调用实例
```java

public class MyApplication extends Application {
    private int fd = -1;
    private Handler handler = new Handler(Looper.getMainLooper()){
        @Override
        public void handleMessage(Message msg) {
            if (msg.what == 0)
            {
                if (fd>0) {
                    WatchDog.feedDog(fd);
                    handler.sendEmptyMessageDelayed(0, 10000);
                }
            }
            super.handleMessage(msg);
        }
    } ;

    @Override
    public void onCreate() {
        super.onCreate();

        fd = WatchDog.init(45,15);
        if (fd>0){
            WatchDog.feedDog(fd);
            Log.i("hph", "看门狗启动成功");
            handler.sendEmptyMessage(0);
        }
        else {
            Log.i("hph", "看门狗启动失败");
        }
    }
}

```
