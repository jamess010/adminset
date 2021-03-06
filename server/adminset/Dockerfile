FROM centos:7.3.1611 

ENV MAIN_DIR /var/opt/adminset
ENV ADMINSET_DIR $MAIN_DIR/main
ENV DATA_DIR $MAIN_DIR/data
ENV CONFIG_DIR $MAIN_DIR/config
ENV LOGS_DIR $MAIN_DIR/logs
ENV MYSQL_HOST 192.168.31.86 
ENV MYSQL_PASSWORD 123456 

RUN mkdir -p $ADMINSET_DIR\
  && mkdir -p $DATA_DIR/scripts\
  && mkdir -p $DATA_DIR/ansible/playbook\
  && mkdir -p $DATA_DIR/ansible/roles\
  && mkdir -p $CONFIG_DIR\
  && mkdir -p $CONFIG_DIR/webssh\
  && mkdir -p $LOGS_DIR\
  && mkdir -p $LOGS_DIR/pid\
  && mkdir -p $MAIN_DIR/pid\

RUN yum install -y epel-release
RUN yum install -y gcc expect python-pip python-devel ansible smartmontools dmidecode libselinux-python git rsync dos2unix

COPY ./git/ /root/
RUN /usr/bin/yum install -y nodejs
WORKDIR /root/adminset/vendor/WebSSH2
RUN /usr/bin/npm config set registry http://registry.cnpmjs.org
RUN /usr/bin/npm install --production
RUN /usr/bin/npm install forever -g

RUN rsync --delete --progress -ra --exclude '.git' /root/adminset/ $ADMINSET_DIR

# 此句是防止报错：Failed to get D-Bus connection: Operation not permitted
#CMD ['/usr/sbin/init']

COPY ./git/adminset/install/server/ansible/ansible.cfg /etc/ansible/ansible.cfg

COPY ./git/adminset/install/server/webssh/webssh.service /usr/lib/systemd/system/webssh.service

RUN systemctl enable webssh.service

RUN pip install -U pip
RUN pip install kombu==4.1.0
RUN pip install celery==4.0.2
RUN pip install billiard==3.5.0.3
RUN pip install pytz==2017.2
RUN pip install kombu==4.1.0

WORKDIR $ADMINSET_DIR/vendor/django-celery-results-master/
RUN python setup.py build
RUN python setup.py install

# 安装mysql客户端
RUN yum install -y mariadb.x86_64 mariadb-libs.x86_64
RUN yum install -y mariadb-devel 
#RUN yum install -y mysql-client
#RUN pip install mysqlclient
RUN mysql -h$MYSQL_HOST -uroot -p$MYSQL_PASSWORD -e "CREATE DATABASE if not exists adminset DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;"


WORKDIR $ADMINSET_DIR
RUN pip install -r requirements.txt
RUN python manage.py makemigrations
RUN python manage.py migrate

RUN mysql -h$MYSQL_HOST -uroot -p$MYSQL_PASSWORD -e "insert into adminset.accounts_userinfo (password,username,email,is_active,is_superuser) values ('pbkdf2_sha256\$24000\$2odRjOCV1G1V\$SGJCqWf0Eqej6bjjxusAojWtZkz99vEJlDbQHUlavT4=','admin','admin@126.com',1,1);"

COPY ./git/adminset/install/server/adminset.service /usr/lib/systemd/system
#RUN systemctl daemon-reload
#RUN systemctl start adminset.service
#RUN chkconfig adminset on
#RUN service adminset start

# celery
RUN mkdir -p $CONFIG_DIR/celery
COPY ./git/adminset/install/server/celery/beat.conf $CONFIG_DIR/celery/beat.conf
COPY ./git/adminset/install/server/celery/celery.service /usr/lib/systemd/system
COPY ./git/adminset/install/server/celery/start_celery.sh $CONFIG_DIR/celery/start_celery.sh
COPY ./git/adminset/install/server/celery/beat.service /usr/lib/systemd/system
RUN chmod +x $CONFIG_DIR/celery/start_celery.sh
#CMD ['/usr/sbin/init']
#RUN systemctl daemon-reload
#RUN chkconfig celery on
##RUN chkconfig beat on
#RUN service celery start
#RUN service beat start
WORKDIR $ADMINSET_DIR
CMD ["gunicorn","-c","gunicorn_conf.py","adminset.wsgi"]
# ssh not install
