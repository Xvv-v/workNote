# nginx

前后端分离的开发模式下，需要使用 nginx 运行静态资源，然后将

常用命令：

* 

配置结构

* **全局块：**
* **events 块：**主要配置 nginx 服务器或用户的网络连接、进程最大数、事件驱动模型等诸如此类。常用配置项如下：
  * use epoll；使用 epoll 网络模型
  * worker_connections: 1024；配置连接数
* **http 块：**主要配置超时时间、缓存等请求相关
  * **http 全局块：**
    * 
  * **upstream 块：**
  * **server 块：**

形如下：

```
{
	# 全局
	worker_processes auto;
	
	events{
	
	}
	
	http{
		include mime.type;
		
		upstream BACKEND {
			server IP:PORT;
		}
		
		server {
			listen 80;
			server_name NAME;
		}
		
		server {
			listen 81;
			server_name NAME;
			location / {
				root html;
				index index.html;
			}
		}
		include servers/*;
	}

}
```

