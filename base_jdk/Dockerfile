FROM ubuntu:14.04

# Java 8
RUN apt-get install -y software-properties-common \
      && apt-add-repository -y ppa:webupd8team/java \
      && apt-get purge --auto-remove -y software-properties-common \
      && apt-get update \
      && echo oracle-java-8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections \
      && apt-get install -y oracle-java8-installer \
      && apt-get install -y oracle-java8-set-default \
      && apt-get install -y supervisor \
      && rm -rf /var/cache/oracle-jdk8-installer


# clean up time
RUN apt-get clean \
    && rm -rf /tmp/* \
       /var/tmp/*
