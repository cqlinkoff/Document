## Pay业务移动端详细设计

### 移动端整体架构

<img src="https://raw.githubusercontent.com/cqmbr/MBRDocument/master/docs/MBRPayApp/Pay%E7%BB%84%E4%BB%B6%E7%BB%93%E6%9E%84%E5%9B%BE.png" width="800"/>

### 库设计

-  **MBRWalletNetworking**
支付业务网络库，封装了所有服务端API，给上层提供网络数据。

<img src="https://raw.githubusercontent.com/cqmbr/MBRDocument/master/docs/MBRPayApp/MBRWalletNetworking%E7%B1%BB%E5%9B%BE.png" width="1200"/>

-  **ethers**
ETH三方库。

-  **MBRWallet**
钱包SDK，提供钱包相关所有功能，供内部各种Pay APP和需要内置钱包业务的三方APP使用

<img src="https://raw.githubusercontent.com/cqmbr/MBRDocument/master/docs/MBRPayApp/MBRWallet%E7%B1%BB%E5%9B%BE.jpeg" width="800"/>

-  **MBRPayCore**
支付业务核心库，提供所有业务逻辑相关功能，UnitPay、COSPay等App调用MBRPayCore，复用同一套业务逻辑代码，方便维护。不同的Pay APP只在UI层做区分。
下图为MBRPayCore分层结构和编码规范：

<img src="https://raw.githubusercontent.com/cqmbr/MBRDocument/master/docs/MBRPayApp/Pay%20App%E7%BC%96%E7%A0%81%E8%A7%84%E8%8C%83.png" width="1200"/>

