# InfluxDB

InfluxDB is a time series database built from the ground up to handle
high write and query loads. InfluxDB is meant to be used as a backing
store for any use case involving large amounts of timestamped data,
including DevOps monitoring, application metrics, IoT sensor data, and
real-time analytics.

[InfluxDB Documentation](https://docs.influxdata.com/influxdb/latest/)

## Using this Image

### Running the container

    docker run -p 8083:8083 -p 8086:8086 influxdb

### Exposed Ports

The following ports are important and will be automatically exposed when
using `docker run -P`.

-	8083 Admin interface port
-	8086 HTTP API PORT

Other important ports that aren't exposed by default:

- 8091 Meta service port
- 8088 Clustering (raft) port

These two ports do not need to be exposed in a single server
configuration.

Find more about API Endpoints & Ports [here](https://docs.influxdata.com/influxdb/latest/concepts/api/).

### Configuration

InfluxDB can be either configured from a config file or using
environment variables. To mount a configuration file and use it with the
server, you can use this command:

    # Generate the default configuration file
    docker run --rm -v $PWD:/etc/influxdb influxdb bash -c 'influxd config > /etc/influxdb/influxdb.conf'
    # Modify the configuration file and start the daemon
    docker run -P -v $PWD:/etc/influxdb influxdb -config /etc/influxdb/influxdb.conf

Modify `$PWD` to the directory where you want to store the configuration
file.

For environment variables, the format is `INFLUXDB_$SECTION_$NAME`. All
dashes (`-`) are replaced with underscores (`_`). If the variable isn't
in a section, then omit that part.

Examples:

    INFLUXDB_REPORTING_DISABLED=true
    INFLUXDB_META_DIR=/path/to/metadir
    INFLUXDB_DATA_QUERY_LOG_ENABLED=false

Find more about configuring InfluxDB [here](https://docs.influxdata.com/influxdb/latest/introduction/installation/)

### Graphite

InfluxDB supports the Graphite line protocol, but the service and ports
are not exposed by default. To run InfluxDB with Graphite support
enabled, you can either use a configuration file

### HTTP API

Creating a DB named mydb:

	curl -G http://localhost:8086/query --data-urlencode "q=CREATE DATABASE mydb"

Inserting into the DB:

	curl -i -XPOST 'http://localhost:8086/write?db=mydb' --data-binary 'cpu_load_short,host=server01,region=us-west value=0.64 1434055562000000000'

Read more about this in the [official documentation](https://docs.influxdata.com/influxdb/latest/guides/writing_data/)

### CLI / SHELL

Start the container:

    docker run --name=influxdb -d -p 8083:8083 -p 8086:8086 influxdb

Run the influx client in another container as such:

    docker run --rm --link=influxdb -it influxdb bash -l
    > influx -host $INFLUXDB_PORT_8086_TCP_ADDR

The client needs to run bash first and then use the `influx` binary from
there so the environment variable becomes available to use.

### Web Administrator Interface

Navigate to [localhost:8083](http://localhost:8083) with your browser
while running the container.

See more about using the web admin [here](https://docs.influxdata.com/influxdb/latest/tools/web_admin/).

## Supported Docker versions

This image is officially supported on Docker version 1.10.1.
