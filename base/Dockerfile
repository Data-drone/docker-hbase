FROM openjdk:8
MAINTAINER Ivan Ermilov <ivan.s.ermilov@gmail.com>

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends net-tools curl netcat

ENV HBASE_VERSION 2.0.0
ENV PHOENIX_VERSION 5.0.0

ENV HBASE_URL http://archive.apache.org/dist/hbase/$HBASE_VERSION/hbase-$HBASE_VERSION-bin.tar.gz
RUN set -x \
    && curl -fSL "$HBASE_URL" -o /tmp/hbase.tar.gz \
    && curl -fSL "$HBASE_URL.asc" -o /tmp/hbase.tar.gz.asc \
    && tar -xvf /tmp/hbase.tar.gz -C /opt/ \
    && rm /tmp/hbase.tar.gz*

ENV PHOENIX_URL http://www.apache.org/dist/phoenix/apache-phoenix-${PHOENIX_VERSION}-HBase-2.0/bin/apache-phoenix-${PHOENIX_VERSION}-HBase-2.0-bin.tar.gz
RUN set -x \
    && curl -fSL "$PHOENIX_URL" -o /tmp/phoenix.tar.gz \
    && curl -fSL "$PHOENIX_URL.asc" -o /tmp/phoenix.tar.gz.asx \
    && tar -xvf /tmp/phoenix.tar.gz -C /opt/ \
    && rm /tmp/phoenix.tar.gz*  

RUN cp /opt/apache-phoenix-${PHOENIX_VERSION}-HBase-2.0-bin/phoenix-${PHOENIX_VERSION}-HBase-2.0-server.jar /opt/hbase-$HBASE_VERSION/lib/phoenix-${PHOENIX_VERSION}-HBase-2.0-server.jar

RUN ln -s /opt/hbase-$HBASE_VERSION/conf /etc/hbase
RUN mkdir /opt/hbase-$HBASE_VERSION/logs

RUN mkdir /hadoop-data

ENV HBASE_PREFIX=/opt/hbase-$HBASE_VERSION
ENV HBASE_CONF_DIR=/etc/hbase

ENV USER=root
ENV PATH $HBASE_PREFIX/bin/:$PATH

ADD entrypoint.sh /entrypoint.sh
RUN chmod a+x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
