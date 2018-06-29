<!-- TOC -->

- [SDK集成](#sdk集成)
    - [使用说明](#使用说明)
    - [初始化](#初始化)
        - [方法](#方法)
        - [参数](#参数)
            - [MBRWalletConfigDelegate](#mbrwalletconfigdelegate)
            - [IMBRWalletNetConfigDelegate](#imbrwalletnetconfigdelegate)
            - [Language](#language)
            - [MBRWalletNetConfig](#mbrwalletnetconfig)
- [关键业务时序图](#关键业务时序图)
	- [支付时序图](#支付时序图)
- [SDK接口文档](#sdk接口文档)
    - [密码接口](#密码接口)
        - [是否有密码](#是否有密码)
        - [是否有密码提示](#是否有密码提示)
        - [获取密码提示](#获取密码提示)
        - [检查密码](#检查密码)
        - [设置密码](#设置密码)
        - [修改密码](#修改密码)
    - [币接口](#币接口)
        - [同步所有币](#同步所有币)
        - [获取所有币列表](#获取所有币列表)
        - [获取强制的币列表](#获取强制的币列表)
        - [获取非强制币列表](#获取非强制币列表)
        - [获取Gas信息](#获取gas信息)
    - [账户接口](#账户接口)
        - [自动生成账户名](#自动生成账户名)
        - [添加账户](#添加账户)
        - [通过助记词添加账号](#通过助记词添加账号)
        - [通过keystore添加账号](#通过keystore添加账号)
        - [重命名账户](#重命名账户)
        - [删除账户](#删除账户)
        - [备份账户](#备份账户)
        - [导出keystore](#导出keystore)
        - [获取助记词](#获取助记词)
        - [是否已存在此账户](#是否已存在此账户)
        - [账户名是否已存在](#账户名是否已存在)
        - [是否为默认账户](#是否为默认账户)
        - [设置默认账户](#设置默认账户)
        - [获取默认账户](#获取默认账户)
        - [根据id获取账户](#根据id获取账户)
        - [根据账户地址获取账户](#根据账户地址获取账户)
        - [根据账户地址获取账户名](#根据账户地址获取账户名)
        - [获取所有账户](#获取所有账户)
        - [获取所有账户地址](#获取所有账户地址)
        - [获取所有账户名](#获取所有账户名)
        - [同步所有账户的余额](#同步所有账户的余额)
        - [同步账户余额](#同步账户余额)
        - [更新账户的币列表选中状态](#更新账户的币列表选中状态)
    - [交易接口](#交易接口)
        - [检查支付时余额是否足够](#检查支付时余额是否足够)
        - [密码支付](#密码支付)
        - [密码转账](#密码转账)

<!-- /TOC -->
## SDK集成

### 使用说明

- 环境：AndroidStudio
- 配置：在主工程下的build.gradle中添加maven配置
```
	maven { url "http://47.100.116.120:9992/repository/android_public"}
    credentials {
        username "user_read"
        password "read"
    }
```

- 版本依赖：
```
	compile('com.mbr.android.wallet:lib_wallet:1.0.0.beta')
```

### 初始化

在使用钱包功能之前必须对钱包sdk进行初始化，建议在应用启动或者启动页中进行初始化。

#### 方法

MBRWallet.instance().init(IMBRWalletConfigDelegate delegate);

#### 参数

##### MBRWalletConfigDelegate

参数名                       |说明             | 备注
----------------------------|--------------------|-------
chain                       | 测试链：-1 ；主链：1  |可选项，默认为主链
accountNamePrefixId         | 账户的默认昵称前缀 id |可选项，接入方可调用autoAccountName()方法创建默认昵称
IMBRWalletNetConfigDelegate | 网络配置         |必选项【[查看IMBRWalletNetConfigDelegate](#imbrwalletnetconfigdelegate)】

##### IMBRWalletNetConfigDelegate

方法名                       |说明                 | 备注
----------------------------|--------------------|-------
getContext();               | 上下文  |必传项
getLanguage();              | 钱包服务端需要的语言  |可选项【[查看Language](#language)】
getMBRWalletNetConfig();    | 网络接口配置         |必选项【[查看MBRWalletNetConfig](#mbrwalletnetconfig)】

##### Language

语言                           |钱包对应值               | 备注
----------------------------- |--------------------|-------------------
Locale.SIMPLIFIED_CHINESE     |  zh_CN             |
Locale.TRADITIONAL_CHINESE    |  zh_TW             |
Locale.ENGLISH                |  en_US             |
Locale.JAPANESE               |  ja_JP             |
Locale.KOREAN                 |  ko_KR             |

##### MBRWalletNetConfig

属性                           |属性说明               | 备注
----------------------------- |--------------------|-------------------
host                          | 服务端地址           |可选项,默认为钱包地址
channel                       | 渠道号              |必选项，渠道号联系服务端人员分配

## Demo
- Demo地址：
https://github.com/cqmbr/MBRWallet-Android.git
</br>
- Demo使用说明：
    demo代码位于wallet-demo目录，使用Android Studio导入即可运行。

## 关键业务时序图

### 支付时序图

![avatar](https://raw.githubusercontent.com/cqmbr/MBRDocument/master/docs/MBRWallet/%E5%86%85%E7%BD%AE%E9%92%B1%E5%8C%85SDK%E6%94%AF%E4%BB%98%E6%B5%81%E7%A8%8B.png)

## SDK接口文档

### 密码接口

#### 是否有密码

```java
    /**
     * 是否有密码
     * @return
     */
    boolean hasPassword();
```

#### 是否有密码提示

```java
    /**
     * 是否有密码提示
     * @return 正确：true;错误：false
     */
    boolean hasReminder();
```

#### 获取密码提示

```java
    /**
     * 获取密码提示
     * @return 密码提示
     */
    String getReminder();
```

#### 检查密码

```java
    /**
     * 检查密码是否正确
     * @param password 密码
     * @return 正确：true;错误：false
     */
    boolean checkPassword(String password);
```

#### 设置密码

```java
    /**
     * 设置密码
     * @param password 钱包口令
     * @param reminder 密码提示
     * @throws MBRWalletException
     */
    void setPassword(String password,String reminder) throws MBRWalletException;
```

#### 修改密码

```java
    /**
     * 修改密码
     * @param oldPassword 旧密码
     * @param newPassword 新密码
     * @param reminder 新密码提示
     * @throws MBRWalletException
     */
    void modifyPassword(String oldPassword,String newPassword,String reminder) throws MBRWalletException;
```

### 币接口

#### 同步所有币

```java
    /**
     * 同步所有币
     * @throws MBRWalletException
     */
    void syncAllCoinList() throws MBRWalletException;
```

#### 获取所有币列表

```java
    /**
     * 获取所有币列表
     *
     * @return 币列表
     * @throws MBRWalletException
     */
    List<MBRBgCoin> getAllCoins() throws MBRWalletException;
```

#### 获取强制的币列表

```java
    /**
     * 获取强制的币列表
     *
     * @return 强制持有的币列表
     * @throws MBRWalletException
     */
    List<MBRBgCoin> getForceCoins() throws MBRWalletException;
```

#### 获取非强制币列表

```java
    /**
     * 获取非强制币列表
     * @return 非强制持有的币列表
     * @throws MBRWalletException
     */
    List<MBRBgCoin> getUnForceCoins() throws MBRWalletException;
```

#### 获取Gas信息

```java
    /**
     * 根据id获取gas实体(单位:已转换为高位(如：ETH))
     * @param coinId 币种id
     * @return Gas信息（矿工费的信息）
     * @throws MBRWalletException
     */
    MBRCoinGasInfo getCoinGasInfo(String coinId) throws MBRWalletException;
```

### 账户接口

#### 自动生成账户名

```java
    /**
     * 自动生成账户名：可调用此方法生成钱包默认的账户名
     */
    String autoAccountName();
```

#### 添加账户

```java
    /**
     * 添加账户
     *
     * @param nickname 昵称
     * @param password 密码
     * @return 返回账户
     * @throws MBRWalletException
     */
    MBRAccount addAccount(String nickname, String password) throws MBRWalletException;
```

#### 通过助记词添加账号

```java
    /**
     * 通过助记词添加账号
     * @param nickname 昵称
     * @param mnemonic 助记词
     * @param password 密码
     * @return 返回账户
     * @throws MBRWalletException
     */
    MBRAccount addAccountWithMnemonic(String nickname, String mnemonic, String password) throws MBRWalletException;
```

#### 通过keystore添加账号

```java
    /**
     * 通过keystore添加账号
     * @param nickname 昵称
     * @param keystore keystore
     * @param keystorePassword keystore密码
     * @param password 密码
     * @return 返回账户
     * @throws MBRWalletException
     */
    MBRAccount addAccountWithKeystore(String nickname, String keystore, String keystorePassword, String password)
            throws MBRWalletException;
```

#### 重命名账户

```java
    /**
     * 重命名账户
     * @param id 账户id
     * @param nickname 昵称
     * @return 返回重命名后的账户
     */
    MBRAccount renameAccount(String id, String nickname) throws MBRWalletException;
```

#### 删除账户

```java
    /**
     * 删除账户
     * @param id 账户id
     * @param password 密码
     * 删除账户
     */
    void deleteAccount(String id,String password) throws MBRWalletException;
```

#### 备份账户

```java
    /**
     * 备份账户
     * @param id 账户id
     * @return 备份账户
     * @throws MBRWalletException
     */
    MBRAccount backupAccount(String id, String mnemonic, String password) throws MBRWalletException;
```

#### 导出keystore

```java
    /**
     * 导出keystore 账户
     * @param id 账户id
     * @param keyStorePassword keystore 密码
     * @param password 钱包密码
     * @return 返回keystore
     * @throws MBRWalletException
     */
    String exportAccountKeystore(String id,String keyStorePassword,String password) throws MBRWalletException;
```

#### 获取助记词

```java
    /**
     * 获取助记词
     * @param id 账户id
     * @param password 密码
     * @throws MBRWalletException
     */
    String getAccountMnemonic(String id,String password) throws MBRWalletException;
```

#### 是否已存在此账户

```java

    /**
     * 是否已存在此账户
     * @param id 账户id
     * @return   存在：true;不存在：false
     * @throws MBRWalletException
     */
    boolean isAccountExisted(String id);
```

#### 账户名是否已存在

```java
    /**
     * 账户名是否已存在
     * @param nickname 昵称
     * @return 存在：true;不存在：false
     * @throws MBRWalletException
     */
    boolean isAccountNameExisted(String nickname);

```

#### 是否为默认账户

```java
    /**
     * 是否为默认账户
     * @param id 账户id
     * @return 默认账户：true;反之：false
     */
    boolean isDefaultAccount(String id);

```

#### 设置默认账户

```java
    /**
     * 设置默认账户
     * @param id 账户id
     * @throws MBRWalletException
     */
    void setDefaultAccount(String id) throws MBRWalletException;

```

#### 获取默认账户

```java
    /**
     * 获取默认账户
     * @return 返回默认账户
     * @throws MBRWalletException
     */
    MBRAccount getDefaultAccount() throws MBRWalletException;

```

#### 根据id获取账户

```java
    /**
     * 根据id获取账户
     * @param id 账户id
     * @return 账户
     * @throws MBRWalletException
     */
    MBRAccount getAccount(String id) throws MBRWalletException;

```

#### 根据账户地址获取账户

```java
    /**
     * 根据账户地址获取账户
     * @param address 账户地址
     * @return 账户
     */
    MBRAccount getAccountByAddress(String address) throws MBRWalletException;

```

#### 根据账户地址获取账户名

```java
    /**
     * 根据账户地址获取账户名
     * @param address 账户地址
     * @return 账户
     * @throws MBRWalletException
     */
    String getAccountNameByAddress(String address) throws MBRWalletException;

```


#### 获取所有账户

```java
    /**
     * 获取所有账户
     * @return 所有账户
     */
    List<MBRAccount> getAllAccount() throws MBRWalletException;

```

#### 获取所有账户地址

```java
    /**
     * 获取所有账户地址
     * @return all addresses
     */
    List<String> getAllAccountAddress() throws MBRWalletException;

```

#### 获取所有账户名

```java
    /**
     * 获取所有账户名
     * @return names of all accounts
     */
    List<String> getAllAccountName() throws MBRWalletException;

```

#### 同步所有账户的余额

```java
    /**
     * 同步所有账户的余额
     */
    void syncAllAccountBalance() throws MBRWalletException;

```

#### 同步账户余额

```java
    /**
     * 同步账户余额
     * @param accountId 账户id
     */
    void syncAccountBalance(String accountId) throws MBRWalletException;

```

#### 更新账户的币列表选中状态

```java
    /**
     * 更新账户的币列表选中状态
     * @param accountId 账户id
     * @param coins 币列表
     * @throws MBRWalletException
     */
    void updateAccountCoinsSelectState(String accountId,List<MBRBgCoin> coins) throws MBRWalletException;

```

### 交易接口

#### 检查支付时余额是否足够

```java
    /**
     * 检查支付时的余额
     * @param accountId 账户id
     * @param coinId 交易币id
     * @param amount 金额
     * @param gasPrice 矿工费
     * @throws MBRWalletException
     */
    void checkInsufficientFund(String accountId, String coinId, BigDecimal amount, BigDecimal gasPrice)
            throws MBRWalletException;
```

#### 密码支付

```java
    /**
     * 密码支付
     * @param param 支付参数
     * @param password 钱包密码
     * @return 支付结果
     * @throws MBRWalletException
     */
    MBRPayResponse payWithPwd(MBRPayParam param, String password) throws MBRWalletException;
```

#### 密码转账

```java
    /**
     * 密码转账
     * @param param 转账参数
     * @param password 钱包密码
     * @return 转账结果
     * @throws MBRDaoException
     */
    MBRTxResponse transferWithPwd(MBRTransferParam param, String password) throws MBRWalletException;
```

## 错误码
使用钱包接口错误时，就会抛出MBRWalletException，使用示例：
```java
    /**
     * 密码转账
     * @param param 转账参数
     * @param password 钱包密码
     * @throws MBRDaoException
     */
     try{
     	transferWithPwd(param,"123456");
     }catch(MBRWalletException e){
     	MBRWalletError error = e.getError();
        if(PAY_BALANCE_NOT_ENOUGH_ERROR.equals(error)){//余额不足
          //处理错误
        }
     }
```

错误                          |含义               | 备注
----------------------------- |--------------------|-------------------
CONFIG_ERROR                  |  配置错误             | 钱包配置错误，抛出此异常
PASSWORD_NO_ERROR             |  没有密码           | 无密码，需先设置密码
PASSWORD_EMPTY_ERROR          |  密码为空             |
PASSWORD_LONG_ERROR           |  密码太长             |密码长度：8~50
PASSWORD_SHORT_ERROR          |  密码太短             |密码长度：8~50
PASSWORD_FORMAT_ERROR         |  密码格式错误         |
PASSWORD_ERROR                |  密码错误            |
PASSWORD_SET_FAIL             |  设置密码失败           |
PASSWORD_SET_FAIL_FOR_ALREADY_SET |  密码已经设置过   |重新设置密码，可调用修改密码接口
PASSWORD_MODIFY_FAIL              | 密码修改失败   |
PASSWORD_REMINDER_EMPTY_ERROR     |密码提示为空    |
PASSWORD_REMINDER_LONG_ERROR | 密码提示太长   | 不能超过100
PASSWORD_REMINDER_EQUAL_PWD_ERROR |输入密码和密码提示一样    |密码和密码提示不能一样
PASSWORD_MODIFY_OLD_PWD_ERROR |  输入的旧密码错误  |密码修改接口
PASSWORD_MODIFY_NEWPWD_SAME_TO_OLDPWD_ERROR | 旧密码和新密码一样   |密码修改就扣
FINGER_UNAVAILABLE_ERROR 	|  指纹不可用  |
FINGERPRINT_ERROR 			|  指纹错误  |
COIN_UPDATE_ERROR 			|  币更新错误  |
ACCOUNT_NAME_EMPTY_ERROR 	| 账户名为空   |
ACCOUNT_NAME_DUPLICATE_ERROR| 账户名重复   |
ACCOUNT_FULL_ERROR 			|  账户名额已满  |名额上限：100
ACCOUNT_DUPLICATE_ERROR 	|  账户重复  |如：重复添加同一个keystore账户时，会抛出此异常
ACCOUNT_CHAIN_TYPE_ERROR 	| 链类型错误   |添加账户接口：此版本忽略
ACCOUNT_CREATE_FAIL_ERROR 	|  账户创建错误  |
ACCOUNT_NOT_EXIST_ERROR 	| 账户不存在   |
ACCOUNT_RENAME_ERROR 		|  账户重命名失败  |
ACCOUNT_DELETE_ERROR 		| 账户删除失败   |
ACCOUNT_BACKUP_ERROR 		|  账户备份失败  |
ACCOUNT_EXPORT_KEYSTORE_ERROR |  导出KeyStore失败  |
ACCOUNT_SET_DEFAULT_ERROR 	|  设置默认账户失败  |
ACCOUNT_COINS_UPDATE_ERROR 	|  账户列表更新失败  |
MNEMONIC_EMPTY_ERROR 		|  助记词为空  |
MNEMONIC_ERROR 				|   助记词错误 |
KEYSTORE_EMPTY_ERROR 		|  keystore为空  |
KEYSTORE_ERROR 				|  keystore错误  |
PAY_FINGER_EMPTY_ERROR 		| 指纹支付失败   |
PAY_FINGER_CHANGED_ERROR 	|  指纹已改变  |
PAY_FEE_INPUT_RANGE_ERROR 	|  矿工费输入范围错误  | [矿工费范围参照接口](#获取Gas信息)
PAY_FEE_NOT_ENOUGH_ERROR 	| 矿工费不足  | 账户的余额不足以支付矿工费
PAY_BALANCE_NOT_ENOUGH_ERROR |  余额不足 |账户的余额不足