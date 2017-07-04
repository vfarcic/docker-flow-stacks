# Web site stacks

## wordpress.yml

This stack sets up WordPres with mySQL.

### Requirements

None

### Deployment

```bash
docker stack deploy -c wordpress.yml wordpress

open "http://localhost:8080"
```

### Customization

The environment variables that can be used to customize the deployment are as follows.

|Name             |Description                   |Default   |
|-----------------|------------------------------|----------|
|DB_NAME          |mySQL database name           |wordpress |
|DB_PASSWORD      |mySQL database password       |wordpress |
|DB_ROOT_PASSWORD |mySQL database root password  |wordpress |
|DB_USER          |mySQL database user           |wordpress |
|HTTP_PORT        |The port service is running on|8080      |

## wordpress-df-proxy.yml

This stack sets up WordPres with mySQL. It uses [Docker Flow Proxy](http://proxy.dockerflow.com/) for forwarding.

### Requirements

* Network called `proxy` is created
* Docker Flow Proxy is running

```bash
docker network create --driver overlay proxy

docker stack deploy -c ../proxy/docker-flow-proxy.yml proxy
```

### Deployment

```bash
docker stack deploy -c wordpress-df-proxy.yml wordpress

open "http://localhost"
```

### Customization

The environment variables that can be used to customize the deployment are as follows.

|Name             |Description                  |Default   |
|-----------------|-----------------------------|----------|
|DB_NAME          |mySQL database name          |wordpress |
|DB_PASSWORD      |mySQL database password      |wordpress |
|DB_ROOT_PASSWORD |mySQL database root password |wordpress |
|DB_USER          |mySQL database user          |wordpress |

## docker-visualizer.yml

This stack sets up [Docker Swarm Visualizer](https://hub.docker.com/r/dockersamples/visualizer/).

### Requirements

None

### Deployment

```bash
docker stack deploy -c docker-visualizer.yml visualizer

open "http://localhost:8080"
```

### Customization

The environment variables that can be used to customize the deployment are as follows.

|Name             |Description                  |Default   |
|-----------------|-----------------------------|----------|
|HTTP_PORT        |Exposed port                 |8080      |


## docker-visualizer-df-proxy.yml

This stack sets up [Docker Swarm Visualizer](https://hub.docker.com/r/dockersamples/visualizer/). It uses [Docker Flow Proxy](http://proxy.dockerflow.com/) for forwarding.

### Requirements

* Network called `proxy` is created
* Docker Flow Proxy is running

```bash
docker network create --driver overlay proxy

docker stack deploy -c ../proxy/docker-flow-proxy.yml proxy
```

### Deployment

```bash
docker stack deploy -c docker-visualizer-df-proxy.yml visualizer

open "http://localhost/visualizer/"
```
