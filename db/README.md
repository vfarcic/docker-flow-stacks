# DB Stacks

## redis-df-proxy.yml

This stack sets up Redis with *Docker Flow Proxy*.

### Requirements

* Network called `proxy` is created
* Docker Flow Proxy is running
* Port 6379 is open

```bash
docker stack deploy -c ../proxy/docker-flow-proxy.yml proxy

docker service update --publish-add 6379:6379 proxy_proxy
```

### Deployment

```bash
docker stack deploy -c redis-df-proxy.yml redis
```
