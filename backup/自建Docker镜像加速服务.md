由于某些原因，Docker Hub官方仓库在国内无法拉取，国内网络环境，需要配置镜像使用，从而解决Docker镜像拉取失败或缓慢问题。通过自建Docker镜像加速服务，提高Docker使用体验。

**使用开源项目[Docker-Proxy](https://github.com/dqzboy/Docker-Proxy/)**

### 📝 你需要
- 一台国外服务器，并且未被墙。
- 域名，无需进行国内备案。
🚀 如果你没有以上条件，那么你也可以试试使用第三方免费容器部署服务 [ClawCloud](https://claw.cloud/) 
⚠️ Clawcloud免费账户流量为 10GB/Mo ，酌情使用

### 📦 部署

```
# CentOS && RHEL && Rocky
yum -y install curl

# ubuntu && debian
apt -y install curl

bash -c "$(curl -fsSL https://raw.githubusercontent.com/dqzboy/Docker-Proxy/main/install/DockerProxy_Install.sh)"
```
    
![Image](https://github.com/user-attachments/assets/9cad509f-0791-43b0-91a8-b680736fe281)

### 💊 使用

域名替换配置如下：
![Image](https://github.com/user-attachments/assets/0e4854a6-2fab-4880-8f5f-9ea5b96c6fc3)

*.040720.xyz 为我自己部署的加速服务，使用Oracle🇺🇸服务器