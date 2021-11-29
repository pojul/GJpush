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