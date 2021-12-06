# GJpush

#### describe
jpush integrated template library, pull the code into your project as a lib library, and configure your jpush appkey and applicationid in app/build.gradle


#### integrate
Integration mode 1： Android Studio->Library Manager->Home Page->GJpush->integrate For one click integration, you need to install the library manager plug-in  
Integration mode 2：Installing using library manager terminal：view->tool windows->Library Manager to open terminal and enter alm install GJpush -v 4.3.0 and install  
Integration mode 3：Directly to your project through the code clone, manually configure the jpush appkey and other related information in app/build.gradle, and configure the dependencies  

Vendor push is not supported at present. If necessary, please manually configure vendor account information and vendor dependent plug-in package. For detailed tutorials, please refer to the official website of jpush

#### instructions
Register in code
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