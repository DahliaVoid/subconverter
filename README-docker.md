# subconverter Docker 使用说明

本项目提供 subconverter 的本地 Docker 构建方式。项目根目录的 Dockerfile 会从源码编译 subconverter，并把仓库内的 `base/` 目录作为运行配置一起打包进镜像。

## 构建镜像

在项目根目录执行：

```bash
docker build -t subconverter:local .
```

可选构建参数：

```bash
docker build --build-arg THREADS=2 --build-arg SHA=commit-id -t subconverter:local .
```

- `THREADS`：编译并行线程数，默认 4
- `SHA`：可选，追加到版本号后的提交标识

## 运行容器

```bash
docker run -d --restart=always -p 25500:25500 subconverter:local
```

检查运行状态：

```bash
curl http://localhost:25500/version
```

看到类似 `subconverter v0.x.x backend` 的输出即表示服务正常。

## 使用 docker-compose

```yaml
version: '3'
services:
  subconverter:
    image: subconverter:local
    container_name: subconverter
    ports:
      - "25500:25500"
    restart: always
```

## 修改配置

镜像内程序的工作目录为 `/base`，运行配置（pref、规则、模板等）来自项目根目录的 `base/`。修改配置后重新构建并重建容器：

```bash
docker build -t subconverter:local .
docker stop subconverter && docker rm subconverter
docker run -d --restart=always -p 25500:25500 subconverter:local
```

使用 compose 时：

```bash
docker compose up -d --force-recreate
```

也可以在线更新 pref 配置：

```bash
curl -F "data=@newpref.ini" "http://localhost:25500/updateconf?type=form&token=password"
```

其中 token 需要在配置文件中自行设置。
