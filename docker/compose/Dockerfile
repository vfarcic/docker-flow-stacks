FROM docker

RUN apk add --update 'py-pip' && pip install "docker-compose"

WORKDIR /compose

CMD ["docker-compose", "version"]