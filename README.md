# TELEGRAF
In this Project used like a "SNMP Browser" of Energy Devices and Power Devices

############# Creamos Directorio ################

``` 
$ mkdir -p /docker-data/telegraf/etc
```

############ Configuramos telegraf.conf ##########3

```
$ nano /docker-data/telegraf/etc/telegraf.conf
```

------------ Archivo telegraf.conf ------------

```
[global_tags]
# Configuration for telegraf agent
[agent]
    interval = "10s"
    debug = false
    hostname = "iot-gw"
    round_interval = true
    flush_interval = "10s"
    flush_jitter = "0s"
    collection_jitter = "0s"
    metric_batch_size = 1000
    metric_buffer_limit = 10000
    quiet = false
    logfile = ""
    omit_hostname = false
```

##############OUTPUTS##############

```
[[outputs.influxdb]]
    urls = ["http://influx1:8086"]
    database = "telegraf"
timeout = "0s"
    username = "telegraf_w"
    password = "password"
    retention_policy = ""    
```
    
##############INPUTS##############

```
[[inputs.cpu]]
    percpu = true
    totalcpu = true
    collect_cpu_time = false
    report_active = false
[[inputs.disk]]
    ignore_fs = ["tmpfs", "devtmpfs", "devfs"]
[[inputs.io]]
[[inputs.mem]]
[[inputs.net]]
[[inputs.system]]
[[inputs.swap]]
[[inputs.netstat]]
[[inputs.processes]]
[[inputs.kernel]]
```

######################## creamos docker-compose#############

```
nano /docker-data/telegraf/docker-compose.yml
```

---------------------- Archivo docker-compose.yml ------------

```
version: '3.3'
services:
 telegraf:
   container_name: telegraf
   restart: unless-stopped
   image: telegraf
   privileged: true
   volumes:
      - '/:/hostfs:ro'
      - '/run/udev:/run/udev:ro'
      - './etc/telegraf.conf:/etc/telegraf/telegraf.conf:ro'
      - '/etc/localtime:/etc/localtime:ro'
   environment:
      - HOST_PROC=/hostfs/proc
      - TZ=America/Santiago
   networks:
      - i40sys
networks:
 dockerlink:
   external:
     name: dockerlink
```

############### Iniciar Compose################

```
cd /docker-data/telegraf
docker-compose up -d
```
