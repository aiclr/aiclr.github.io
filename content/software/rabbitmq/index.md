+++
title = 'RabbitMQ'
date = '2024-05-28'
author = 'aiclr'
categories = ['software']
tags = ['mq','rabbitmq','docker','podman']
Summary='RabbitMQ is a messaging broker, an intermediary for messaging. It gives your applications a common platform to send and receive messages, and your messages a safe place to live until received.'
+++

[official site](https://www.rabbitmq.com/) \
[Get Started](https://www.rabbitmq.com/tutorials) \
[Docs](https://www.rabbitmq.com/docs) \
[Downloading and Installing RabbitMQ](https://www.rabbitmq.com/download.html) \
[podman](../podman/#rabbitmq)

## docker

Management Plugin rabbitmq default username and password of ***guest/guest***
```sh
docker run -d -p 5672:5672 -p 15672:15672 --name containerName imageID
docker run -d -p 5672:5672 -p 15672:15672 --name containerName imageName:version
docker run -d --hostname my-rabbit --name some-rabbit -e RABBITMQ_DEFAULT_USER=user -e RABBITMQ_DEFAULT_PASS=password rabbitmq:3.11-management-alpine
```

go to container exit container will not stop
```sh
docker exec -it containerID /bin/bash
# alpine linux
docker exec -it containerID /bin/sh
```

go to container exit container will stop
```sh
docker attach containerID
```

exit container
- ***exit*** 退出容器伪终端并关闭容器
- ***ctrl+d*** 退出容器伪终端并关闭容器
- ***ctrl+c*** 退出容器伪终端不关闭容器
- ***ctrl+p+ctrl+q*** 退出容器伪终端不关闭容器

## rabbitmqadmin

exchange
```sh
rabbitmqadmin -u username -p password declare exchange name=exchange type=topic
```

queue
```sh
rabbitmqadmin -u username -p password declare queue name=queue  durable=true
```

binding
```sh
rabbitmqadmin -u username -p password declare binding source=exchange destination=queue routing_key=key
```