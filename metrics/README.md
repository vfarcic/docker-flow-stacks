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

## grafana-df-proxy.yml

This stack sets up Grafana with Docker Flow Proxy. The stack assumes that Grafana will be served through a dedicated (sub)domain.

### Requirements

* docker-flow-proxy.yml stack

```bash
docker network create --driver overlay proxy

docker stack deploy -c ../proxy/docker-flow-proxy.yml proxy
```

### Deployment

```bash
# Change to your Grafana user
export USER=admin

# Change to your Grafana password
export PASS=secret

# Change to your domain
export DOMAIN=localhost

docker stack deploy -c grafana-df-proxy.yml grafana

open "http://${DOMAIN}"
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
