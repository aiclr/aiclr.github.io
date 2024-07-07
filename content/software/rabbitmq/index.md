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

## k8s

### rabbitmq-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: rabbitmq-deployment
  labels:
    app: rabbitmq
spec:
  replicas: 1
  selector:
    matchLabels:
      app: rabbitmq
  template:
    metadata:
      labels:
        app: rabbitmq
    spec:
      containers:
      - name: rabbitmq
        image: rabbitmq:3.8.26-management-alpine
        imagePullPolicy: Never # IfNotPresent Always Never
        ports:
        - name: manager
          containerPort: 15672
          protocol: TCP
        - name: http
          containerPort: 5672
          protocol: TCP
        env:
        - name: RABBITMQ_DEFAULT_USER
          value: user
        - name: RABBITMQ_DEFAULT_PASS
          value: "123456"
        volumeMounts:
          - name: mq-time
            mountPath: /etc/localtime
            readOnly: true
      volumes:
        - name: mq-time
          hostPath:
            path: /etc/localtime
```

### Create the Deployment

```shell
kubectl apply -f rabbitmq-deployment.yaml
kubectl get deployments
kubectl get deploy -o wide
kubectl get rs
kubectl get rs -w
kubectl get pods
kubectl get pods --show-labels
kubectl describe po -l app=rabbitmq
```

### Scaling a Deployment

```shell
kubectl scale deployment/rabbitmq-deployment --replicas=3
kubectl autoscale deployment/rabbitmq-deployment --min=3 --max=5 --cpu-percent=80
```

### update

```shell
kubectl set image deployment.v1.apps/rabbitmq-deployment rabbitmq=rabbitmq:3.9-management-alpine
kubectl set image deployment/rabbitmq-deployment rabbitmq=rabbitmq:3.9-management-alpine
kubectl rollout status deployment/rabbitmq-deployment
kubectl rollout history deployment/rabbitmq-deployment
kubectl rollout history deployment/rabbitmq-deployment --revision=2
```

### rollback

```shell
kubectl rollout undo deployment/rabbitmq-deployment
kubectl rollout undo deployment/rabbitmq-deployment --to-revision=2
```

### Pausing and Resuming a Deployment 

```shell
# 暂停
kubectl rollout pause deployment/rabbitmq-deployment

# 执行更新等操作
# do something 
# 执行更新等操作

# 恢复 并应用更新
kubectl rollout resume deployment/rabbitmq-deployment
```

### rabbitmq-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: rabbitmq-service
spec:
  type: NodePort
  selector:
    app: rabbitmq
  ports:
    - name: manager
      protocol: TCP
      port: 15672
      targetPort: 15672
      nodePort: 30001
    - name: http
      protocol: TCP
      port: 5672
      targetPort: 5672
      nodePort: 30002
```