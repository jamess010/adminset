# adminset
本项目是adminset的Docker实现。adminset是一个基于python的开源运维监控项目, 项目地址参看：https://github.com/guohongze/adminset

### 1）安装mysql 
在server中运行：<br>
docker-compose up -d mysql
 
### 2）安装adminset

需要改以下内容：
* server->adminset下，Dockerfile中的MYSQL_HOST为宿主机地址
* server->adminset->git->adminset->adminset.conf中，改mysql, mongodb, redis的连接地址为宿主机地址

然后在server中运行：<br>
docker-compose up -d adminset
### 3）启动所有的其它应用
在server中运行：<br>
docker-compose up -d  

### 4）浏览器中输入：http://宿主机_ip
user=admin, password=Adminset123
### 5) 在需要监控的机器中安装客户端软件
将client中内容拷贝到需要监控的机器中，运行sudo ./install.sh
