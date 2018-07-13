<!-- TOC -->

- [SDK集成](#sdk集成)
    - [使用说明](#使用说明)
    - [初始化](#初始化)
        - [初始化方法](#初始化方法方法)
        - [参数](#参数)
            - [MBRWWalletConfig](#mbrwwalletconfig)
- [Demo](#demo)
- [关键业务时序图](#关键业务时序图)
	- [支付时序图](#支付时序图)
- [SDK接口文档](#sdk接口文档)
	- [钱包接口](#钱包接口)
        - [设置当前钱包](#设置当前钱包)
        - [清空当前钱包数据](#清空当前钱包数据)
    - [密码接口](#密码接口)
        - [设置交易密码](#设置交易密码)
        - [修改密码](#修改密码)
        - [是否已设置密码](#是否已设置密码)
        - [密码是否相同](#密码是否相同)
        - [设置密码提示](#设置密码提示)
        - [获取密码提示](#获取密码提示)
    - [币接口](#币接口)
        - [同步币列表](#同步币列表)
        - [获取所有币列表](#获取所有币列表)
        - [获取必选币列表](#获取必选币列表)
        - [获取非必选币列表](#获取非必选币列表)
        - [获取矿工费默认值](#获取矿工费默认值)
        - [获取矿工费最大值](#获取矿工费最大值)
        - [获取矿工费最小值](#获取矿工费最小值)
    - [账户接口](#账户接口)
        - [创建新账户](#创建新账户)
        - [助记词导入账户](#助记词导入账户)
        - [keystore导入账户](#keystore导入账户)
        - [删除账户](#删除账户)
        - [获取账户助记词](#获取账户助记词)
        - [是否已备份账户](#是否已备份账户)
        - [备份账户](#备份账户)
        - [导出keystore](#导出keystore)
        - [设为默认账户](#设为默认账户)
        - [重命名账户](#重命名账户)
        - [获取所有账户](#获取所有账户)
        - [获取默认账户](#获取默认账户)
        - [查找账户](#查找账户)
        - [同步账户余额信息](#同步账户余额信息)
        - [更新账户中币的选中状态](#更新账户中币的选中状态)
    - [交易接口](#交易接口)
        - [密码支付](#密码支付)
        - [密码转账](#密码转账)
- [错误码](#错误码)

<!-- /TOC -->
## SDK集成

### 使用说明

- 工具：XCode、CocoaPods
- 配置：在工程的Podfile文件里面添加以下代码：
```
  pod 'MBRWallet'
```
  保存并执行pod install,然后用后缀为.xcworkspace的文件打开工程。

### 初始化

在使用钱包功能之前必须对钱包sdk进行初始化。
建议在AppDelegate.m的application:didFinishLaunchingWithOptions:方法中初始化。

#### 初始化方法

```
+(void)setupWithConfig:(MBRWWalletConfig*)config
```

#### 参数

##### MBRWWalletConfig

参数名                       |说明             | 备注
----------------------------|----------------|-------
channel                     | 渠道号          |必选项，由钱包方分配
privateKey                  | 私钥            |必选项，RSA2048，由钱包方分配
merchantId                  | 商户id          |必选项，由钱包方分配
apiHost                     | 服务端地址       |可选项，不填默认为生产环境地址（测试环境地址：http://47.100.18.6:9900/）
languageCode                | 语言码          |可选项，默认为中文（可选值见languageCode说明）
walletMode                  | 钱包模式         |可选项，默认为单钱包模式。(单模式：0) 多模式（1）



languageCode可选值

语言码            | 备注
------------------|-------------------
zh_CN             |中文
zh_TW             |繁体中文
en_US             |英文
ja_JP             |日文
ko_KR             |韩文

walletMode可选值

钱包模式                      | 备注
----------------------------|-------------------
MBRWWalletModel_Single      |单钱包模式
MBRWWalletModel_Muti        |多钱包模式（使用场景举例：接入方需要给每个登录用户设置单独的钱包）

## Demo
- Demo地址
https://github.com/cqmbr/MBRWallet-iOS.git

- Demo使用说明
    1、打开终端，在Sample目录执行如下命令：
    ```
    pod udpate
    ```
    2、打开WalletDemo.xcworkspace运行

## 关键业务时序图

### 支付时序图

![avatar](https://raw.githubusercontent.com/cqmbr/MBRDocument/master/docs/MBRWallet/%E5%86%85%E7%BD%AE%E9%92%B1%E5%8C%85SDK%E6%94%AF%E4%BB%98%E6%B5%81%E7%A8%8B.png)

## SDK接口文档

### 钱包接口

#### 设置当前钱包
```java
/**
 设置当前钱包（如果使用的多钱包模式，需要给钱包设置一个唯一id，id由接入方控制，应用内需要唯一）
 
 @param wId 钱包id
 @param err 错误信息
 */
+ (void)setCurrentWalletWithId:(NSString*)wId error:(NSError *__autoreleasing *)err;
```

#### 清空当前钱包数据
```java
/**
 清空当前钱包数据(会清除钱包内的密码，账户等所有数据，请慎用)
 
 @param err 错误信息
 */
+ (void)clearCurrentWalletWithError:(NSError *__autoreleasing *)err;
```

### 密码接口

#### 设置交易密码

```java
/**
 设置交易密码

 @param pwd 密码
 @param err 错误
 */
+ (void)setPassword:(NSString*)pwd error:(NSError *__autoreleasing *)err;
```

#### 修改密码

```java
/**
 修改密码

 @param oldPwd 旧密码
 @param newPwd 新密码
 @param success 修改成功回调
 @param failure 修改失败回调
 */
+ (void)modifyPasswordWithOldPwd:(NSString *)oldPwd
                          newPwd:(NSString*)newPwd
                         success:(void (^)(void))success
                         failure:(MBRWFailureBlock)failure;
```

#### 是否已设置密码

```java
/**
 是否已设置密码
 - 使用账户操作等需要密码权限校验的接口前应该先设置密码
 - 设置密码接口调用：[MBRWWallet setPassword:pwd error:&error];

 @return YES=已设置；NO=未设置
 */
+ (BOOL)haveSetPassword;
```

#### 密码是否相同

```java
/**
 密码是否相同（校验传入密码是否与设置的密码相等）

 @return YES = 相等；NO = 不相等
 */
+ (BOOL)isSamePassword:(NSString*)pwd;
```

#### 设置密码提示

```java
/**
 设置密码提示

 @param hint 密码提示文本
 */
+ (void)setPasswardHint:(NSString*)hint;
```

#### 获取密码提示

```java
/**
 获取密码提示

 @return 密码提示文本
 */
+ (NSString*)getPasswordHint;
```

### 币接口

#### 同步币列表

```java
/**
 同步币列表
 
 @param callBack 结果回调（success=YES:同步成功）

 */
+ (void)syncAllERC20CoinList:(void (^)(BOOL success))callBack;
```

#### 获取所有币列表

```java
/**
 获取所有币列表
 
 @return 币数组
 */
+ (NSArray<MBRBgCoin*>*)getAllCoins;
```

#### 获取必选币列表

```java
/**
 获取必选币列表

 @return 币数组
 */
+ (NSArray<MBRBgCoin*>*)getForceCoins;
```

#### 获取非必选币列表

```java
/**
 获取非必选币列表

 @return 币数组
 */
+ (NSArray<MBRBgCoin*>*)getNomalCoins;
```

#### 获取矿工费默认值

```java
/**
 获取矿工费默认值
 
 @param coinId 币Id
 @return 价格（转换后的价格：单位：eth)
 */
+ (NSDecimalNumber*)defaultEthWithCoinId:(NSString*)coinId;
```

#### 获取矿工费最大值

```java
/**
 矿工费最大值
 
 @param coinId 币Id
 @return 价格（转换后的价格：单位：eth)
 */
+ (NSDecimalNumber*)maxEthWithCoinId:(NSString*)coinId;
```

#### 获取矿工费最小值

```java
/**
 矿工费最小值
 
 @param coinId 币Id
 @return 价格（转换后的价格：单位：eth)
 */
+ (NSDecimalNumber*)minEthWithCoinId:(NSString*)coinId;
```

### 账户接口

#### 创建新账户

```java
/**
 创建新账户

 @param name 账户名称
 @param pwd 交易密码
 @param success 操作成功回调
 @param failure 操作失败回调
 */
+ (void)addNewAccountWithName:(NSString*)name
                          pwd:(NSString *)pwd
                      success:(MBRWAccountOprationSuccessBlock)success
                      failure:(MBRWFailureBlock)failure;
```

#### 助记词导入账户

```java
/**
 助记词导入账户

 @param mnemonic 助记词
 @param pwd 交易
 @param name 账户名称
 @param success 操作成功回调
 @param failure 操作失败回调
 */
+ (void)importAccountByMnemonic:(NSString*)mnemonic
                            pwd:(NSString*)pwd
                           name:(NSString*)name
                        success:(MBRWAccountOprationSuccessBlock)success
                        failure:(MBRWFailureBlock)failure;
```

#### keystore导入账户

```java
/**
 keystore导入账户

 @param keyStore keystore
 @param pwd 密码
 @param pwdKS keystore密码
 @param name 账户名称
 @param success 操作成功回调
 @param failure 操作失败回调
 */
+ (void)importAccountByKeystore:(NSString*)keyStore
                            pwd:(NSString*)pwd
                          pwdKS:(NSString*)pwdKS
                           name:(NSString*)name
                        success:(MBRWAccountOprationSuccessBlock)success
                        failure:(MBRWFailureBlock)failure;
```

#### 删除账户

```java
/**
 删除账户

 @param name 账户名
 @param pwd 密码
 @param success 操作成功回调
 @param failure 操作失败回调
 */
+ (void)removeAccount:(NSString*)name
                  pwd:(NSString*)pwd
              success:(MBRWAccountOprationSuccessBlock)success
              failure:(MBRWFailureBlock)failure;
```

#### 获取账户助记词

```java
/**
 获取账户助记词

 @param address 账户地址
 @param pwd 密码
 @param err 错误
 @return 助记词文本
 */
+ (NSString*)getMnemonic:(NSString*)address
                     pwd:(NSString*)pwd
                   error:(NSError *__autoreleasing *)err;
```

#### 是否已备份账户

```java
/**
 是否已备份账户
 
 @param address 账户地址
 @return YES：账户已经备份过助记词或者账户是由keystore导入的
 */
+ (BOOL)isAccountBackup:(NSString*)address;
```

#### 备份账户

```java
/**
 备份账户
 
 @param address 账户地址
 @param pwd 校验密码
 @param mnemonic 校验助记词
 @param success 操作成功
 @param failure 操作失败
 */
+ (void)backupAccount:(NSString*)address
                  pwd:(NSString*)pwd
             mnemonic:(NSString*)mnemonic
              success:(MBRWAccountOprationSuccessBlock)success
              failure:(MBRWFailureBlock)failure;
```

#### 导出keystore

```java
/**
 导出keystore
 
 @param address 账户地址
 @param pwd 账户密码
 @param ksPwd keystory密码
 @param success 导出成功回调
 @param failure 导出失败回调
 */
+ (void)exportKeystore:(NSString*)address
                   pwd:(NSString*)pwd
                 ksPwd:(NSString*)ksPwd
               success:(void (^)(NSString *json))success
               failure:(MBRWFailureBlock)failure;
```

#### 设为默认账户

```java
/**
 设为默认账户

 @param address 地址
 @param pwd 密码
 @param success 操作成功回调
 @param failure 操作失败回调
 */
+ (void)setAsDefaultAccount:(NSString*)address
                        pwd:(NSString*)pwd
                    success:(MBRWAccountOprationSuccessBlock)success
                    failure:(MBRWFailureBlock)failure;
```

#### 重命名账户

```java
/**
 重命名账户

 @param name 账户名
 @param newName 新名称
 @param pwd 交易密码
 @param success 操作成功回调
 @param failure 操作失败回调
 */
+ (void)renameAccount:(NSString*)name
              newName:(NSString*)newName
                  pwd:(NSString*)pwd
              success:(MBRWAccountOprationSuccessBlock)success
              failure:(MBRWFailureBlock)failure;
```

#### 获取所有账户

```java
/**
 获取所有账户

 @return MBRWAccount数组
 */
+ (NSArray<MBRWAccount*>*)getAllAccount;

```

#### 获取默认账户

```java
/**
 获取默认账户

 @return MBRWAccount
 */
+ (MBRWAccount*)getDefaultAccount;

```

#### 查找账户

```java
/**
 查找账户
 
 @param address 地址
 @return MBRWAccount
 */
+ (MBRWAccount*)getAccountWithAddress:(NSString*)address;
```

#### 同步账户余额信息

```java
/**
 同步账户余额信息
 
 @param complete 同步完成回调
 */
+ (void)syncAllAccountBalance:(void(^)(BOOL success))complete;
```

#### 更新账户中币的选中状态

```java
/**
 更新账户中币的选中状态
 
 @param coin MBRBgCoin
 @param address 账户地址
 @param callBack 完成回调
 */
+ (void)updateAccountCoinSelectSatate:(MBRBgCoin *)coin
                     toAccountAddress:(NSString *)address
                             callBack:(void (^)(BOOL success))callBack;
```

### 交易接口

#### 支付

```java
/**
 密码支付
 
 @param param 参数
 @param pwd 密码
 @param success 支付成功回调
 @param failure 支付失败回调
 */
+ (void)payWithPrarm:(MBRWPayParam*)param
            password:(NSString*)pwd
             success:(MBRWTransactionSuccessBlock)success
             failure:(MBRWFailureBlock)failure;
```

#### 转账

```java
/**
 密码转账
 
 @param param 参数
 @param pwd 密码
 @param success 支付成功回调
 @param failure 支付失败回调
 */
+ (void)transferWithPrarm:(MBRWTransferParam*)param
            password:(NSString*)pwd
             success:(MBRWTransactionSuccessBlock)success
             failure:(MBRWFailureBlock)failure;
```

## 错误码
错误码使用示例：
```java
+ (NSString*)parseError:(NSError*)error {

    NSString *errorCOde = nil；
    //判断是否钱包错误码
    if ([error mbrw_isWalletError]) {
        // 获取错误码
        NSString *errorCOde = [error mbrw_errorCode];
    }
        
    return errorCOde;    
}    
```

错误                                 |含义               | 备注
------------------------------------|------------------|-------------------
WALLET_UNKNOWN_ERROR                |  未知错误                    | 
PASSWORD_EMPTY_ERROR                |  输入的密码为空               |  
PASSWORD_NO_ERROR                   |  钱包交易密码不存在，未设置     |  无密码，需先设置密码
PASSWORD_SET_FAIL_FOR_ALREADY_SET   |  密码已经存在                 |
PASSWORD_ERROR                      |  密码错误                    | 
PASSWORD_MODIFY_NEWPWD_SAME_TO_OLDPWD_ERROR|  密码一样             | 修改密码时，密码也原密码一样
ACCOUNT_NAME_EMPTY_ERROR            |  账号名为空                   |
ACCOUNT_NAME_DUPLICATE_ERROR        |  账户名已经存在               |
ACCOUNT_DUPLICATE_ERROR             |  账户地址已存在               |  如：重复添加同一个keystore账户时，会抛出此异常
ACCOUNT_NOT_EXIST_ERROR             |  账户不存在                   |
MNEMONIC_EMPTY_ERROR                |  助记词导入，助记词为空         |
MNEMONIC_ERROR                      |  导入时，助记词不匹配          |
KEYSTORE_EMPTY_ERROR                |  keystore导入，keystore为空   |
KEYSTORE_PWD_EMPTY_ERROR            |  keystore导入，keystore密码为空|
KEYSTORE_ERROR                      |  keystore错误                 |
PAY_PRAMAM_ERROR                    |  支付参数错误                 |
PAY_BALANCE_NOT_ENOUGH_ERROR        |  余额不足                     |  账户的余额不足
PAY_FEE_NOT_ENOUGH_ERROR            |  矿工费不足                   |  账户的余额不足以支付矿工费
FINGER_UNAVAILABLE_ERROR            |  设备不支持touchid            |
FINGERPRINT_ERROR                   |  touchid验证失败              |
ACCOUNT_COIN_NOT_EXIST_ERROR        |  账户中代币不存在              |
NO_WALLET_ERROR_CODE            	|  没有设置当前钱包            |多钱包模式，若未设置钱包id，调用API的情况下会检测此配置
WALLET_MODE_NOT_SUPPORTED_CODE      |  用户模式不支持              |
WALLET_DELETE_FAIL_CODE        		|  钱包数据删除失败              |