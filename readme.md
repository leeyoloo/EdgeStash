# EdgeStash | 一款全新的Cloudflare R2云盘，支持带密码分享、查看下载量、管理用户等功能


**EdgeStash** 是一个功能强大、易于部署的私有云盘解决方案，完全构建在 Cloudflare 的全球网络之上。它利用 **Cloudflare Workers**、**R2 存储** 和 **KV 存储**，为您提供一个安全、快速且低成本的个人或团队文件存储与分享平台。

EdgeStash**支持带密码分享文件、在线预览docx或pdf文档、后台管理授权用户、查看分享文件浏览/下载量！**

这个项目旨在提供一个"一键部署"的体验，您只需要一个 Cloudflare 账户，即可在几分钟内拥有属于自己团队的云盘服务。

## 🛸 预览

<!-- - 🌍 官网：[xxx.com未完成](https:// /)
- 😃 部署视频教程：未制作完成 -->

|                                    |                                    |
| ---------------------------------- | ---------------------------------- |
| ![Demo](.//imgs/1.png) | ![Demo](./imgs/2.png) |
| ![Demo](.//imgs/3.png) | ![Demo](./imgs/4.png) |

### 视频：功能快速预览 + 部署教程
https://www.bilibili.com/video/BV1s2FPzqEdA/

## ✨ 核心特性

- **👑多用户系统**：
  - **管理员**：拥有最高权限，可通过密码登录，管理用户、分享链接和查看统计数据。
  - **授权用户**：由管理员授权创建（邮箱+密码），授权用户仅可浏览文件首页，无法进入管理后台。
  - **普通用户**：使用密码下载被分享文件，无密码保护的分享文件可直接下载。

- **⏳灵活的文件分享**：
  - **密码保护**：为分享链接设置访问密码。
  - **有效期设置**：支持 1 小时、1 天、1 个月或永久有效。
  - **访问统计**：跟踪每个分享链接的浏览和下载次数。

- **🧸在线文件预览**：
  - 无需下载，直接在浏览器中预览多种文件格式。
  - **文档**：Word (.docx), PDF, Markdown (.md), 纯文本 (.txt)。
  - **图片**：JPG, PNG, GIF, WebP, SVG 等。
  - **代码/数据**：JSON 文件自动格式化。
  - **音视频**：MP4, WebM, MP3, WAV 等。

- **📁强大的文件管理**：
  - 支持文件和文件夹的创建、重命名、移动和删除。
  - 拖拽式文件上传和多文件上传。
  - 面包屑导航，轻松在不同层级目录间穿梭。

- **🪣 S3 兼容 API**：
  - 通过 AWS Signature V4 认证，兼容 `aws cli`、`rclone`、Cyberduck 等 S3 客户端。
  - 支持完整的文件操作：上传、下载、删除、复制、列举。
  - 支持 Multipart Upload（分块上传），可传输大文件。
  - 支持 Range 请求（断点续传）。
  - 通过管理后台生成和管理 Access Key / Secret Key。

- **📂 WebDAV 服务端**：
  - 对外暴露标准 WebDAV 协议（`/dav/*` 路径）。
  - 支持 macOS Finder、Windows 资源管理器、iOS 文件、Solid Explorer 等客户端直接挂载。
  - 支持 PROPFIND、GET、PUT、DELETE、MKCOL、COPY、MOVE、LOCK 等方法。
  - 支持 HTTP Range 请求（断点续传）。
  - 支持 EPUB、MOBI、CBZ、CBR 等电子书 MIME 类型。
  - 使用 Basic Auth 认证，复用现有用户系统。

- **📖 OPDS 目录服务**：
  - 内置 OPDS 1.1 协议支持（`/opds` 路径），兼容 Reeden、KyBook 等阅读软件。
  - 自动扫描文件夹，列出所有电子书（EPUB、MOBI、PDF、CBZ、TXT）。
  - 每个 EPUB 条目自动附带封面图链接，阅读软件可直接获取封面。
  - 支持文件夹层级浏览。

- **🖼️ EPUB 封面提取**：
  - 通过 `/api/cover/*` 接口从 EPUB 文件中提取封面图片。
  - 支持多种封面查找策略（meta cover、properties cover-image、文件名匹配等）。
  - 提取后自动缓存到 R2，后续请求秒返回。

- **🔧管理后台**：
  - **数据统计**：实时查看总分享数、总浏览量和总下载量。
  - **用户管理**：轻松添加新用户、撤销现有用户授权。
  - **分享管理**：集中查看和删除所有已创建的分享链接。
  - **S3/WebDAV 管理**：创建和管理 S3 Access Key，查看接入点信息。

- **💡现代化界面**：
  - 紫蓝色系渐变配色，美观、专业。
  - 完全响应式设计，适配桌面、平板和手机。
  - 流畅的动画和操作反馈提示。

- **🙈安全与隐私**：
  - 所有数据存储在您自己的 R2 和 KV 中，完全掌控。
  - 通过 JWT (JSON Web Tokens) 进行安全的会话管理。

## 🚀 部署指南

部署 EdgeStash 非常简单，全程在 Cloudflare Dashboard 中完成。

### 前置要求

- 一个 Cloudflare 账户。
- 已开通 R2 和 Workers 、 KV 服务。

### 部署步骤

| 配置   | 变量名          | 说明              |
| :----- | :----------------------------- | :------------------------- |
| `R2` | `R2_BUCKET` | 存放文件的地方，名称随意|
| `KV` | `KV_STORE`| 存放链接的地方，名称随意|
| `管理员密码` | `ADMIN_PASSWORD`| 你想设置什么都行  |


1.  **创建 R2 存储桶**
    - 登录 Cloudflare -> R2 -> 创建存储桶。
    - 记下您的存储桶名称（例如 `edgestash-files`）。

2.  **创建 KV 命名空间**
    - 登录 Cloudflare -> Workers 和 Pages -> KV -> 创建命名空间。
    - 记下您的命名空间名称（例如 `edgestash-kv`）。

3.  **创建 Worker**
    - 登录 Cloudflare -> Workers 和 Pages -> 创建应用程序 -> 创建 Worker。
    - 为您的 Worker 命名（例如 `edgestash`），然后点击 **部署**。

4.  **上传代码**
    - 在 Worker 页面，点击 **编辑代码**。
    - 将本项目提供的 `worker.js` 文件内容完整粘贴进去。
    - 点击 **部署**。

5.  **配置绑定**
    - 返回 Worker 概览页面，点击 **设置** -> **变量和机密**。
    - **配置环境变量**：
        - `ADMIN_PASSWORD`：设置您的管理员登录密码。
    - **配置 R2 绑定**：
        - 变量名称：`R2_BUCKET`
        - R2 存储桶：选择您在第 1 步创建的存储桶。
    - **配置 KV 绑定**：
        - 变量名称：`KV_STORE`
        - KV 命名空间：选择您在第 2 步创建的命名空间。

6.  **完成！**
    - 访问您的 Worker URL (`https://<worker-name>.<subdomain>.workers.dev`) 即可开始使用！
7.  **绑定你自己的域名！**

### 😤已知的小bug
管理员登录时如果点击`登录`按钮无反应，切换到`用户登录`，把`用户登录`的`邮箱`**输入框内容清空**，然后重新进行管理员登录即可~

## 🔌 S3 / WebDAV 接入指南

### S3 兼容 API

在管理后台的「S3/WebDAV」标签页创建 Access Key 后，可使用任何 S3 兼容客户端连接。

**Endpoint**: `https://<your-worker>.workers.dev`

#### aws cli

```bash
# 配置凭证
aws configure
# 输入 Access Key ID 和 Secret Key
# Region 输入任意值（如 us-east-1）

# 列出存储桶（即根目录下的文件夹）
aws --endpoint-url https://your-worker.workers.dev s3 ls

# 上传文件
aws --endpoint-url https://your-worker.workers.dev s3 cp ./myfile.txt s3://mybucket/

# 下载文件
aws --endpoint-url https://your-worker.workers.dev s3 cp s3://mybucket/myfile.txt ./

# 列出桶内文件
aws --endpoint-url https://your-worker.workers.dev s3 ls s3://mybucket/
```

#### rclone

```bash
# 交互式配置
rclone config
# 选择: New remote → 输入名称 → type=s3 → provider=Other
# endpoint=https://your-worker.workers.dev
# access_key_id=你的AccessKey
# secret_access_key=你的SecretKey

# 使用
rclone ls myremote:/mybucket/
rclone copy ./myfile.txt myremote:/mybucket/
```

#### macOS Finder

前往 → 连接服务器 → 输入 `https://your-worker.workers.dev/dav/`

#### Windows

映射网络驱动器 → 输入 `https://your-worker.workers.dev/dav/`

### WebDAV

Endpoint: `https://<your-worker>.workers.dev/dav/`

认证方式：使用 EdgeStash 的用户邮箱和密码（Basic Auth）。

支持的客户端：
- macOS Finder / iOS 文件 app
- Windows 资源管理器
- Solid Explorer、ES文件浏览器（Android）
- Cyberduck、Mountain Duck
- 任何标准 WebDAV 客户端

### OPDS（阅读软件接入）

Endpoint: `https://<your-worker>.workers.dev/opds`

认证方式：使用 EdgeStash 的用户邮箱和密码（Basic Auth）。

支持的客户端：
- Reeden（iOS/Android）
- KyBook（iOS）
- Moon+ Reader（Android）
- 任何标准 OPDS 阅读器

使用方式：在阅读软件中添加 OPDS 书源，输入上述地址和账号密码即可浏览和下载电子书，封面自动显示。

### 🔐 安全建议

**⚠️ 重要安全提示：**

1. **S3 密钥安全**：创建 Access Key 后，Secret Key 只显示一次。请妥善保管，泄露可能导致未授权访问。
2. **使用自定义域名**：强烈建议绑定自定义域名并启用 HTTPS，不要使用默认的 `workers.dev` 域名用于生产环境。
3. **WebDAV 使用 HTTPS**：WebDAV 的 Basic Auth 仅做 Base64 编码（非加密），必须通过 HTTPS 传输。
4. **定期轮换密钥**：建议定期更换 S3 Access Key，可通过管理后台禁用旧密钥并创建新密钥。
5. **管理员密码强度**：`ADMIN_PASSWORD` 应使用强密码，因为它是所有认证的基础。
6. **R2 访问控制**：确保 R2 存储桶没有配置公开访问权限，所有访问应通过 Worker 进行。

### 💰 费用说明

EdgeStash 本身免费，但使用 Cloudflare 服务可能产生费用：

| 项目 | 免费额度 | 超出后费用 |
|:-----|:---------|:-----------|
| Workers 请求 | 100,000 次/天 | $0.30 / 百万次 |
| Workers CPU 时间 | 10ms/请求（免费版） | - |
| R2 存储 | 10 GB/月 | $0.015 / GB/月 |
| R2 A 类操作（写入） | 100 万次/月 | $4.50 / 百万次 |
| R2 B 类操作（读取） | 1000 万次/月 | $0.36 / 百万次 |
| KV 读取 | 10 万次/天 | $0.50 / 百万次 |
| KV 写入 | 1,000 次/天 | $5.00 / 百万次 |
| KV 存储 | 1 GB | $0.50 / GB/月 |
| 出站流量 | 免费 | 免费 |

**一般使用场景**：个人或小团队使用，大概率在免费额度内，**基本不会产生费用**。

**可能产生费用的场景**：
- 大量文件操作（通过 S3/WebDAV 频繁读写）
- 存储超过 10GB 的文件
- Workers 超出每日 10 万次请求

**建议**：在 Cloudflare Dashboard 中设置用量提醒，避免意外超支。

## 📚 API 接口参考

EdgeStash 通过一套 RESTful API 提供服务，以下是核心接口列表。

| 方法   | 路径                           | 说明                       |
| :----- | :----------------------------- | :------------------------- |
| **认证** |                              |                            |
| `POST` | `/api/login`                   | 管理员或用户登录           |
| `POST` | `/api/logout`                  | 退出登录                   |
| `GET`  | `/api/auth/check`              | 检查当前登录状态           |
| **文件管理** |                            |                            |
| `GET`  | `/api/files/*`                 | 获取指定路径下的文件和文件夹 |
| `POST` | `/api/files/*`                 | 上传文件到指定路径         |
| `PUT`  | `/api/files/*`                 | 重命名文件或文件夹         |
| `DELETE`| `/api/files/*`                 | 删除文件或文件夹           |
| `POST` | `/api/folders`                 | 创建新文件夹               |
| `GET`  | `/api/download/*`              | 下载文件                   |
| `GET`  | `/api/preview/*`               | 获取文件内容用于在线预览   |
| **分享管理** |                            |                            |
| `POST` | `/api/share`                   | 为文件创建分享链接         |
| `GET`  | `/api/share/:id`               | 获取分享链接信息           |
| `POST` | `/api/share/:id/download`      | 下载分享的文件             |
| **管理后台** |                            |                            |
| **OPDS & 封面** |                            |                            |
| `GET`  | `/opds`                        | OPDS 根目录（电子书列表）  |
| `GET`  | `/opds/*`                      | 浏览子文件夹               |
| `GET`  | `/api/cover/*`                 | 提取 EPUB 封面图片         |
| **管理后台** |                            |                            |
| `GET`  | `/api/admin/stats`             | 获取统计数据               |
| `GET`  | `/api/admin/shares`            | 列出所有分享链接           |
| `DELETE`| `/api/admin/shares/:id`        | 删除指定的分享链接         |
| `GET`  | `/api/admin/users`             | 列出所有授权用户           |
| `POST` | `/api/admin/users`             | 创建新用户                 |
| `DELETE`| `/api/admin/users/:email`      | 删除指定用户               |
| **S3 密钥管理** |                            |                            |
| `GET`  | `/api/admin/s3keys`            | 列出所有 S3 Access Key     |
| `POST` | `/api/admin/s3keys`            | 创建新的 S3 密钥对         |
| `DELETE`| `/api/admin/s3keys/:id`        | 删除指定 S3 密钥           |
| `POST` | `/api/admin/s3keys/:id/toggle` | 启用/禁用 S3 密钥          |

## 📜 开源协议

本项目采用 **MIT License** 开源。
您可以自由地使用、修改、分发本项目的代码，但需要在您的衍生作品中包含原始的版权和许可声明。

---
## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=hhy-2021/EdgeStash&type=date&legend=top-left)](https://www.star-history.com/#hhy-2021/EdgeStash&type=date&legend=top-left)
