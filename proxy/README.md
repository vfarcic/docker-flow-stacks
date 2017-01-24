```
docker network create --driver overlay proxy
docker stack deploy -c proxy.yml proxy
```