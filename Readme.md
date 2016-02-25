docker run -i -p 8083:8083 -p 8086:8086  <image_name>:<tag>
docker run -i -p 8083:8083 -p 8086:8086 -v /home/synerzip/docker/influxDB_temp:/root/   <image_name>:<tag>   -config /root/temp.conf


curl -i -XPOST 'http://localhost:8086/write?db=mydb' --data-binary 'cpu_load_short,host=server01,region=us-west value=0.64 1434055562000000000'

