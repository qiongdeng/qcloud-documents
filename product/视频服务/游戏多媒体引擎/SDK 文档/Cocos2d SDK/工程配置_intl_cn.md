欢迎使用腾讯云游戏多媒体引擎 SDK 。为方便 Cocos2D 开发者调试和接入腾讯云游戏多媒体引擎产品 API，这里向您介绍适用于 Cocos2D 开发的工程配置

## SDK 准备

请在 [下载指引](https://cloud.tencent.com/document/product/607/18521) 下载相关 Demo 及 SDK。

### 解压 SDK 资源

1. 解压获取到的 SDK 资源
2. 文件夹内容如下：

|文件夹名称                     | 文件夹详情
| ----------------------|-----------------------------------        |
| TMG_SDK                    |游戏音视频 SDK framework 文件        |
| TMGCocosDemo          |游戏音视频 SDK 示例工程                        |

## 系统要求

SDK 支持在 Mac 系统上编译。

## iOS Xcode 预备工作

### 1. 导入 SDK 相关的 framework 文件

需要将 framework 添加到 Xcode 工程中并设置头文件引用位置。

TMG_SDK 文件夹里面有 GMESDK.framework 游戏音视频 SDK framework 文件，必须添加到工程中。

### 2. 添加依赖库

参考下图：  
![](https://main.qcloudimg.com/raw/b6156b8c7a596248c148607070e38f67.png)

## Android 预备工作

### 1. 将 tmgsdk.jar 加入到 libs 库中。

![](https://main.qcloudimg.com/raw/fe1bde45a15f273aa9b9707420bb2696.png)

### 2. Activity 中导入 so 文件

```
public class AppActivity extends Cocos2dxActivity {
    static final String TAG = "AppActivity";
    static OpensdkGameWrapper gameWrapper ;
    static {
        Log.e(TAG, "Load so begin");
        System.loadLibrary("stlport_shared");
        System.loadLibrary("xplatform");
        System.loadLibrary("UDT");
        System.loadLibrary("qav_tlssign");
        System.loadLibrary("traeimp-armeabi-v7a");
        System.loadLibrary("qavsdk");
        Log.e(TAG, "Load so end");
    }
}
```

### 3. 初始化

在 oncreate 函数中进行初始化，顺序不能出错。
```
protected void onCreate(Bundle savedInstanceState) {
        super.setEnableVirtualButton(false);
        super.onCreate(savedInstanceState);

        //初始化， 顺序不能错
        AVChannelManager.setIMChannelType(AVChannelManager.IMChannelTypeImplementInternal);
        gameWrapper = new OpensdkGameWrapper(this);
        runOnGLThread(new Runnable() {
            @Override
            public void run() {
                gameWrapper.initOpensdk();
            }
        });
}
```
