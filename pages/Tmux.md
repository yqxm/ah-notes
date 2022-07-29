- 创建配置文件
	- ```console
	  cd ~
	  
	  vim .tmux.conf
	  ```
- 常用配置
	- ```conf
	  # 在当前目录创建窗口
	  bind-key c new-window -c "#{pane_current_path}"
	  bind-key % split-window -h -c "#{pane_current_path}"
	  bind-key '"' split-window -c "#{pane_current_path}"
	  
	  # 设置256色
	  set -g default-terminal "screen-256color"
	  
	  # 为neovim配置
	  set-option -sg escape-time 10
	  set-option -g focus-events on
	  set-option -ga terminal-overrides ",xterm-256color:Tc"
	  
	  ```
- **翻滚**: `Ctrl-b + [`
-