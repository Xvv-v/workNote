# HTTPS 全解

HTTPS 协议即为 HTTP + TLS/SSL

<img src="/Users/xujingwen/Code/workNote/asset/HTTPS-HTTP.webp" alt="https和http区别" style="zoom: 50%;" />

HTTPS 解决数据传输安全问题的方案就是加密算法，具体来说就是混合加密算法，也就是对称加密和非对称加密的混合使用

* 对称加密：即加密和解密都使用一个密钥，常见的对称加密密钥有 DES、3DES 和 AES 等

* 非对称加密：即加密解密使用的是不同的密钥：公钥和私钥。公钥用于加密，私钥用于解密

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

数据传输的过程就是对称加密的，使用客户端生成的 KEY 传输的数据进行加密

## 如何生成证书

生成证书有以下几种方式：

* 首先生成私钥，再通过私钥生成自签名的 cert 证书文件
* 首先生成私钥，再生成 csr 文件，通过 csr 文件申请证书

证书类型：

* 自签名私有证书：独立使用，没有建立信任链，主要用于小范围、短期的加密和身份验证。
* 自签名 CA 证书：作为根CA，能够签发其他受信任的证书，通过预安装的方式建立信任链，适用于需要管理多个证书的环境。

生成证书的基本流程：

1. 生成自己的私钥文件（.key）
2. 基于私钥生成证书的签名请求文件（.csr）
3. 将证书请求文件提交给 CA 机构，CA 会将提交的证书请求里所有的信息生成一个摘要，然后使用 CA 根证书对应的私钥进行加密，这就是所谓的 “签名” 操作，完成签名后就会得到真正的签发证书（.cer 或 .crt）

生成证书主要有以下几种方式

openssl

certbot

cloudflare

let's encrypt

### openssl

1. 生成私钥 key

   ```shell
   openssl genrsa -aes128 -out pricate.key 2048
   ```

2. 使用私钥生成证书请求文件

   ```shell
   openssl req -new -key pricate.key -out cert.csr
   ```

有了私钥和证书请求文件只有，有三种选择：

* 标准 CA 签发流程

  这种方式是使用 CA 机构的私钥去签名

  ```shell
  openssl x509 -req -days 365 -in cert.csr -CA ca.crt -CAkey ca.key -set_serial 01 -out server.crt
  ```

* 生成自签名证书

  自签名证书是使用自己的私钥（开头生成的那个私钥）进行签名，自签名的证书不会被浏览器信任

  ```shell
  openssl x509 -req -days 365 -in server.csr -signkey private.key -out cert.crt
  ```

* 私有 CA 签发证书

  即自签生成的证书作为 CA 证书，去签署其他的证书

其他命令：

* 使用私钥生成对应的公钥：

  ```shell
  openssl rsa -in private.key -pubout -out public.pub
  ```

* 从证书中提取公钥：

  ```shell
  openssl x509 -pubkey -noout -in cert.crt > public_key.pem
  ```

### Let's encrypt

Let's Encrypt 是一个证书颁发机构

## Go 语言搭建 HTTPS 服务





## 其他问题





参考文章：

* [[HTTPS 详解一：附带最精美详尽的 HTTPS 原理图]](https://segmentfault.com/a/1190000021494676)
* [HTTPS详解二：SSL / TLS 工作原理和详细握手过程](https://segmentfault.com/a/1190000021559557)
* [openssl 生成证书步骤](https://blog.csdn.net/hinewcc/article/details/137826940?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EYuanLiJiHua%7EPosition-3-137826940-blog-123617558.235%5Ev43%5Econtrol&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EYuanLiJiHua%7EPosition-3-137826940-blog-123617558.235%5Ev43%5Econtrol&utm_relevant_index=6)
* [pem、crt 和 key 文件的区别](https://blog.csdn.net/qq_33745102/article/details/123280477)
* [openssl 生成证书步骤](https://blog.csdn.net/hinewcc/article/details/137826940?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EYuanLiJiHua%7EPosition-3-137826940-blog-123617558.235%5Ev43%5Econtrol&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EYuanLiJiHua%7EPosition-3-137826940-blog-123617558.235%5Ev43%5Econtrol&utm_relevant_index=6)
