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

## registry.yml

This stack sets up Docker private registry.

### Requirements

None

### Deployment

```bash
docker stack deploy -c registry.yml registry
```

## registry-cert.yml

This stack sets up Docker private registry with certificates injected as Docker services.

### Requirements

Certificates and docker secrets.

```bash
docker secret create registry.crt certs/registry.crt

docker secret create registry.key certs/registry.key
```

Feel free to replace `certs/registry.crt` and `certs/registry.key` with paths to your certificate and the key.

### Deployment

```bash
docker stack deploy -c registry-cert.yml registry
```

## registry-df-proxy.yml

This stack sets up Docker private registry integrated with *Docker Flow Proxy*.

### Requirements

* Network called `proxy` is created
* Docker Flow Proxy is running and has the environment variable `CONNECTION_MODE=http-keep-alive`

```bash
docker-machine create -d virtualbox test

eval $(docker-machine env test)

docker swarm init \
    --advertise-addr $(docker-machine ip test)

docker network create \
    --driver overlay proxy

docker stack deploy \
    -c ../proxy/docker-flow-proxy.yml \
    proxy
```

### Deployment

```bash
DOMAIN=localhost docker stack deploy \
    -c registry-df-proxy.yml \
    registry
```

### Test

```bash
docker image pull alpine

docker image tag alpine localhost/alpine

docker image push localhost/alpine

docker image pull localhost/alpine
```

## registry-df-proxy-ssl.yml

This stack sets up Docker private registry integrated with *Docker Flow Proxy*.
It also assumes that ssl is terminated at the proxy.

### Requirements

* Network called `proxy` is created
* Docker Flow Proxy is running and has the environment variable `CONNECTION_MODE=http-keep-alive`
* SSL Terminated at proxy

```bash
docker-machine create -d virtualbox test

eval $(docker-machine env test)

docker swarm init \
    --advertise-addr $(docker-machine ip test)

docker network create --driver overlay proxy

DOMAIN=localhost docker stack deploy \
    -c ../proxy/docker-flow-proxy-ssl.yml \
    proxy
```

### Deployment

```bash
docker stack deploy -c registry-df-proxy-ssl.yml registry
```

### Test

```bash
docker image pull alpine

docker image tag alpine localhost/alpine

docker image push localhost/alpine

docker image pull localhost/alpine
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
