# GJpush

#### 介绍
极光推送集成模板库，将代码拉到你的工程中作为一个lib库，并在app/build.gradle中配置你的appkey和applicationid即可使用


#### 使用Library Manager插件一键集成
集成方式1： Android Studio->Library Manager->Home Page->GJpush->integrate 一键集成，需要安装Library Manager插件  
集成方式2：使用Library Manager terminal安装：view->tool windows->Library Manager打开terminal，输入alm install GJpush -v 4.3.0安装  
集成方式3：直接经代码clone到你的工程项目中，并手动配置app/build.gradle中极光推送appkey等相关信息，并配置好依赖关系  

暂不支持厂商推送，如果需要请手动配置厂商账号信息和厂商依赖插件包，详细教程请参考极光推送官网

#### 使用说明

在代码中注册
```java
import android.app.ActivityManager;
import android.app.Application;
import android.content.Context;
import android.util.Log;
import com.base.thirdlibs.utils.SpUtil;
import com.thirdlib.jpush.DeviceIdUtil;
import com.thirdlib.jpush.JpushManager;
import com.thirdlib.jpush.JpushMessageListener;
import java.util.List;
import cn.jpush.android.api.CustomMessage;
import cn.jpush.android.api.NotificationMessage;

public class MyApplication extends Application {

    private static final String TAG = MyApplication.class.getSimpleName();

    @Override
    public void onCreate() {
        super.onCreate();
        Log.e(TAG, "onCreate");
        String processName = getProcessName(this, android.os.Process.myPid());
		//avoid initializing multiple times
        if (getApplicationInfo().packageName.equals(processName)) {
            init();
        }
    }

    /**
     * @return null may be returned if the specified process not found
     */
    public static String getProcessName(Context cxt, int pid) {
        ActivityManager am = (ActivityManager) cxt.getSystemService(Context.ACTIVITY_SERVICE);
        List<ActivityManager.RunningAppProcessInfo> runningApps = am.getRunningAppProcesses();
        if (runningApps == null) {
            return null;
        }
        for (ActivityManager.RunningAppProcessInfo procInfo : runningApps) {
            if (procInfo.pid == pid) {
                return procInfo.processName;
            }
        }
        return null;
    }


    private void init() {
        JpushManager.getInstance().init(getApplicationContext());
        JpushManager.getInstance().setJpushMessageListener(new JpushMessageListener(){
            @Override
            public void onMessage(Context context, CustomMessage customMessage) {
                super.onMessage(context, customMessage);
                Log.e(TAG, "test onMessage");
            }

            @Override
            public void onNotifyMessageArrived(Context context, NotificationMessage notificationMessage) {
                super.onNotifyMessageArrived(context, notificationMessage);
                Log.e(TAG, "test onNotifyMessageArrived");
            }
        });
        JpushManager.getInstance().setAlias(DeviceIdUtil.getDeviceId(getApplicationContext()));
        SpUtil.getInstance().init(getApplicationContext(), "test");
    }
}
```