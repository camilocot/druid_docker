FROM camilocot/base_jdk:v1

RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

# MySQL (Metadata store)
RUN echo 'mysql-server mysql-server/root_password password toor' | debconf-set-selections
RUN echo 'mysql-server mysql-server/root_password_again password toor' | debconf-set-selections
RUN apt-get install -y mysql-server

RUN sed -i -e"s/^bind-address\s*=\s*127.0.0.1/bind-address = 0.0.0.0/" /etc/mysql/my.cnf

RUN /etc/init.d/mysql start \
      && mysql -u root --password=toor -e "GRANT ALL ON druid.* TO 'druid'@'%' IDENTIFIED BY 'diurd'; CREATE database druid CHARACTER SET utf8;" \
      && sleep 2s \
      && /etc/init.d/mysql stop

EXPOSE 3306

# Setup supervisord
ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf
ENTRYPOINT exec /usr/bin/supervisord -c /etc/supervisor/conf.d/supervisord.conf

