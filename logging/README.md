```
cd images/logstash
docker build -t logstash .
docker tag logstash localhost:5000/logstash:1.0
docker push localhost:5000/logstash:1.0
```

```
cd ../../
docker stack deploy -c logging.yml logging
```