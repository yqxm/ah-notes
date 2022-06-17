- ```bash
  # 拉取镜像
  sudo docker run -it pkuflyingpig/pintos bash
  
  # 回到主机clone
  git clone https://github.com/PKU-OS/pintos
  
  # mount
  docker run -it --rm --name pintos --mount type=bind,source=/home/huhu/Desktop/learn/pku-os/pintos,target=/home/PKUOS/pintos pkuflyingpig/pintos bash
  ```
- `Ctrl + D`退出docker ubuntu
- `Ctrl + C`退出Pintos