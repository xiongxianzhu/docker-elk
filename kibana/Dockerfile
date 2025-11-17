ARG ELASTIC_VERSION

# https://www.docker.elastic.co/
FROM docker.elastic.co/kibana/kibana:${ELASTIC_VERSION}

USER root

# 复制新的软件源文件到镜像中
# COPY sources.list.d/ubuntu2004/sources.list /etc/apt/sources.list

# 更新软件源并安装所需软件包
RUN apt-get update && \
    apt-get install -y vim lsof telnet tzdata && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 时区设置为 Asia/Shanghai，即北京时间(GMT+8)
RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone

USER kibana
