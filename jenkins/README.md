# Jenkins stacks

## jenkins-rexray-df-proxy.yml

This stack sets up Jenkins master.

### Requirements

* REX-Ray driver is configured
* Network called `proxy` is created
* Docker Flow Proxy is running

```bash
# Configure REX-Ray (out of scope of this README)

docker network create --driver overlay proxy

docker stack deploy -c ../proxy/docker-flow-proxy.yml proxy
```

### Deployment

```bash
docker stack deploy -c jenkins-rexray-df.yml jenkins
```