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

## docker-flow-proxy-secrets.yml

This stack deploys Docker Flow Proxy and Docker Flow Swarm Listener. Statistics page will be protected through secrets.

### Requirements

None

### Deployment

```
docker network create --driver overlay proxy

echo "secret-user" \
    | docker secret create dfp_stats_user -

echo "secret-pass" \
    | docker secret create dfp_stats_pass -

docker stack deploy -c docker-flow-proxy-secrets.yml proxy
```
