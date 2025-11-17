# docker-elk

搭建ELK日志分析平台(Elastic Stack):

```plain
docker + elasticsearch + logstash + kibana + filebeat
```

## ELK docker

当前ELK最新版本是8.10.4，推荐选择最新版本安装。

ELK8.10.4的镜像基于`ubuntu:20.04`。

ELK的docker镜像版本：

```plain
docker.elastic.co/kibana/kibana:8.10.4
docker.elastic.co/logstash/logstash:8.10.4
docker.elastic.co/elasticsearch/elasticsearch:8.10.4
```

filebeat版本也是8.10.4:

```plain
docker.elastic.co/beats/filebeat:8.10.4
```

filebeat既可以用docker镜像安装也可以直接下载filebeat二进制文件安装:

<https://www.elastic.co/guide/en/beats/filebeat/8.10/filebeat-installation-configuration.html>

<https://www.elastic.co/cn/downloads/beats/>

## 端口

- 5044: Logstash Beats端口
- 50000: Logstash TCP端口
- 9600: Logstash monitoring API端口
- 9200: Elasticsearch HTTP端口
- 9300: Elasticsearch TCP传输端口
- 5601: Kibana端口

## 用法

先创建角色和用户，设置密码，再创建容器：

```sh
docker-compose up setup
docker-compose up -d
```

其他命令：

```sh
docker-compose up
docker-compose up -d
docker-compose up elasticsearch
docker-compose up setup
docker-compose up elasticsearch logstash kibana
docker-compose up -d logstash kibana
docker-compose down -v
docker-compose build
docker-compose build elasticsearch
docker-compose build elasticsearch logstash kibana
docker-compose rm -f <container_id_or_name>
docker rm -f xx-setup

docker exec -it xx-elasticsearch /bin/sh
docker exec -it xx-kibana /bin/sh
docker exec -it xx-logstash /bin/sh

docker-compose restart elasticsearch
docker-compose restart kibana
docker-compose restart logstash

# 查看日志
docker-compose logs -f
```

## 访问

<http://127.0.0.1:5601>
<http://127.0.0.1:9200>

## ELK容器相关信息

查看软件源:

```sh
cat /etc/apt/sources.list
````

能看出容器默认的软件源是ubuntu20.04的

- [检查健康状态]<http://localhost:9200/_cluster/health>
- [检查指标]<http://localhost:9200/_cat/indices?v>

## docker-compose.yml说明

在Docker Compose文件中的`volumes`部分，`ro,Z`是关于挂载的选项的参数设置。

- `ro`表示将挂载的文件系统设为"只读"（read-only）。这意味着容器中的应用程序只能读取挂载的文件，无法对其进行写入或修改操作。

- `Z`是与SELinux相关的参数。它用于设置挂载的文件或目录的SELinux安全上下文。当在SELinux启用的环境中运行时，使用`Z`参数可以确保挂载的文件或目录具有适当的安全上下文，以便Docker容器可以正确访问它们。

综合起来，`ro,Z`表示将`./elasticsearch/config/elasticsearch.yml`文件以只读方式挂载到容器的`/usr/share/elasticsearch/config/elasticsearch.yml`路径，并确保挂载的文件具有适当的SELinux安全上下文。

请注意，这些选项是可选的，具体使用取决于你的需求和环境设置。

## 参考

- <https://github.com/deviantony/docker-elk>

## filebeat的安装

```bash
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.10.4-darwin-aarch64.tar.gz
tar xzvf filebeat-8.10.4-darwin-aarch64.tar.gz
cd filebeat-8.10.4-darwin-aarch64
```
