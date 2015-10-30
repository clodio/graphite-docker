## Graphite + Carbon

An all-in-one image running graphite and carbon-cache. **Version**: 0.9.12.

This image contains a sensible default configuration of graphite and
carbon-cache. Starting this container will, by default, bind the the following
host ports:

- `80`: the graphite web interface
- `2003`: the carbon-cache line receiver (the standard graphite protocol)
- `2004`: the carbon-cache pickle receiver
- `7002`: the carbon-cache query port (used by the web interface)

With this image, you can get up and running with graphite by simply running:

    docker run -d clodio/graphite-docker

If you already have services running on the host on one or more of these ports,
you may wish to allow docker to assign random ports on the host. You can do this
easily by running:

    docker run -p 80 -p 2003 -p 2004 -p 7002 -d clodio/graphite-docker

You can log into the administrative interface of graphite-web (a Django
application) with the username `admin` and password `admin`. These passwords can
be changed through the web interface.

**NB**: Please be aware that by default docker will make the exposed ports
accessible from anywhere if the host firewall is unconfigured.

### Data volumes

Graphite data and logs are stored whithin the conainer at `/var/lib/graphite/storage` 
and `/var/log`. If you wish to store your persistent data outside the container (highly
recommended) you can use docker's data volumes feature. For example, to store
data on the host, you could use:

    docker run -v /data/graphite/storage:/var/lib/graphite/storage \
               -v /data/graphite/log:/var/log \
               -d clodio/graphite-docker

### Technical details

#### Metrics

By default, this instance of carbon-cache uses the following retention period.

    1m:31d

You can use alternatives retentions periods by prefixing your metrics 
(Ex 1min.prod.my_app.my_metric.users will store metrics with precision of 1 minute for 31 days)

    1sec.* : 1s:1d

    1min.* 1m:31d

    10m.* 10m:31d

    1day.* 1d:365d

If you use statsd, the folowing retention rules are applied :

	stats.* : 10s:1d,1m:31d,10m:31d,1d:365d

#### Logs
	By default, useless logs and carbon metrics are disabled.

For more information, see [the repository](https://github.com/clodio/graphite-docker).
