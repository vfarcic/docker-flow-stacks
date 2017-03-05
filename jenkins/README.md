# Jenkins stacks

## jenkins-rexray-df-proxy.yml

This stack sets up Jenkins master with *REX-Ray* for network storage and *Docker Flow Proxy*.

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
docker stack deploy -c jenkins-rexray-df-proxy.yml jenkins

open "localhost/jenkins"
```

## jenkins-swarm-agent.yml

This stack deploys Jenkins agents to all the nodes of a Swarm cluster.

### Requirements

* `/workspace` directory needs to exist on all nodes of a cluster
* Jenkins master service running

```bash
sudo mkdir /workspace

sudo chmod 777 /workspace

# Deploy Jenkins master using one of the stacks (e.g. jenkins-rexray-df-proxy.yml)
```

### Deployment

```bash
export JENKINS_IP=[...]

docker stack deploy -c jenkins-swarm-agent.yml jenkins-agent
```

## jenkins-swarm-agent-rexray.yml

This stack deploys Jenkins agents to all the nodes of a Swarm cluster. REX-Ray is used for persisting agent workspaces.

### Requirements

* REX-Ray driver is configured

```bash
# Configure REX-Ray (out of scope of this README)
```

* Jenkins master service running

```bash
# Deploy Jenkins master using one of the stacks (e.g. jenkins-rexray-df-proxy.yml)
```

### Deployment

```bash
export JENKINS_IP=[...]

docker stack deploy -c jenkins-swarm-agent-rexray.yml jenkins-agent
```