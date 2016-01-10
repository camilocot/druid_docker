FROM camilocot/base_jdk:v1

RUN wget -q -O - http://apache.mirrors.pair.com/zookeeper/zookeeper-3.4.7/zookeeper-3.4.7.tar.gz | tar -xzf - -C /opt \
    && mv /opt/zookeeper-3.4.7 /opt/zookeeper \
    && cp /opt/zookeeper/conf/zoo_sample.cfg /opt/zookeeper/conf/zoo.cfg \
    && mkdir -p /tmp/zookeeper \
    && chown :daemon /tmp/zookeeper

RUN chmod 770 /tmp/zookeeper

ENV JAVA_HOME /usr/lib/jvm/java-8-oracle

EXPOSE 2181 2888 3888

VOLUME ["/opt/zookeeper/conf", "/tmp/zookeeper"]

WORKDIR /tmp

# Setup supervisord
ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf
ENTRYPOINT exec /usr/bin/supervisord -c /etc/supervisor/conf.d/supervisord.conf
