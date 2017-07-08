# Metrics stacks

## docker-flow-monitor.yml

This stack deploys *Docker Flow Monitor*.

### Requirements

None

### Deployment

```bash
docker stack deploy -c docker-flow-monitor.yml monitor

open "http://localhost:9090"
```

## docker-flow-monitor-proxy.yml

This stack deploys *Docker Flow Monitor* and *Docker Flow Swarm Listener*.

### Requirements

* Network called `proxy` is created
* *Docker Flow Proxy* is running

```bash
docker network create --driver overlay proxy

docker stack deploy -c ../proxy/docker-flow-proxy.yml proxy
```

### Deployment

```bash
docker network create --driver overlay monitor

docker stack deploy -c docker-flow-monitor-proxy.yml monitor

open "http://localhost/monitor"
```

## exporters.yml

This stack deploys Prometheus *HA PRoxy Exporter*.

### Requirements

* Network called `proxy` is created
* Network called `monitor` is created
* *Docker Flow Proxy* is running
* *Docker Flow Monitor* is running

```bash
docker network create --driver overlay proxy

docker stack deploy -c ../proxy/docker-flow-proxy.yml proxy

docker network create --driver overlay monitor

docker stack deploy -c docker-flow-monitor-proxy.yml monitor
```

### Deployment

```bash
docker stack deploy -c exporters.yml exporter

open "http://localhost/monitor"
```

## prometheus.yml

This stack deploys Prometheus

### Requirements

None

### Deployment

```bash
docker stack deploy -c prometheus.yml metrics

open "http://localhost:9090"
```

## prometheus-grafana.yml

This stack deploys node-exporter, cAdvisor, Prometheus, and Grafana

### Requirements

* logging-df-proxy.yml stack

```bash
docker stack deploy -c ../logging/logging.yml logging
```

### Deployment

```bash
docker stack deploy -c prometheus-grafana.yml metrics
```

Use `http://metrics_prometheus:9090` as Prometheus data source and `http://logging_elasticsearch:9200` for ElasticSearch.

## prometheus-grafana-df-proxy.yml

This stack sets up the node-exporter, cAdvisor, Prometheus, and Grafana

### Requirements

* docker-flow-proxy.yml stack
* logging-df-proxy.yml stack

```bash
docker network create --driver overlay proxy

docker stack deploy -c ../proxy/docker-flow-proxy.yml proxy

docker stack deploy -c ../logging/logging-df-proxy.yml logging
```

### Deployment

```bash
export USER=admin

export PASS=secret

docker stack deploy -c prometheus-grafana-df-proxy.yml metrics

open "http://localhost/grafana"
```

Use `http://metrics_prometheus:9090` as Prometheus data source and `http://logging_elasticsearch:9200` for ElasticSearch.





```bash
docker stack deploy -c zabbix.yml zabbix

open "http://localhost:8081/"
```