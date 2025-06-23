# 镜像操作
  - 查看所有镜像
  ```bash
    docker image ls
  ```
  - 删除镜像
  ```bash
    docker rmi <image_name>:<image_tag>
  ```
  - 导出镜像
    docker save -o <file_name>.tar <image_name>:<image_tag>
  ```
  - 导入镜像
  ```bash
    docker load -i <file_name>.tar
  ```
  - 镜像重命名
  ```bash
    docker tag <old_name>:<old_tag> <new_name>:<new_tag>
  ```
  - 镜像推送到远程仓库
  ```bash
    docker push <image_name>:<image_tag>
  ```

# 容器操作
  - 运行容器
  ```bash
    docker run -d -p <host_port>:<container_port> --name <container_name> <image_name>:<image_tag>
    docker run -d -p <host_port>:<container_port> -v ${PWD}\html:/usr/share/nginx/html --name <container_name> <image_name>:<image_tag> # 挂载目录
    docker run -d -p <host_port>:<container_port> -v ngconf:/etc/nginx --name <container_name> <image_name>:<image_tag> # 挂载卷
    docker run -d -p <host_port>:<container_port> -e <env_name>=<env_value> --name <container_name> <image_name>:<image_tag> # 设置环境变量
  ```
  - 停止容器
  ```bash
    docker stop <container_name>
  ```
  - 启动容器
  ```bash
    docker start <container_name>
  ```
  - 重启容器
  ```bash
    docker restart <container_name>
  ```
  - 进入容器
  ```bash
    docker exec -it <container_name> /bin/bash
  ```
  - 删除容器
  ```bash
    docker rm [-f] <container_name>
    docker rm -f $(docker ps -aq) # 删除所有容器
  ```
  - 查看容器日志
  ```bash
    docker logs <container_name>
  ```
  - 查看容器信息
  ```bash
    docker inspect <container_name>
  ```
  - 提交
  ```bash
    docker commit [-m <commit_message>] <container_name> <image_name>:<image_tag>
  ```

# 卷操作
  - 创建卷
  ```bash
    docker volume create <volume_name>
  ```
  - 查看卷
  ```bash
    docker volume ls
  ```
  - 查看卷信息
  ```base
    docker volume inspect <volume_name>
  ```
  - 删除卷
  ```bash
    docker volume rm <volume_name>
  ```
  - 挂载卷
  ```bash
    docker run -d -p <host_port>:<container_port> -v <volume_name>:<container_path> --name <container_name> <image_name>:<image_tag>
  ```

# 网络操作
  - 创建网络
  ```bash
    docker network create <network_name>
  ```
  - 查看网络
  ```bash
    docker network ls
  ```
  - 删除网络
  ```bash
    docker network rm <network_name>
  ```
  - 连接到网络
  ```bash
    docker run -d -p <host_port>:<container_port> --name <container_name> --network <network_name> <image_name>:<image_tag>
  ```

# compose操作
  - 启动compose
  ```bash
    docker compose up -d
    docker compose start [x1]
  ```
  - 停止compose
  ```bash
    docker compose down [-v] # -v 在移除容器和网络的同时，一并移除具名卷。
    docker compose stop [x1]
  ```

# dockerfile操作
  - 构建镜像
  ```bash
    docker build -f Dockerfile -t <image_name>:<image_tag> . # . 表示当前目录
  ```