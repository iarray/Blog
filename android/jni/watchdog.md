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

Java代码

