# 安卓端接入说明

## 导入资源
-   在project下的build.gradle->repositories标签下添加仓库
```
    maven {
        url "http://47.100.116.120:9992/repository/android_public"
        credentials {
            username "user_read"
            password "read"
        }
    }
    
```
-	在app module下的build.gradle下手动添加依赖，配置如下所示：
```

    api('com.mbr.pay:lib_sdk:1.0.8.release'){
        changing=true
    }
```
-   修改app模块下的build.gradle,替换CHANNEL和MERCHANT_ID中的000000
```
defaultConfig {
        applicationId "com.mbr.dcpay.demo"
        minSdkVersion 15
        targetSdkVersion 23
        versionCode 1
        versionName "1.0"

        buildConfigField "String", "CHANNEL", "\"000000\""
        buildConfigField "String", "MERCHANT_ID", "\"000000\""
    }
```
## 支付接口调用（可参考Demo实现）
###  1.配置环境
```
        //设置当前开发环境，根据环境的不同会下载不同的apk，默认为生产环境
        EnvConfig.setEnv(EnvConfig.PRE);
```
###  2.调用支付需要传递一个orderInfo字符串，这个字符串参数来源于应用后台服务器。
*   参数名称:   orderInfo
*   参数说明:   支付请求字符串，String类型， key=value形式，以&连接。
*   参数示例:   amount=0.01&coinId=34190899187000&merchantId=10000000000003&merchantName=测试商户&orginAmount=10000000000000000&payBillNo=72911335082956&refBizNo=1525186687194&toAddr=0x91f8654587917f3a0c7cfc5fa05bd86dc0162ddb&sign=iS0vfd7pUpiCiFwZuZrbqbklT4cSF0VukZtYALyQmuAyOwjGSGdJPn235bo2rEMvXc5olaD/FiRSujvMXaYmiZZ0sxbYPFoozIdIdK8H33pGBshtoinrfXxlPytGfT+Qqq7GlN300+nb9vQ1yv0xIRyZqxtdIjelN6fPb/SAXt9DeW/CGqRRofbnQ8fB7UqLa+qX5j6dckqgFXQu9mzdZcKM+1f2ffxhgsYlUEl7g9HHWizxMSrYqRL6WRuc0BBQarq1LdCPTHLfzZ81GVg4M+5uHM9URUa7tnstPzMQVEx0cbGLmMLx+7lQWo6Pxp1TWPz8WkApABo0IpllB1jY+g==
###  3.调用支付示例代码：
```
 Pay.pay(this, orderInfo, new Pay.OnPayListener() {
            @Override
            public void onPayResult(PayResult result) {
                if (result != null && result.isSuccess()) {
                    // 发送成功
                    Log.i(TAG, "onPayResult: success");
                }
                else {
                    // 发送失败
                    String errorMsg = null;
                    if (result != null) errorMsg = result.getErrorMessage();
                    Log.w(TAG, "onPayResult: " + errorMsg);
                }
            }
        });
```
### 4.服务端同步支付结果。
本地SDK返回的支付结果表示支付请求已经成功发送，实际支付结果以后台同步的结果为准。
##  Demo
### demo地址
https://github.com/cqmbr/PaySDK-Android

