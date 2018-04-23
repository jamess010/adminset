# adminset
本项目是adminset的Docker实现，adminset项目地址是https://github.com/guohongze/adminset

## 1）docker-compose up -d mysql
安装mysql  
## 2）docker-compose up -d adminset
安装adminset，需要改以下内容
* adminset下，Dockerfile中的MYSQL_HOST为宿主机地址  
* 改adminset->git->adminset->adminset.conf中，mysql的地址为宿主机地址
  
## 3）docker-compose up -d  
  启动所有的其它应用
