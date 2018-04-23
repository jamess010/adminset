# adminset
本项目是adminset的Docker实现，adminset项目地址是https://github.com/guohongze/adminset <br>
1）docker-compose up -d mysql<br>
安装mysql<br>
2）docker-compose up -d adminset<br>
安装adminset，需要改adminset下，Dockerfile中的MYSQL_HOST为宿主机地址<br>
3）docker-compose up -d<br>
