############################################################
# Dockerfile to run an OrientDB (Graph) Container
############################################################

FROM rhel7.2:latest

MAINTAINER OrientDB LTD (info@orientdb.com)

# Override the orientdb download location with e.g.:
#   docker build -t mine --build-arg ORIENTDB_DOWNLOAD_SERVER=http://repo1.maven.org/maven2/com/orientechnologies/ .
ARG ORIENTDB_DOWNLOAD_SERVER

ENV ORIENTDB_VERSION 2.2.14
ENV ORIENTDB_DOWNLOAD_MD5 599e89bc8a0cd1452b583027515aeecd
ENV ORIENTDB_DOWNLOAD_SHA1 cd3bf0012bcc9b5ee311b71ae711af20bec0b11a

ENV ORIENTDB_DOWNLOAD_URL ${ORIENTDB_DOWNLOAD_SERVER:-http://central.maven.org/maven2/com/orientechnologies}/orientdb-community/$ORIENTDB_VERSION/orientdb-community-$ORIENTDB_VERSION.tar.gz

#Saitisfy the Docker yum requirements
RUN yum repolist --disablerepo=* && \
    yum-config-manager --disable \* > /dev/null && \
    yum-config-manager --enable rhel-7-server-rpms > /dev/null

#Install JDK and necessary applications
RUN yum -y install java-1.8.0-openjdk && \
    yum -y install wget && \
    yum -y install tar && \
    yum clean all

#download distribution tar, untar and delete databases
RUN mkdir /orientdb 
RUN wget  $ORIENTDB_DOWNLOAD_URL && \
  echo "$ORIENTDB_DOWNLOAD_MD5 *orientdb-community-$ORIENTDB_VERSION.tar.gz" | md5sum -c - && \
  echo "$ORIENTDB_DOWNLOAD_SHA1 *orientdb-community-$ORIENTDB_VERSION.tar.gz" | sha1sum -c -&& \
  tar -xvzf orientdb-community-$ORIENTDB_VERSION.tar.gz -C /orientdb --strip-components=1 && \
  rm orientdb-community-$ORIENTDB_VERSION.tar.gz && \
  rm -rf /orientdb/databases/*


ADD help.1 /help.1

ENV PATH /orientdb/bin:$PATH

VOLUME ["/orientdb/backup", "/orientdb/databases", "/orientdb/config"]

WORKDIR /orientdb

#OrientDb binary
EXPOSE 2424

#OrientDb http
EXPOSE 2480

# Default command start the server
CMD ["server.sh"]
