```
docker network create --driver overlay proxy
docker stack deploy -c docker-flow-proxy.yml proxy
```
