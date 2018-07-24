# 安卓端接入说明

## 导入资源
-	将DCPaySDK.jar包放入应用工程根目录的libs目录下
-	进入应用工程的“Project Structure”，在app module下选择“File dependency”，将libs目录下的DCPaySDK.jar导入。
    或者在app module下的build.gradle下手动添加依赖，配置如下所示：
```

    dependencies {
        compile files('libs/DCPaySDK.jar')
    }
```
## 支付接口调用（可参考Demo实现）
###  1.调用支付需要传递一个orderInfo字符串，这个字符串参数来源于应用后台服务器。
*   参数名称:   orderInfo
*   参数说明:   支付请求字符串，String类型， key=value形式，以&连接。
*   参数示例:   amount=0.01&coinId=34190899187000&merchantId=10000000000003&merchantName=测试商户&orginAmount=10000000000000000&payBillNo=72911335082956&refBizNo=1525186687194&toAddr=0x91f8654587917f3a0c7cfc5fa05bd86dc0162ddb&sign=iS0vfd7pUpiCiFwZuZrbqbklT4cSF0VukZtYALyQmuAyOwjGSGdJPn235bo2rEMvXc5olaD/FiRSujvMXaYmiZZ0sxbYPFoozIdIdK8H33pGBshtoinrfXxlPytGfT+Qqq7GlN300+nb9vQ1yv0xIRyZqxtdIjelN6fPb/SAXt9DeW/CGqRRofbnQ8fB7UqLa+qX5j6dckqgFXQu9mzdZcKM+1f2ffxhgsYlUEl7g9HHWizxMSrYqRL6WRuc0BBQarq1LdCPTHLfzZ81GVg4M+5uHM9URUa7tnstPzMQVEx0cbGLmMLx+7lQWo6Pxp1TWPz8WkApABo0IpllB1jY+g==
###  2.调用支付示例代码：
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
### 3.服务端同步支付结果。
本地SDK返回的支付结果表示支付请求已经成功发送，实际支付结果以后台同步的结果为准。

代码示例：
```
private Handler mHandler = new Handler() { 
    public void handleMessage(Message msg) { 
        Result result = new Result((String) msg.obj);
        Toast.makeText(DemoActivity.this, result.getResult(), Toast.LENGTH_LONG).show(); 
    }; 
};
```
### 异步通知
商户需要提供一个http协议的接口，将接口访问地址告知支付服务方。支付最终结果将通过访问该接口告知商户后台。异步通知在链处理了支付请求后进行，共将访问8次，若在某次访问的返回数据的body中包含了success，则不再进行访问。
对于异步通知的信息通过服务端SDK的httpPayCallBack方法进行处理。
