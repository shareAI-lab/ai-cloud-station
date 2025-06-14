# ai-cloud-station

## 项目简介

**ai-cloud-station** 是一套面向全球协作团队的 AI 云端开发环境一键部署解决方案。通过 Docker 容器化技术，将顶级 AI 编码工具（如 Claude Code、OpenAI Codex、Devin 等）和现代开发环境集成在一起，帮助团队成员无论身处何地，都能安全、稳定、高效地使用最强 AI 编码能力。

## 核心价值

- **极致易用**：一键部署，5 分钟内为每位成员分配专属 AI 云端开发环境。
- **全球可用**：突破地域和网络限制，团队成员可随时随地访问。
- **安全合规**：代码、AI 认证集中在云端，避免本地泄露和账号风控。
- **高效协作**：新成员入职无需复杂配置，环境标准化，极大提升团队效率。
- **开源透明**：所有脚本、配置、流程均开源，便于自定义和二次开发。

## 适用场景

- 跨国/远程/分布式开发团队
- 需要统一 AI 编码环境的企业/创业公司
- 个人开发者希望体验 AI 3.0 时代的云端开发
- 教育/培训/编程教学场景

## 快速开始

### 1. 克隆仓库

```bash
git clone https://github.com/shareAI-Lab/ai-cloud-station.git
cd ai-cloud-station
```

### 2. 构建标准化开发镜像

```bash
sudo docker build -t ai-dev-env:latest .
```

### 3. 为每位成员一键部署专属环境

```bash
chmod +x deploy_user.sh
./deploy_user.sh 用户名 端口基数
# 例如 ./deploy_user.sh xinlu 10
```
- 脚本会自动分配端口、生成随机密码、初始化持久化目录、同步AI认证。
- 脚本执行成功后，会输出所有访问方式和凭证。

### 4. 认证同步/批量维护（如有需要）

```bash
chmod +x resync_auth.sh
./resync_auth.sh
```

### 5. 访问方式

- **VS Code Web**: `http://YOUR_SERVER_IP:端口`
- **noVNC 桌面**: `http://YOUR_SERVER_IP:端口`
- **SSH 终端**: `ssh dev@YOUR_SERVER_IP -p 端口`
- 具体端口和密码见脚本输出

## 进阶用法与最佳实践

### 1. 端口分配与资源限制

- 推荐每位成员分配独立端口段（如 10xx、20xx），避免冲突。
- 部署脚本默认为每个容器分配 2C/8G 资源，可根据实际需求调整 `--cpus` 和 `--memory` 参数。
- 支持批量部署、批量认证同步，适合 10-50 人团队。

### 2. 数据持久化与备份

- 所有用户代码、AI 配置均挂载到主机 `/srv/user-data/用户名`，容器重建不丢数据。
- 建议定期使用 `cron` 任务自动备份 `/srv/user-data/` 目录到云存储或 NAS，防止意外丢失。

### 3. 认证同步与自动化维护

- Claude 认证并非永久有效，管理员可在主机上重新登录后，运行 `resync_auth.sh` 一键同步所有用户认证，无需重启容器。
- 支持一键批量同步，极大降低维护成本。

### 4. 安全加固建议

- 建议仅开放必要端口，使用防火墙（如 ufw）限制访问来源。
- 推荐为 VS Code Web、noVNC 配置 HTTPS 访问，提升安全性。
- 支持 SSH 密钥认证，进一步提升安全等级。
- 所有认证配置均以只读方式挂载，防止泄露和篡改。

### 5. 性能与运维建议

- 镜像分层优化，减少构建时间和体积。
- 支持自定义 npm/pip 镜像源，加速依赖安装。
- 推荐定期更新基础镜像和工具，及时打安全补丁。
- 可通过 `docker stats`、`df -h` 等命令监控资源使用和健康状态。

## 常见问题（FAQ）

**Q1：如何批量为团队成员分配环境？**  
A：可编写简单的 shell 脚本循环调用 `deploy_user.sh`，或结合 CI/CD 工具实现自动化。

**Q2：Claude 认证失效怎么办？**  
A：管理员在主机上重新登录 Claude 后，运行 `resync_auth.sh` 即可一键同步，无需重启容器。

**Q3：如何实现 HTTPS/子域名访问？**  
A：推荐在主机部署 Nginx/Traefik 反向代理，为每位成员分配独立子域名并配置 SSL 证书。

**Q4：如何扩展更多 AI 工具或自定义开发环境？**  
A：可直接修改 Dockerfile，添加所需依赖和工具，重建镜像即可。

## 适用与不适用场景分析

- **最适合**：小型技术团队、重视开发环境标准化、需要快速扩展的企业/创业公司。
- **不推荐**：完全离线环境、对云端数据有极高敏感要求的场景。

## 监控与维护建议

- 定期监控容器 CPU/内存、磁盘使用、Claude API 状态。
- 建议每月更新镜像、工具和安全补丁。
- 支持日志审计，可通过 `docker run --log-driver=syslog ...` 启用。

## 用户反馈与真实体验

> "以前用 Claude Code断断续续，现在丝滑得很，重构大型组件再也不怕了。" —— 前端开发 
> "统一环境确实省心，新人入职直接给账号就能干活，不用再折腾各种配置。" —— 后端架构师 
> "团队开发效率明显提升，大家的代码质量也更一致了。" —— 项目经理 

## 贡献与支持

- 欢迎 Star、Fork、提 Issue、提 PR！
- 如需定制化部署、AI 账号代开、企业级支持、垂直场景Manus开发，欢迎联系 ShareAI 团队 


---

让 AI 赋能每一位开发者，让协作无国界。

---

如需更详细的技术原理、架构设计、实战经验和优化建议，欢迎查阅代码或 Issues 区交流、微信交流。   
ai-lab@foxmail.com
<img src="./qrcode.jpg" width="600">