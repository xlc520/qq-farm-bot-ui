## 使用 GitHub 镜像

本仓库通过 GitHub Actions 自动构建并推送镜像到 GitHub Container Registry。
可把 `ghcr.io/xlc520/qq-farm-bot-ui:latest` 替换到 `docker-compose.yml` 中的 image

```shell
# 国内加速
ghcr.1ms.run/xlc520/qq-farm-bot-ui:latest  
# 原地址
ghcr.io/xlc520/qq-farm-bot-ui:latest
```

### docker-compose 部署

```yaml
services:
  qq-farm-bot-ui:
    image: ghcr.1ms.run/xlc520/qq-farm-bot-ui:latest  # 国内加速
    # image: ghcr.io/xlc520/qq-farm-bot-ui:latest     # 原地址
    container_name: qq-farm-bot-ui
    restart: unless-stopped
    environment:
      ADMIN_PASSWORD: admin
      TZ: Asia/Shanghai
    ports:
      - "3000:3000"
    volumes:
      - ./data:/app/core/data

```

### docker 部署

```bash
# 拉取镜像（将 <user/repo> 替换为实际仓库地址）
docker pull ghcr.io/xlc520/qq-farm-bot-ui:latest

# 运行容器
docker run -d \
  --name qq-farm-bot \
  -p 3000:3000 \
  -v ./data:/app/core/data \
  -e ADMIN_PASSWORD=你的强密码 \
  ghcr.io/xlc520/qq-farm-bot-ui:latest

# 查看日志
docker logs -f qq-farm-bot
```

**可选参数说明**

| 参数                         | 说明                      |
|----------------------------|-------------------------|
| `-p 3000:3000`             | 端口映射，格式为 `宿主机端口:容器端口`   |
| `-v ./data:/app/core/data` | 数据持久化，账号与配置保存在 `./data` |
| `-e ADMIN_PASSWORD=xxx`    | 设置管理密码（默认 `admin`）      |
| `-e TZ=Asia/Shanghai`      | 时区设置（镜像已默认配置）           |
