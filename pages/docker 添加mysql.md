- ```console
  docker pull mysql
  
  docker run -itd --name mysql-docker -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
  ```