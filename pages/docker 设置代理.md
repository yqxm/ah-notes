- docker daemon
	- 创建目录
		- sudo mkdir -p /etc/systemd/system/docker.service.d
	- 创建文件
		- ```console
		  sudo vim /etc/systemd/system/docker.service.d/http-proxy.conf
		  ```
	- 文件中添加设置
		- ```conf
		  [Service]
		  Environment="HTTP_PROXY=http://proxy.example.com:80"
		  Environment="HTTPS_PROXY=https://proxy.example.com:443"
		  Environment="NO_PROXY=localhost,127.0.0.1,docker-registry.example.com,.corp"
		  
		  # 或者
		  Environment=ALL_PROXY="socks5://127.0.0.1:10809"
		  ```
	- 重启docker
		- ```console
		   sudo systemctl daemon-reload
		   sudo systemctl restart docker
		  ```
	- 验证
		- ```console
		  sudo systemctl show --property=Environment docker
		  ```
	- 不知道为什么前几次代理没有设置成功，切换了几次`socks5`和`http`之后，重启之后又好了。
- container 代理
	- 配置`~/.docker/config.json`
		- ```json
		  {
		   "proxies":
		   {
		     "default":
		     {
		       "httpProxy": "http://proxy.example.com:8080",
		       "httpsProxy": "http://proxy.example.com:8080",
		       "noProxy": "localhost,127.0.0.1,.example.com"
		     }
		   }
		  }
		  ```
	- 也可以使用`-e`注入环境变量