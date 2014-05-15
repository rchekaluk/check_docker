check_docker
============

A Nagios check to check some basic statistics reported by the Docker
daemon. Additionally validates the absence of Ghost containers and
and be made to require the presence of a container running from a
particular image tag.

`check_docker` is written in Go and is multi-threaded to keep the
drag time low on your Nagios server. It makes two API requests to the
Docker daemon, one to `/info` and one to `/containers/json`
and processes the results, all simultaneously.

It is built using the the 
[go_nagios](http://source.datanerd.us/site-engineering/go_nagios)
framework.

Building
--------

```
go fetch source.datanerd.us/site-engineering/go_nagios
go build
```

Usage
-----
```
Usage of ./check_docker:
  -base-url="http://docker-server:4243/": The Base URL for the Docker server
  -crit-data-space=100: Critical threshold for Data Space
  -crit-meta-space=100: Critical threshold for Metadata Space
  -ghosts-status=1: If ghosts are present, treat as this status
  -image-id="": An image ID that must be running on the Docker server
  -warn-data-space=100: Warning threshold for Data Space
  -warn-meta-space=100: Warning threshold for Metadata Space
```

`-base-url`: Here you specify the base url of the docker server.

`-image-id`: You can specify an image tag that needs to be running on the server for
certain cases where you have pegged a container to a server (e.g. each server
has a Nagios monitoring container running to report on server health).

`-ghosts-status`: the Nagios exit code you want to use if ghost containers
are present on the server. The number follows standard Nagios conventions.

`-(warn|crit)-(meta|data)-space`: the thresholds at which the named Nagios status codes
should be emitted. These are percentages, so `-crit-data-space=95` would send
a CRITICAL response when the threshold of 95% is crossed.