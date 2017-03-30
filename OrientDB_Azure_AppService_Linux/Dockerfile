############################################################
# Dockerfile to run an OrientDB (Graph) Container in Azure App service for Linux (ASL)
############################################################

FROM openjdk:8-jdk-alpine

MAINTAINER Harald Fianbakken (harald.fianbakken@gmail.com)

ARG ORIENTDB_DOWNLOAD_SERVER
ENV ORIENTDB_VERSION 2.2.17
ENV ORIENTDB_DOWNLOAD_MD5 3be5c561fbee52ca6ba12f1637f206fc
ENV ORIENTDB_DOWNLOAD_SHA1 e43104e7dafb301a232212d2300922a086f7aedf
ENV ORIENTDB_DOWNLOAD_URL ${ORIENTDB_DOWNLOAD_SERVER:-http://central.maven.org/maven2/com/orientechnologies}/orientdb-community/$ORIENTDB_VERSION/orientdb-community-$ORIENTDB_VERSION.tar.gz

RUN apk add --update tar \
    && rm -rf /var/cache/apk/*

#download distribution tar, untar - Does _not_ delete databases if you have them in the location mapped
RUN mkdir /home/orientdb && \
  wget  $ORIENTDB_DOWNLOAD_URL \
  && echo "$ORIENTDB_DOWNLOAD_MD5 *orientdb-community-$ORIENTDB_VERSION.tar.gz" | md5sum -c - \
  && echo "$ORIENTDB_DOWNLOAD_SHA1 *orientdb-community-$ORIENTDB_VERSION.tar.gz" | sha1sum -c - \
  && tar -xvzf orientdb-community-$ORIENTDB_VERSION.tar.gz -C /home/orientdb --strip-components=1 \
  && rm orientdb-community-$ORIENTDB_VERSION.tar.gz 

ENV PATH /home/orientdb/bin:$PATH

VOLUME ["/home/orientdb/backup", "/home/orientdb/databases", "/home/orientdb/config"]

WORKDIR /home/orientdb

#OrientDb binary
EXPOSE 2424

#OrientDb http
EXPOSE 2480

# Default command start the server
CMD ["server.sh"]