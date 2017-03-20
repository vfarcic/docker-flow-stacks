# Docker Stacks

## portainer.yml

This stack deploys Portainer.

### Requirements

None

### Deployment

```bash
docker stack deploy -c portainer.yml portainer
```

## portainer-df-proxy.yml

This stack deploys Portainer integrated with *Docker Flow Proxy*.

### Requirements

* Network called `proxy` is created
* Docker Flow Proxy is running and has the environment variable `CONNECTION_MODE=http-keep-alive`

```bash
# Configure REX-Ray (out of scope of this README)

docker network create --driver overlay proxy

CONNECTION_MODE=http-keep-alive \
    docker stack deploy \
    -c ../proxy/docker-flow-proxy.yml \
    proxy
```

### Deployment

Portainer requires root path. Unless it is the only front-facing service in the cluster, it should run in a separate domain. Please replace `192.168.1.38.xip.io` with your (sub)domain.

```bash
SERVICE_DOMAIN=192.168.1.38.xip.io

docker stack deploy \
    -c portainer-df-proxy.yml \
    portainer

open "http://$SERVICE_DOMAIN"
```

## registry-rexray.yml

This stack sets up Docker private registry with *REX-Ray* for network storage.

### Requirements

* REX-Ray driver is configured

```bash
# Configure REX-Ray (out of scope of this README)
```

### Deployment

```bash
docker stack deploy -c registry-rexray.yml registry
```
