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
docker network create --driver overlay proxy

CONNECTION_MODE=http-keep-alive \
    docker stack deploy \
    -c ../proxy/docker-flow-proxy.yml \
    proxy
```

### Deployment

```bash
docker stack deploy \
    -c portainer-df-proxy.yml \
    portainer

open "http://localhost/portainer/"
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

## registry-rexray-external.yml

This stack sets up Docker private registry with externally defined *REX-Ray* volume for network storage.

### Requirements

* REX-Ray driver is configured and the registry volume is created

```bash
# Configure REX-Ray (out of scope of this README)

docker volume create -d rexray registry
```

### Deployment

```bash
docker stack deploy -c registry-rexray-external.yml registry
```
