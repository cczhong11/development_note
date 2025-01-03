1. PortainerPortainer 是一个非常流行的 Docker 管理工具，操作界面直观而友好，支持多种集群管理（比如 Docker Swarm 和 Kubernetes）。可以快速启动和管理多个容器、监控资源使用、设置权限等。安装步骤:首先，SSH 进入你的 AWS 实例。拉取并运行 Portainer 容器：
 
```
docker volume create portainer_data
docker run -d -p 9000:9000 -p 8000:8000 --name portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce
```

完成后，访问 http://your-aws-public-ip:9000 进入 Portainer WebUI，进行初始设置。优点：UI 直观，可以轻松启动/停止容器、管理网络、存储卷等。支持多用户权限管理。适用于多种环境，包括本地 Docker 和云上的 Docker 集群。提供资源监控、日志等。


在树莓派上的 Portainer Web UI 中添加 AWS 实例打开树莓派上的 Portainer Web UI（假设访问 http://raspberry-pi-ip:9000）。登录后，点击 Environments（环境） > Add Environment（添加环境）。选择 Agent（代理）选项来添加新环境。在 Agent Address（代理地址）输入框中，输入你的 AWS 实例的公网 IP 和 Portainer Agent 的端口 9001，例如 http://your-aws-public-ip:9001。给这个新环境取个名字，比如“AWS Docker”。点击 Connect（连接）进行连接。完成后，Portainer Web UI 应该会显示 AWS 实例上的 Docker 容器，并可以管理这些容器啦！3. 安全优化建议确保 AWS 安全组只允许来自树莓派的 IP 地址访问 Portainer Agent 的 9001 端口，以防止其他未授权访问。Portainer Web UI 支持用户权限管理，可以在 Portainer 上配置好用户名和密码来保护你的环境哦！这样你的 AWS 上的 Docker 就可以通过树莓派上的 Portainer Web UI 来集中管理啦！o(≧▽≦)o 任何问题都可以来问我哟！