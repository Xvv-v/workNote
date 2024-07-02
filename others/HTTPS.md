# HTTPS 全解

HTTPS 协议即为 HTTP + TLS/SSL

<img src="/Users/xujingwen/Code/workNote/asset/HTTPS-HTTP.webp" alt="https和http区别" style="zoom:67%;" />

HTTPS 解决数据传输安全问题的方案就是加密算法，具体来说就是混合加密算法，也就是对称加密和非对称加密的混合使用

对称加密

即加密和解密都使用一个密钥，常见的对称加密密钥有 DES、3DES 和 AES 等

非对称加密

即加密解密使用的是不同的密钥：公钥和私钥。公钥用于加密，私钥用于解密



## HTTPS 通信

HTTPS 的通信过程可以分为两个阶段：

* 证书验证：非对称加密
* 数据传输：对称加密

![HTTPS 加密、解密、验证及数据传输过程.png](/Users/xujingwen/Code/workNote/asset/https02.webp)

### 证书验证

1. 客户端请求到到 server 的 443 端口
2. 采用 HTTPS 的服务器要有一套数字 CA 证书，在生成证书时会同时产生一对公钥和私钥，私钥保存在客户端，公钥是附带在证书中的信息，可以公开。证书本身也携带一个电子签名，用来验证证书的完整性和真实性
3. 服务端响应客户端请求，将证书（连带公钥）发送给客户端
4. 客户端拿到证书验证证书合法性
5. 通过验证后，客户端从证书中取出公钥，并生成一个随机 KEY，使用公钥对 KEY 进行加密
6. 把加密后的 KEY 发送给服务端，作为后续数据传输的对称加密密钥，传输给服务端的数据都由这个 KEY 进行加密
7. 服务器使用私钥对加密 KEY 进行解密拿到 KEY，后续给客户端的数据都使用这个 KEY 进行解密

### 数据传输





## 证书管理工具

cloudflare

certbot

let's encrypt



参考文章：

* [[HTTPS 详解一：附带最精美详尽的 HTTPS 原理图]](https://segmentfault.com/a/1190000021494676)
* [HTTPS详解二：SSL / TLS 工作原理和详细握手过程](https://segmentfault.com/a/1190000021559557)
