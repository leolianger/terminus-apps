# Open Alice for Olares

本目录仅包含 **Olares 部署配置**（Chart、OlaresManifest、K8s 模板）。  
Docker 镜像的构建与推送在 **你的 OpenAlice fork 仓库** 中完成，不在此工程内构建。

## 分工说明

| 位置 | 用途 |
|------|------|
| **你的 fork**（如 `github.com/leolianger/OpenAlice`，本地 `olares/OpenAlice`） | 源码、Dockerfile、构建并推送镜像到 Docker Hub |
| **本目录**（`terminus-apps/openalice`） | Olares 应用包：Chart、入口、Deployment/Service、values 中配置要拉取的镜像 |

## 构建镜像（在 fork 仓库中做）

在 **OpenAlice 仓库** 根目录（例如 `/home/leolianger/work/01_code/olares/OpenAlice`）执行，详见该仓库中的 **README-Docker.md**：

```bash
cd /home/leolianger/work/01_code/olares/OpenAlice
docker login
docker build -t 你的DockerID/openalice:0.9.0-beta.6 .
docker push 你的DockerID/openalice:0.9.0-beta.6
```

## 在本工程中配置拉取镜像

编辑 **values.yaml**，将镜像改为你实际推送的仓库（例如 Docker Hub 用户名为 goai007 则用 `goai007/openalice`）：

```yaml
image:
  repository: docker.io/goai007/openalice
  tag: "0.9.0-beta.6"
  pullPolicy: IfNotPresent
```

之后在 Olares 中部署 openalice 时，会从 Docker Hub 拉取上述镜像。

## 部署到 Olares

将本 openalice 应用包上传到 Olares 应用市场，或使用 Helm 安装：

```bash
helm install openalice . -n YOUR_NAMESPACE --set userspace.appData=/path/to/appdata
```

## 数据持久化


应用数据（config、brain、sessions）落在该根路径下，在容器内挂载为 `/app/data`。
