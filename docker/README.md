# Docker Stacks

## portainer.yml

This stack deploys Portainer.

### Requirements

None

### Deployment

```bash
docker stack deploy -c portainer.yml portainer
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
