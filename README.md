<img width="1825" height="378" alt="62ce3162-4a56-4101-b10f-bb1d685501e7" src="https://github.com/user-attachments/assets/82f95d9f-1da5-436b-a843-b98a6b90d650" /># K8s CI/CD 自动化发布链路 Demo

## 📌 项目简介
基于 Jenkins + Docker + Harbor + Kubernetes 实现的一套完整的自动化发布链路。
只需在 Jenkins 中一键构建，即可实现：**构建 Docker 镜像 -> 推送私有 Harbor 仓库 -> 触发 K8s 滚动更新** 的全流程自动化。

## 🛠 技术栈
- **CI/CD**: Jenkins (Declarative Pipeline)
- **容器化**: Docker (Nginx alpine)
- **镜像仓库**: Harbor (私有仓库)
- **编排部署**: Kubernetes (Deployment + Ingress)
- **底层运行时**: containerd (配置 HTTP 私有仓库信任)

## 🚀 核心亮点
1. **全链路闭环**：打通了从 CI 到 CD 的全部环节，零人工干预。
2. **解决底层网络痛点**：通过配置 containerd `hosts.toml` 与 K8s `imagePullSecrets`，完美解决 K8s 拉取 HTTP 私有 Harbor 镜像的认证与协议痛点。
3. **声明式流水线**：使用 Jenkinsfile 定义流水线，支持版本号（Tag）自动递增。

## 📸 运行效果
- **构建状态**：四阶段全绿（Prepare -> Build -> Push -> Deploy）
- **部署效果**：网页成功显示 🚀 Welcome to K8s CI/CD Demo!
Version: v7
Deployed by Jenkins + Docker + K8s
- *Nginx 字符编码问题（顺便解决）：并且我在 echo 写入 HTML 的时候，加 入了<meta charset="UTF-8">解决了乱码问题。
