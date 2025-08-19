# Scalingo Xray 伪装部署项目

这是一个用于在 Scalingo 平台上部署 Xray (VLESS + WS + TLS) 节点的模板项目。它内置了一个静态网页作为伪装，以降低被平台检测的风险。

**警告：在 Scalingo 上运行代理服务违反其服务条款，你的应用和账户随时可能被封禁。请自行承担风险。**

## 部署步骤

### 1. 准备 GitHub 仓库

- **创建仓库**：在你的 GitHub 账户上创建一个新的**私有 (Private)**仓库。
- **上传文件**：将本项目的所有文件 (`.gitignore`, `Procfile`, `config.json`, `index.html`, `README.md`) 上传到这个新的私有仓库中。

### 2. 部署到 Scalingo

1.  登录 Scalingo Dashboard，创建一个新应用，选择地区（如 `osc-fr1`）。
2.  进入应用的 **"Deployment"** 页面。
3.  点击 **"Link to GitHub"**，授权 Scalingo 访问你的 GitHub 账户。
4.  选择你刚刚创建的那个私有仓库。
5.  **部署分支 (Branch to deploy)** 选择 `main` (或 `master`)。
6.  **开启 "Automatic deployments"**。
7.  点击链接按钮，Scalingo 将开始第一次部署。这次部署会因为缺少环境变量而失败，这是正常现象。

### 3. 设置环境变量

1.  进入应用的 **"Environment"** 页面。
2.  点击 **"Add variable"** 添加以下两个变量：
    -   **`UUID`**: 粘贴你提前准备好的 UUID。
    -   **`WSPATH`**: 设置一个自定义的 WebSocket 路径，**必须以 `/` 开头**，例如 `/query-data-123`。请务必使用你自己想的、无规律的路径。

3.  添加完变量后，回到 **"Deployment"** 页面，手动触发一次新的部署（点击 "Deploy" 按钮）。

### 4. 获取连接信息并配置客户端

部署成功后，你的节点信息如下：

-   **协议类型**: VLESS
-   **地址 (Address)**: `你的应用名.scalingo.io` (如果绑定了自定义域名，请使用自定义域名)
-   **端口 (Port)**: `443`
-   **用户 ID (UUID)**: 你设置的 `UUID` 环境变量的值
-   **传输协议 (Network)**: `ws` (WebSocket)
-   **路径 (Path)**: 你设置的 `WSPATH` 环境变量的值
-   **底层传输安全 (TLS)**: `tls` (或在客户端中选择 "开启 TLS")

直接访问你的应用域名 `https://你的应用名.scalingo.io` 应该能看到一个伪装网页。
