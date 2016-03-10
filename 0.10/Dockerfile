FROM debian:jessie

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    ca-certificates apt-transport-https

RUN apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys 684A14CF2582E0C5 && \
    echo "deb https://repos.influxdata.com/debian jessie stable" > /etc/apt/sources.list.d/influxdata.list && \
    apt-get update 

RUN apt-get install influxdb

EXPOSE 8083 8086 8088 8091

COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["influxd"]
