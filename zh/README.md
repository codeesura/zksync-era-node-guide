# zkSync Era 节点配置指南

本指南逐步解释如何在 Ubuntu Linux 上配置 `zkSync Era` 节点。

## 系统要求

- CPU：建议使用相对现代的 CPU。
- 内存：32 GB
- 存储：300 GB，状态增长约为每月 1 TB。
- 网络：100 Mbps 连接（建议 1 Gbps 以上）

### 服务器租赁

如果您需要服务器，您可以从 Hetzner 租用。此外，如果您使用此推荐链接注册，您将获得 20 欧元的信用额度。

[Hetzner](https://hetzner.cloud/?ref=fu2umOyLCWhh)

## 第一步：安装 Visual Studio Code（在本地计算机上）

1. **下载并安装 Visual Studio Code：**
   [Visual Studio Code 下载页面](https://code.visualstudio.com/)

   - 下载完成后，按照安装向导安装 Visual Studio Code。

2. **安装 SSH 扩展：**
   - 打开 Visual Studio Code。
   - 点击左侧边栏的 `扩展` 图标。
   - 在搜索栏中输入 `Remote - SSH`，然后安装 Microsoft 开发的扩展。

## 第二步：通过 SSH 连接到服务器（从本地计算机）

1. **在 Visual Studio Code 中连接到 SSH：**

   - 在 Visual Studio Code 中按 `Ctrl+Shift+P`，选择 `Remote-SSH: Connect to Host...`。
   - 输入服务器地址，格式为 `ssh root@ip_address`。
   - 输入您的密码或 SSH 密钥以完成连接。

   **注意：** 将 `ip_address` 替换为服务器的实际 IP 地址。

2. **验证连接：**

   - 如果连接成功，连接的服务器名称将显示在 Visual Studio Code 窗口的左下角。

3. **打开终端：**
   - 连接到服务器后，可以通过点击顶部菜单的 `终端` > `新终端` 打开终端。
   - 或者，可以按 `Ctrl+`（Control 键和位于 ESC 键下方的反引号键）打开终端。

## 第三步：安装 Docker 和 Docker Compose（在服务器上）

成功连接到服务器后，在服务器上执行这些命令。

### 安装 Docker

1. **更新系统包：**

   ```sh
   sudo apt update
   ```

2. **安装 Docker：**

   ```sh
   sudo apt install docker.io
   ```

3. **验证 Docker 服务正在运行：**

   ```sh
   sudo systemctl status docker
   ```

4. **验证 Docker 安装：**
   ```sh
   docker --version
   ```

### 安装 Docker Compose

1. **下载 Docker Compose：**

   ```sh
   sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ```

2. **授予 Docker Compose 执行权限：**

   ```sh
   sudo chmod +x /usr/local/bin/docker-compose
   ```

3. **验证安装：**
   ```sh
   docker-compose --version
   ```

## 第四步：启动 zkSync Era 节点（在服务器上）

1. **安装 Git（如果尚未安装）：**

   ```sh
   sudo apt install git
   ```

2. **克隆仓库：**

   ```sh
   git clone https://github.com/matter-labs/zksync-era.git
   ```

3. **导航到克隆的目录：**

   ```sh
   cd zksync-era/docs/guides/external-node
   cd docker-compose-examples
   ```

4. **使用 Docker Compose 启动节点：**

   ```sh
   docker-compose -f mainnet-external-node-docker-compose.yml up -d
   ```

5. **验证正在运行的容器：**

   ```sh
   docker ps
   ```

   此命令列出当前正在运行的 Docker 容器。您应该会看到与 `zkSync Era` 节点相关的容器。

6. **查看并跟踪最后 100 个日志：**
   ```sh
   docker logs -f --tail 100 docker-compose-examples_external-node_1
   ```

按 `Ctrl+C` 退出日志视图。这不会停止您的节点；它将继续在后台运行。

## 第五步：通过端口转发访问仪表板（在本地计算机上）

运行 Docker 后，您可以通过端口 3000 访问仪表板。要从本地计算机查看此仪表板，您需要在 Visual Studio Code 中配置端口转发。

1. **配置端口转发：**

   - 按 `Ctrl+Shift+P` 打开命令面板。
   - 输入 `Forward a Port` 并选择该选项。
   - 输入 `3000` 作为端口号并按回车键。

   或者：

   - 点击 Visual Studio Code 右下角的 `PORTS` 部分。
   - 从菜单中选择 `Forward Port...`。
   - 输入 `3000` 作为端口号并按回车键。

2. **访问仪表板：**

   - 打开您的 Web 浏览器，转到 `http://localhost:3000/d/0/external-node`。
   - 您现在应该可以访问仪表板了。

此步骤使您可以轻松地从本地计算机查看在远程服务器上运行的应用程序的仪表板。

## 其他信息

- **查看容器日志：**

  ```sh
  docker logs <container_id>
  ```

- **停止节点：**
  ```sh
  docker-compose -f mainnet-external-node-docker-compose.yml down
  ```
