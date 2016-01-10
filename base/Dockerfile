FROM camilocot/base_jdk:v1

# Druid system user
RUN adduser --system --group --no-create-home druid \
      && mkdir -p /var/lib/druid \
      && chown druid:druid /var/lib/druid

# git
RUN apt-get install -y git

# Maven
RUN wget -q -O - http://archive.apache.org/dist/maven/maven-3/3.2.5/binaries/apache-maven-3.2.5-bin.tar.gz | tar -xzf - -C /usr/local \
      && ln -s /usr/local/apache-maven-3.2.5 /usr/local/apache-maven \
      && ln -s /usr/local/apache-maven/bin/mvn /usr/local/bin/mvn

# Druid (from source)
RUN mkdir -p /usr/local/druid/lib
RUN mkdir -p /usr/local/druid/config
# whichever github owner (user or org name) you would like to build from
ENV GITHUB_OWNER druid-io
# whichever branch you would like to build
ENV DRUID_VERSION master

# trigger rebuild only if branch changed
ADD https://api.github.com/repos/$GITHUB_OWNER/druid/git/refs/heads/$DRUID_VERSION druid-version.json
RUN git clone -q --branch $DRUID_VERSION --depth 1 https://github.com/$GITHUB_OWNER/druid.git /tmp/druid
WORKDIR /tmp/druid
# package and install Druid locally
# use versions-maven-plugin 2.1 to work around https://jira.codehaus.org/browse/MVERSIONS-285
RUN mvn -U -B org.codehaus.mojo:versions-maven-plugin:2.1:set -DgenerateBackupPoms=false -DnewVersion=$DRUID_VERSION \
  && mvn -U -B install -DskipTests=true -Dmaven.javadoc.skip=true \
  && cp services/target/druid-services-$DRUID_VERSION-selfcontained.jar /usr/local/druid/lib

RUN cp -r distribution/target/extensions /usr/local/druid/
RUN cp -r distribution/target/hadoop_dependencies /usr/local/druid/

RUN chown -R druid: /usr/local/druid /var/lib/druid

WORKDIR /var/lib/druid
