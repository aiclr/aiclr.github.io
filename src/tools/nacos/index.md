Nacos is an easy-to-use platform designed for dynamic service discovery and configuration and service management. It helps you to build cloud native applications and microservices platform easily.

- [Nacos 文档](https://nacos.io/zh-cn/docs/quick-start.html)
- [Nacos 官网](https://nacos.io/)
- [Nacos 源码](https://github.com/alibaba/nacos)
- [Nacos-docker-github](https://github.com/nacos-group/nacos-docker)
- [spring-cloud-alibaba](https://github.com/alibaba/spring-cloud-alibaba)

```sh
# docker
docker run --name nacos -e MODE=standalone -p 8848:8848 -p 9848:9848 -d nacos/nacos-server:latest

# podman
podman run --name nacos -e MODE=standalone -p 8848:8848 -p 9848:9848 -d docker.io/nacos/nacos-server:latest
```

**Nacos** 控制台 [http://127.0.0.1:8848/nacos/](http://127.0.0.1:8848/nacos/) default username and password **_nacos/nacos_**
