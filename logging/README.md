# Logging stacks

## logging-df-proxy.yml

This stack sets up the ELK stack (ElasticSearch, LogStash, and Kibana) and LogSpout.

### Requirements

* Network called `proxy` is created
* Docker Flow Proxy is running
* `sysctl -w vm.max_map_count=262144`

For ElasticSearch to work properly, `sudo sysctl -w vm.max_map_count=262144` must be run on the server. It's best to put this in the packer.json file

```bash
sudo sysctl -w vm.max_map_count=262144

docker network create --driver overlay proxy

docker stack deploy -c ../proxy/docker-flow-proxy.yml proxy
```

### Deployment

```bash
docker stack deploy -c logging-df-proxy.yml logging

open "http://localhost/app/kibana"
```