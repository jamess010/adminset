# adminset
本项目是adminset的Docker实现，adminset项目地址是https://github.com/guohongze/adminset

### 1）docker-compose up -d mysql
安装mysql  
### 2）docker-compose up -d adminset
安装adminset，需要改以下内容：
* adminset下，Dockerfile中的MYSQL_HOST为宿主机地址
* adminset->git->adminset->adminset.conf中，mysql, mongodb, redis的连接地址为宿主机地址
### 3）docker-compose up -d  
启动所有的其它应用
### 4）浏览器中输入：http://宿主机_ip
user=admin, password=Adminset123
### 5) 安装客户端软件
将adminset->git->adminset->install-client中内容拷贝到需要监控的机器中，运行sudo ./install.sh
