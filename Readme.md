## InfluxDB 

InfluxDB is a time series database built from the ground up to handle high write and query loads. InfluxDB is meant to be used as a backing store for any use case involving large amounts of timestamped data, including DevOps monitoring, application metrics, IoT sensor data, and real-time analytics.

[InfluxDB Documentation] (https://docs.influxdata.com/influxdb/v0.10/)

## Using this image

#### Exposed Ports 

- 8083  Admin interface port
- 8086  HTTP API PORT
- 8088 & 8091  Internal Communication ports

Find more about API Endpoints & Ports [here] (https://docs.influxdata.com/influxdb/v0.10/concepts/api/).

#### Using one of the published ports 

Use the --publish or the -p option of the docker run. Find more about it in the official docker [docs] (https://docs.docker.com/engine/reference/commandline/run/)

```
docker run -i -p 8083:8083 -p 8086:8086  influxdb 
```
#### Supplying a config file available on the host machine

```
docker run -i -p 8083:8083 -p 8086:8086 -v /some/host/path:/path/on/container   influxdb   -config /path/on/container/temp.conf
```
Find more about configuring InfluxDB [here] (https://docs.influxdata.com/influxdb/v0.10/introduction/installation/)

#### Example usage of the image created the way described above

##### Creating a DB named myDB
```
curl -G http://localhost:8086/query --data-urlencode "q=CREATE DATABASE mydb"
```
##### Inserting in the DB 
```
curl -i -XPOST 'http://localhost:8086/write?db=mydb' --data-binary 'cpu_load_short,host=server01,region=us-west value=0.64 1434055562000000000'
```
[Documentation for writing data to influxDB] (https://docs.influxdata.com/influxdb/v0.10/guides/writing_data/)

##### Clustering 
###### Example of creating a pseudo-cluster on the same host using 3 containers

- Create a docker network intially 
```
docker network create --subnet=172.18.0.0/16 influxdbnetwork
```
- Create three config files as described [here] (https://docs.influxdata.com/influxdb/v0.10/clustering/cluster_setup/)
  
  Assume that the three container have ip addresses as:-
    - 172.18.0.11  (config file host1.conf)
    - 172.18.0.12  (config file host2.conf)
    - 172.18.0.13  (config file host3.conf)

```
[meta]
  enabled = true
  ...
  bind-address = "<IP>:8088"
  http-bind-address = "<IP>:8091"

...

[data]
  enabled = true

[http]
  ...
  bind-address = "<IP>:8086"
```
- Run the containers as follows:- 
```
docker run -i -p 8083:8083 \
              -p 8086:8086 \
              -p 8088:8088 \
              -p 8091:8091 \
              --net influxdbnetwork  --ip 172.18.0.11 \
              -v /path/on/host:/root/ \
              influxdb   -config /root/host1.conf
              

docker run -i -p 10083:8083 \
              -p 10086:8086 \
              -p 10088:8088 \
              -p 10091:8091 \
              --net influxdbnetwork  --ip 172.18.0.12 \
              -v /path/on/host:/root/  \
              influxdb -config /root/host2.conf -join 172.18.0.11:8091,172.18.0.12:8091

docker run -i -p 11083:8083 \
              -p 11086:8086 \
              -p 11088:8088 \
              -p 11091:8091 \
              --net influxdbnetwork  --ip 172.18.0.13 \
              -v /path/on/host:/root/ \
              influxdb   -config /root/host3.conf -join 172.18.0.11:8091,172.18.0.12:8091,172.18.0.13:8091
```
```/path/on/host``` is where you store ```host1.conf```, ```host2.conf```, ```host3.conf```
