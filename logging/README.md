# Logging stacks

## logging-df-proxy.yml

This stack sets up the ELK stack (ElasticSearch, LogStash, and Kibana) and LogSpout.

### Requirements

* Network called `proxy` is created
* Docker Flow Proxy is running

```bash
docker network create --driver overlay proxy

docker stack deploy -c ../proxy/docker-flow-proxy.yml proxy
```

### Deployment

```bash
docker stack deploy -c logging-df-proxy.yml logging


```