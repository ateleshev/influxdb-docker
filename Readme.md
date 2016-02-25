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

