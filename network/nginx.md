# nginx

前后端分离的开发模式下，需要使用 nginx 运行静态资源，然后将

配置：

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
		
		server{
			listen 80;
			server_name NAME;
		}
		
		server{
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

