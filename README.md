# mini-fdu-gateway

2024年春季学期《高级Web技术》课程项目实践 网关

# docker 部署

## 后端docker镜像打包

> 不选择进行多阶段构建，而是本机构建 jar 包，因为多阶段构建更慢

1. 打包jar：maven-->项目名称-->生命周期-->package （如果测试无法通过，可能需要使用`@Disabled`注解禁用测试）

2. 构建docker镜像

```shell
# 本地构建
docker build -t mini-fdu-gateway:1.0.0 .
# 构建并推送到docker hub
## 如果需要推送到自己的docker hub，需要先登录，并更改 zmarkgo 为自己的用户名
docker build -t zmarkgo/mini-fdu-gateway:1.0.0 .
docker push zmarkgo/mini-fdu-gateway:1.0.0
```

## 使用docker部署后端

本地测试：

```shell
# 多行命令
docker run -d \
--add-host=host.docker.internal:host-gateway \
--name mini-fdu-gateway \
-p 8700:8700 \
-e SERVER_PORT=8700 \
-e USER_SERVICE_IP=host.docker.internal \
-e USER_SERVICE_PORT=8799 \
-e MESSAGE_SERVICE_IP=host.docker.internal \
-e MESSAGE_SERVICE_PORT=8798 \
-e CHAT_SERVICE_IP=host.docker.internal \
-e CHAT_SERVICE_PORT=8798 \
-e GAME_SERVICE_IP=host.docker.internal \
-e GAME_SERVICE_PORT=8798 \
-e VIDEO_CHAT_SERVICE_IP=host.docker.internal \
-e VIDEO_CHAT_SERVICE_PORT=8798 \
-e STUDY_SERVICE_IP=host.docker.internal \
-e STUDY_SERVICE_PORT=8797 \
-e AI_SERVICE_IP=host.docker.internal \
-e AI_SERVICE_PORT=8796 \
mini-fdu-gateway:1.0.0 

# 单行命令
docker run -d --add-host=host.docker.internal:host-gateway --name mini-fdu-gateway -p 8700:8700 -e SERVER_PORT=8700 -e USER_SERVICE_IP=host.docker.internal -e USER_SERVICE_PORT=8799 -e MESSAGE_SERVICE_IP=host.docker.internal -e MESSAGE_SERVICE_PORT=8798 -e CHAT_SERVICE_IP=host.docker.internal -e CHAT_SERVICE_PORT=8798 -e GAME_SERVICE_IP=host.docker.internal -e GAME_SERVICE_PORT=8798 -e VIDEO_CHAT_SERVICE_IP=host.docker.internal -e VIDEO_CHAT_SERVICE_PORT=8798 -e STUDY_SERVICE_IP=host.docker.internal -e STUDY_SERVICE_PORT=8797 -e AI_SERVICE_IP=host.docker.internal -e AI_SERVICE_PORT=8796 mini-fdu-gateway:1.0.0
```

### 环境变量配置

在配置文件[application-prod.yml](./src/main/resources/application-prod.yml)中使用

| 变量名                       | 说明                      | 默认值       |
|---------------------------|-------------------------|-----------|
| `SERVER_PORT`             | 服务端口                    | 8700      |
| `USER_SERVICE_IP`         | user-service IP地址       | 127.0.0.1 |
| `USER_SERVICE_PORT`       | user-service 端口         | 8799      |
| `MESSAGE_SERVICE_IP`      | message-service IP地址    | 127.0.0.1 |
| `MESSAGE_SERVICE_PORT`    | message-service 端口      | 8798      |
| `CHAT_SERVICE_IP`         | chat-service IP地址       | 127.0.0.1 |
| `CHAT_SERVICE_PORT`       | chat-service 端口         | 8798      |
| `GAME_SERVICE_IP`         | game-service IP地址       | 127.0.0.1 |
| `GAME_SERVICE_PORT`       | game-service 端口         | 8798      |
| `VIDEO_CHAT_SERVICE_IP`   | video-chat-service IP地址 |
| `VIDEO_CHAT_SERVICE_PORT` | video-chat-service 端口   | 8798      |
| `STUDY_SERVICE_IP`        | study-service IP地址      | 127.0.0.1 |
| `STUDY_SERVICE_PORT`      | study-service 端口        | 8797      |
| `AI_SERVICE_IP`           | ai-service IP地址         | 127.0.0.1 |
| `AI_SERVICE_PORT`         | ai-service 端口           | 8796      |
