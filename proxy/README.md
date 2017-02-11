# Proxy stacks

## docker-flow-proxy.yml

This stack deploys Docker Flow Proxy and Docker Flow Swarm Listener.

### Requirements

None

### Deployment

```
docker network create --driver overlay proxy

docker stack deploy -c docker-flow-proxy.yml proxy
```

## docker-flow-proxy-admin.yml

This stack deploys Docker Flow Proxy and Docker Flow Swarm Listener. Proxy port *8080* used for administrative tasks will be opened.

### Requirements

None

### Deployment

```
docker network create --driver overlay proxy

docker stack deploy -c docker-flow-proxy-admin.yml proxy
```
