# rpi-influxdb [![Build Status](https://travis-ci.org/hypriot/rpi-influxdb.svg?branch=master)](https://travis-ci.org/hypriot/rpi-influxdb) [![This image on DockerHub](https://img.shields.io/docker/pulls/hypriot/rpi-influxdb.svg)](https://hub.docker.com/r/hypriot/rpi-influxdb/)

Raspberry Pi compatible Docker base image with InfluxDB, an open source database written in Go specifically to handle time series data with high availability and high performance requirements.

### Build Details
- [Source Project Page](https://github.com/hypriot)
- [Source Repository](https://github.com/hypriot/rpi-influxdb)
- [Dockerfile](https://github.com/hypriot/rpi-influxdb/blob/master/Dockerfile)
- [DockerHub](https://registry.hub.docker.com/u/hypriot/rpi-influxdb/)

#### Build the Docker Image
```bash
make build
```

#### Run the Docker Image and get the version of installed InfluxDB client
```bash
make version
```

#### Push the Docker Image to the Docker Hub
* First use a `docker login` with username, password and email address
* Second push the Docker Image to the official Docker Hub

```bash
make push
```

Running your InfluxDB image
---------------------------

Start your image binding the external port `8086` of your containers:

    docker run -d -p 8086:8086 hypriot/rpi-influxdb

Docker containers are easy to delete. If you are serious about keeping InfluxDB data persistently, then consider adding a volume mapping to the containers `/data` folder:

    docker run -d --volume=/var/influxdb:/data -p 8086:8086 hypriot/rpi-influxdb

Configuring your InfluxDB
-------------------------

You can use the RESTful API to talk to InfluxDB on port `8086`. Use the new `influx` cli tool to configure the database. While the container is running, you launch the tool with the following command:

  ```
  docker exec -it <influxdb-container-name> /usr/bin/influx
  Visit https://enterprise.influxdata.com to register for updates, InfluxDB server management, and monitoring.
  Connected to http://localhost:8086 version 1.2.2
  InfluxDB shell 1.2.2
  >
  ```

Initially create Database
-------------------------
Use `-e PRE_CREATE_DB="db1;db2;db3"` to create database named "db1", "db2", and "db3" on the first time the container starts automatically. Each database name is separated by `;`. For example:

```docker run -d -p 8086:8086 -e ADMIN_USER="root" -e INFLUXDB_INIT_PWD="somepassword" -e PRE_CREATE_DB="db1;db2;db3" hypriot/rpi-influxdb:latest```

Alternatively, create a database and user with the InfluxDB 1.2 shell:

```
  > CREATE DATABASE db1
  > SHOW DATABASES
  name: databases
  ---------------
  name
  db1
  > USE db1
  > CREATE USER root WITH PASSWORD 'somepassword' WITH ALL PRIVILEGES
  > GRANT ALL PRIVILEGES ON db1 TO root
  > SHOW USERS
  user  admin
  root  true
```

Credits
-------
[This docker image was based upon the tutum-image](https://github.com/tutumcloud/influxdb)


## License

The MIT License (MIT)

Copyright (c) 2017 Hypriot

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
