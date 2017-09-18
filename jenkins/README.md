# Jenkins stacks

## jenkins.yml

This stack sets up a Jenkins master.

### Requirements

None

### Deployment

```bash
docker stack deploy -c jenkins.yml jenkins

open "http://localhost:8080/jenkins"

docker service logs jenkins_main # Copy the password and paste it in the UI

# Follow the setup instructions in UI
```

## jenkins-df-proxy.yml

This stack sets up a Jenkins master integrated with *Docker Flow Proxy*.

### Requirements

* Network called `proxy` is created
* Docker Flow Proxy is running

```bash
docker network create --driver overlay proxy

docker stack deploy -c ../proxy/docker-flow-proxy.yml proxy
```

### Deployment

```bash
docker stack deploy -c jenkins-df-proxy.yml jenkins

open "http://localhost/jenkins"

docker service logs jenkins_main # Copy the password and paste it in the UI

# Follow the setup instructions in UI
```

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

open "http://localhost/jenkins"

docker service logs jenkins_main # Copy the password and paste it in the UI
```

## jenkins-swarm-agent.yml

This stack deploys Jenkins agents to all the nodes of a Swarm cluster.

### Requirements

* `/workspace` directory needs to exist on all nodes of a cluster
* Jenkins master service running
* *Self-Organizing Swarm Plug-in Modules* plugin is installed in Jenkins master

```bash
sudo mkdir /workspace

sudo chmod 777 /workspace

# Deploy Jenkins master using one of the stacks (e.g. jenkins-rexray-df-proxy.yml)

# Install *Self-Organizing Swarm Plug-in Modules* plugin
```

### Deployment

```bash
    # Replace MASTER_USER and MASTER_PASS with "real" values
    docker stack deploy -c jenkins-swarm-agent.yml jenkins-agent -username MASTER_USER -password MASTER_PASS
```

## jenkins-swarm-agent-secrets.yml

This stack deploys Jenkins agents to all the nodes of a Swarm cluster.

### Requirements

* `/workspace` directory needs to exist on all nodes of a cluster
* Jenkins master service running
* *Self-Organizing Swarm Plug-in Modules* plugin is installed in Jenkins master
* Secrets with Jenkins master username and password are created

```bash
sudo mkdir /workspace

sudo chmod 777 /workspace

# Deploy Jenkins master using one of the stacks (e.g. jenkins-rexray-df-proxy.yml)

# Install *Self-Organizing Swarm Plug-in Modules* plugin

echo "admin" | docker secret create jenkins-user -

echo "admin" | docker secret create jenkins-pass -
```

### Deployment

```bash
export JENKINS_IP=[...]

# Replace MASTER_USER and MASTER_PASS with "real" values
JENKINS_USER_SECRET=jenkins-user \
    JENKINS_PASS_SECRET=jenkins-pass \
    docker stack deploy -c jenkins-swarm-agent-secrets.yml jenkins-agent
```
