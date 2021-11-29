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
```
```