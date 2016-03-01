FROM ubuntu:14.04

ENV INFLUXDB_VERSION 0.10

EXPOSE 8083
EXPOSE 8086
EXPOSE 8088
EXPOSE 8091

RUN apt-get update && \
    apt-get install -y wget

RUN wget https://s3.amazonaws.com/influxdb/influxdb_0.10.1-1_amd64.deb && \
    sudo dpkg -i influxdb_0.10.1-1_amd64.deb && \
    rm /influxdb_0.10.1-1_amd64.deb

COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["influxd"]
