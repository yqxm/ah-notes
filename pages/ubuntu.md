- bash 全局大小写敏感
	- ```console
	  echo 'set completion-ignore-case On' | sudo tee -a /etc/inputrc
	  ```
- `Pending update of snap-store`
	- ```bash
	  sudo snap refresh
	  # 如果弹出提示框 运行 sudo snap refresh <application-name> 获得pid
	  # 应用名称在上个命令中可以看到
	  # kill pid
	  sudo snap refresh
	  ```