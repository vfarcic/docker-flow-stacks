# DB Stacks

## redis-df-proxy.yml

This stack sets up Redis with *Docker Flow Proxy*.

### Requirements

* Network called `proxy` is created
* Docker Flow Proxy is running

```bash
docker stack deploy -c ../proxy/docker-flow-proxy.yml proxy
```

### Deployment

```bash
docker stack deploy -c redis-df-proxy.yml redis
```
