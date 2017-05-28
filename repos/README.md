# Nexus Stacks

## nexus.yml

This stack deploys Sonatype Nexus.

### Requirements

None

### Deployment

```bash
docker stack deploy -c nexus.yml nexus
```

### Testing

Wait until Nexus is running. The status can be checked with the command `docker stack ps nexus`.

```bash
curl -u admin:admin123 \
    http://localhost:8081/service/metrics/ping
```

## nexus-df-proxy.yml

This stack deploys Sonatype Nexus integrated with *Docker Flow Proxy*.

### Requirements

* Network called `proxy` is created
* *Docker Flow Proxy* is running

```bash
docker network create --driver overlay proxy

docker stack deploy \
    -c ../proxy/docker-flow-proxy.yml \
    proxy
```

### Deployment

```bash
DOMAIN=localhost \
    docker stack deploy \
    -c nexus-df-proxy.yml \
    nexus
```

### Testing

Wait until Nexus service and the process are running. The status of the service can be checked with the command `docker stack ps nexus`. To confirm whether the nexus process is running, please execute `docker service logs nexus_main`.

```bash
curl -u admin:admin123 \
    http://localhost/service/metrics/ping
```