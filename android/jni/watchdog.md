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

    
    private static Context mContext;
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

    public static Context getContext()
    {
        return mContext;
    }

    @Override
    public void onCreate() {
        super.onCreate();
        mContext = getApplicationContext();

        fd = WatchDog.init(45,15);
        if (fd>0){
            WatchDog.feedDog(fd);
            Log.i("zhongnan", "看门狗启动成功");
            handler.sendEmptyMessageDelayed(0, 10000);
        }
        else {
            Log.i("zhongnan", "看门狗启动失败");
        }

        //Thread.setDefaultUncaughtExceptionHandler(this);
        NeverCrash.init(new NeverCrash.CrashHandler() {
            @Override
            public void uncaughtException(Thread t, Throwable e) {
                e.printStackTrace();
            }
        });

        DaemonEnv.initialize(
                getApplicationContext(),  //Application Context.
                ZhongNanService.class, //刚才创建的 Service 对应的 Class 对象.
                20*1000);  //定时唤醒的时间间隔(ms), 默认 6 分钟.

        Intent Service = new Intent(this, ZhongNanService.class);
        startService(Service);

        /* 配置异常奔溃日志记录 (已更改为bugly上报异常)
        if (ActivityCompat.checkSelfPermission(this, Manifest.permission.WRITE_EXTERNAL_STORAGE) == PackageManager.PERMISSION_GRANTED) {
            CrashUtils.init( Environment.getExternalStorageDirectory());
        }
        */

        CrashReport.UserStrategy strategy = new CrashReport.UserStrategy(getApplicationContext());
        strategy.setAppVersion(AppUtils.getAppVersionName());

        Beta.autoCheckUpgrade = false;
        Beta.enableHotfix = false;
        Beta.upgradeListener = new UpgradeListener() {
            @Override
            public void onUpgrade(int ret, UpgradeInfo strategy, boolean isManual, boolean isSilence) {
                if (strategy != null) {
                    UpdateHelper.slienceUpdate(new UpdateHelper.UpdateCallBack() {
                        @Override
                        public void onStart(long total) {
                            Log.i("beat", "开始下载更新程序...");
                        }

                        @Override
                        public void onProgress(long process, long total) {

                        }

                        @Override
                        public void onFail(Exception e) {

                        }

                        @Override
                        public void onInstall() {
                            Log.i("beat", "准备安装更新...");
                            //安装前喂一次狗, 不然有可能装到一半翻车
                            if(fd!=-1)
                                WatchDog.feedDog(fd);
                        }
                    });
                } else {

                }
            }
        };

        if (AppUtils.isAppDebug()) {
            //调试
            Bugly.init(getApplicationContext(), "c7620dbcbd", true, strategy);
        }
        else {
            //正式
            Bugly.init(getApplicationContext(), "c7620dbcbd", false, strategy);
        }

        //检查更新
        Beta.checkUpgrade(false, true);

        /* Debug阶段配置内存泄漏监控 */
        //if (!LeakCanary.isInAnalyzerProcess(this)) {
        //    LeakCanary.install(this);
        //}

        //配置信息初始化
        Config.init(this);
        //数据库初始化
        DbFactory.init(this);
        //提示音播放工具初始化
        TipSound.init(this);
        //MPush推拉服务初始化
        MPush.I.initPush(this,PUBLICKEY, Config.getInstance().getMPushHost(), BuildConfig.DEBUG);
        //百度语音合成初始化
        TTSSpeechUtils.init(this);
        //人脸识别初始化
        FaceSdkFactory.getSdk(getApplicationContext()).init();

        /*
        if (!ShortcutUtils.hasShortcut(getApplicationContext()))
        {
            ShortcutUtils.addShortcut(getApplicationContext(), R.mipmap.ic_launcher);
        }
         */

        //JPushInterface.setDebugMode(true); 	// 设置开启日志,发布时请关闭日志


        /* 初始化极光推送 */
        JPushInterface.init(this);     		// 初始化 JPush
        final String alias = TagAliasOperatorHelper.getInstance().getAlias();
        if (alias != null && !alias.isEmpty()) {
            JPushInterface.setAliasAndTags(this, alias, null, new TagAliasCallback() {
                @Override
                public void gotResult(int i, String s, Set<String> set) {
                    if (i == 0)
                    {
                        Log.i("jpush", "alias set success = " + alias);
                    }
                }
            });
        }
        else{
            JPushInterface.setAliasAndTags(this, "", null, new TagAliasCallback() {
                @Override
                public void gotResult(int i, String s, Set<String> set) {
                    if (i == 0)
                    {
                        Log.i("jpush", "alias set success = <null>");
                    }
                }
            });
        }

        //JPushInterface.stopPush(this);

        /* 开机自动同步网络时间 */
        autoSyncNetWorkTime();
    }

    private void autoSyncNetWorkTime() {

    }

    /* 改为NeverCrash 处理了
    @Override
    public void uncaughtException(Thread thread, Throwable ex) {
        ex.printStackTrace();

        //未处理的异常重启进程
        AppUtils.relaunchApp(true);
    }
    */
}

```
