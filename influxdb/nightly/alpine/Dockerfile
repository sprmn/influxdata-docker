FROM arm32v6/alpine

RUN apk add --no-cache bash

RUN set -ex && \
    apk add --no-cache --virtual .build-deps wget gnupg tar ca-certificates && \
    update-ca-certificates && \
    for key in \
        05CE15085FC09D18E99EFB22684A14CF2582E0C5 ; \
    do \
        gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key" || \
        gpg --keyserver pgp.mit.edu --recv-keys "$key" || \
        gpg --keyserver keyserver.pgp.com --recv-keys "$key" ; \
    done && \
    wget --no-verbose https://dl.influxdata.com/influxdb/nightlies/influxdb-nightly_linux_armel.tar.gz.asc && \
    wget --no-verbose https://dl.influxdata.com/influxdb/nightlies/influxdb-nightly_linux_armel.tar.gz && \
    gpg --batch --verify influxdb-nightly_linux_armel.tar.gz.asc influxdb-nightly_linux_armel.tar.gz && \
    mkdir -p /usr/src && \
    tar -C /usr/src -xzf influxdb-nightly_linux_armel.tar.gz && \
    chmod +x /usr/src/influxdb-*/usr/bin/* && \
    cp -a /usr/src/influxdb-*/usr/bin/* /usr/bin/ && \
    rm -rf *.tar.gz* /usr/src /root/.gnupg && \
    apk del .build-deps
COPY influxdb.conf /etc/influxdb/influxdb.conf

EXPOSE 8083 8086

VOLUME /var/lib/influxdb

COPY entrypoint.sh /entrypoint.sh
COPY init-influxdb.sh /init-influxdb.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["influxd"]
