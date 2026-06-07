# HandBrake Web UI (fnOS)

基于 Web 界面的 HandBrake 视频转码管理工具，支持队列管理、预设管理和 Intel QSV / VA-API 硬件加速。

适用于 fnOS App Center 安装，也支持通用 Docker 部署。

## 快速开始 (Docker)

```yaml
# docker-compose.yaml
services:
  handbrake-webui:
    image: ray5378/handbrake-webui-fnapp:latest
    container_name: handbrake-webui
    restart: unless-stopped
    ports:
      - 52389:52389
    volumes:
      - ./config:/config
      - /path/to/your/media:/drive
    devices:
      - /dev/dri:/dev/dri   # Intel QSV / VA-API 硬解（可选）
```

```bash
docker compose up -d
```

访问 `http://your-nas-ip:52389` 即可使用。

## fnOS 安装

从 [Releases](https://github.com/ray5378/HaddBrake-Webui-fnapp/releases) 下载 `.fpk` 文件，在 fnOS App Center 中手动安装。

安装向导中输入媒体目录路径（如 `/vol1/1000/Video`），该目录将在容器内映射为 `/drive`。

## 配置

在 App Center 中打开应用 → 设置页面可配置：
- **缓存目录**：转码临时文件存放位置
- **同时转码任务数**：并发任务数量

安装后也可在 App Center → 配置 中修改媒体目录路径，修改后容器自动重启。

## 硬件加速

容器自动检测 `/dev/dri` 设备。支持：
- Intel QSV（Quick Sync Video）
- VA-API（AMD / Intel）

无需额外配置。

## 构建

```bash
# 构建前端
cd src/frontend && npm ci && npm run build

# 构建后端
cd src/backend && npm ci && npm run build

# 构建 Docker 镜像
docker build -f src/docker/Dockerfile -t ray5378/handbrake-webui-fnapp:latest src
```

## License

MIT
