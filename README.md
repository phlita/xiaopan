# 小网盘 (XiaoPan File Manager)

一个基于 PHP 的轻量级网页文件管理器，支持多种后端存储，并提供用户认证、文件操作和在线预览功能。

---

## 主要特点

### 通用功能
- 响应式设计，适应不同屏幕尺寸。
- 多用户登录认证，支持 2FA (双因素认证) 增强安全性。
- 可配置的会话管理和登录锁定机制。
- 通过配置文件 (`config.json`) 进行灵活的应用和后端设置。

### 文件操作
- 上传、下载、创建目录、删除、重命名文件和文件夹。
- 在线文本文件编辑，方便快速修改代码或文档。
- 支持客户端文件加密和解密，为文件内容提供额外保护（生成 `.xiaopan` 后缀）。

### 文件预览
- 在线图片预览（JPG, PNG, GIF, BMP, WebP, SVG 等）。
- 集成媒体播放器，支持主流音频（MP3, OGG, WAV 等）和视频（MP4, WebM, MOV 等）格式在线播放。
- 文本文件和代码文件在线预览（支持多种文本/代码扩展名）。

### 安全性
- 所有 POST 请求均受 CSRF (跨站请求伪造) 保护。
- IP 登录锁定，有效防止暴力破解攻击。
- 文件名自动清理，防止路径遍历和非法字符注入。
- 配置和用户数据文件（`config.json`, `users.json`）建议通过 `.htaccess` 或服务器配置进行保护，防止未经授权的直接访问。

---

## 支持的存储后端

本项目采用模块化设计，通过统一接口支持多种存储后端：

- **本地文件系统 (Local)**：直接管理服务器上的文件。
- **GitHub**：操作 GitHub 仓库中的文件。
- **FTP**：通过 FTP 协议连接服务器，支持 FTPES (显式 SSL/TLS)。
- **SFTP**：通过 SFTP 协议连接服务器，支持密码和私钥认证（需 PHP ssh2 扩展）。
- **WebDAV**：连接兼容 WebDAV 协议的服务器，支持多种认证方式（Basic, Digest, NTLM）。
- **七牛云 Kodo**：通过原生 API 与七牛云对象存储交互，支持私有/公共空间和自定义区域。
- **Amazon S3**：通过原生 API 与 AWS S3 兼容存储交互，支持 SignatureV4 签名和自定义 Endpoint（可用于 MinIO 等）。

---

## 版本说明

小网盘支持多种部署模式，适应不同场景：

### XiaoPan 全功能管理版 (Admin & Multi-Backend)

**描述**：默认完整版本，含用户登录系统、管理员面板、所有后端、用户账户管理、全部文件操作和在线预览功能。

- **用户登录与管理**：安全登录入口，支持添加/修改用户、启用 2FA。
- **集中式配置**：管理员可在 Web 界面动态切换和配置所有存储后端，无需手动编辑配置文件。
- **全套文件操作**：上传、下载、创建、删除、重命名、编辑等。
- **灵活性高**：适合需强大管理功能和灵活后端切换的场景。
- **适用场景**：私人云存储、小团队文件协作、Web 服务器文件管理等。

### XiaoPan 公共文件浏览版 (Public / Lite)

**描述**：移除用户登录和管理界面，成为轻量级公共文件浏览器。所有访问者无需登录即可查看文件列表。通常为只读模式，也可配置允许匿名上传（需谨慎）。

- **无需登录**：任何访问者可直接浏览文件。
- **无管理界面**：所有配置须直接编辑 `config.json`。
- **精简代码**：移除用户认证和大部分管理逻辑，代码更简洁。
- **部署简便**：适合快速搭建公开文件分享或媒体展示页面。
- **实现方式**：
    - 移除/注释 `index.php` 中与登录、会话、管理员操作相关的代码。
    - `config.json` 中 `show_file_list_logged_out` 设为 `true`。
    - （可选）需公共上传时，确保后端允许匿名写入，并移除上传表单的认证检查。
- **适用场景**：作品集展示、公共文档库、图片画廊、视频播放、简单文件分享等。
- **安全提示**：无认证模式安全风险高，仅建议在受限环境部署。务必配置后端权限，严禁在生产环境处理敏感数据或允许匿名写入，除非完全理解安全影响。

---

## 安装

1. **环境要求**：
   - PHP 7.4+（推荐 PHP 8.0+），按所选后端启用相关 PHP 扩展（如 curl, ssh2, simplexml）。
2. **下载**：
   - 将项目代码下载并解压到 Web 服务器根目录或子目录。
3. **文件权限**：
   - 确保 `log/` 目录及 `config.json`, `users.json` 文件对 Web 服务器有写权限（如 `chmod 0755 log`, `chmod 0644 config.json users.json`）。
4. **安全配置**：
   - **.htaccess**：建议根目录配置 .htaccess 禁止直接访问 `.json` 文件，保护敏感信息。本项目会自动尝试添加该规则。
   - **默认密码**：首次登录（默认：admin/admin）后，请立即修改默认密码。
5. **访问**：
   - 浏览器访问项目 URL（如 `http://yourdomain.com/path/to/xiaopan/` 或 `http://localhost/xiaopan/`）。

---

## 使用

1. **登录**：
   - 初次使用请用默认管理员账户 `admin/admin` 登录。
2. **配置**：
   - 登录后进入“设置”，配置应用名、会话超时、安全选项、文件预览及后端存储（如 Local, GitHub, S3 等）。
3. **文件管理**：
   - 文件列表视图中可浏览、上传、创建目录、重命名、删除文件等。
4. **账号管理**：
   - “账号”菜单可修改当前用户密码和 2FA 设置。

---

## 配置 (`config.json`)

- `config.json` 是核心配置文件，定义全局设置和各种后端参数。请按需修改。
- `users.json` 存储用户凭据（含密码哈希和 2FA 邮箱），请勿手动修改，用户管理应通过 Web 界面完成。

---

# XiaoPan File Manager

A lightweight, PHP-based web file manager supporting various backend storage solutions, providing user authentication, file operations, and online preview functionalities.

---

## Key Features

### General
- Responsive design for various screen sizes.
- Multi-user login authentication with 2FA (Two-Factor Authentication) for enhanced security.
- Configurable session management and login lockout mechanisms.
- Flexible application and backend settings via `config.json`.

### File Operations
- Upload, download, create directories, delete, rename files and folders.
- Online text file editing for quick modifications of code or documents.
- Supports client-side file encryption and decryption (generates `.xiaopan` suffix).

### File Preview
- Online image preview (JPG, PNG, GIF, BMP, WebP, SVG, etc.).
- Integrated media player supporting popular audio (MP3, OGG, WAV, etc.) and video (MP4, WebM, MOV, etc.) formats.
- Online preview for text and code files (supports various extensions).

### Security
- All POST requests are protected against CSRF.
- IP-based login lockout to prevent brute-force attacks.
- Automatic filename sanitization to prevent path traversal and illegal characters.
- Sensitive configuration and user data files (`config.json`, `users.json`) are recommended to be protected via `.htaccess` or server configuration.

---

## Supported Storage Backends

- **Local**: Manages files directly on the server.
- **GitHub**: Interacts with files in a GitHub repository.
- **FTP**: Connects via FTP protocol, supports FTPES (Explicit SSL/TLS).
- **SFTP**: Connects via SFTP protocol, supports password/private key authentication (requires PHP ssh2 extension).
- **WebDAV**: Connects to any WebDAV compliant server.
- **Qiniu Kodo**: Native API integration, supports private/public spaces and custom regions.
- **Amazon S3**: Native API integration, supports SignatureV4 and custom endpoints (e.g., MinIO).

---

## Version Differentiations

### XiaoPan Full Management Edition (Admin & Multi-Backend)

- **Description**: Full version with user login, admin panel, all backends, account management, full file operations, and online preview.
- **Features**:
  - Secure login and user management with 2FA.
  - Switch and configure backends via the web interface.
  - Comprehensive file operations.
  - High flexibility.
- **Use Cases**: Private cloud, small team collaboration, web server file management.

### XiaoPan Public File Browser Edition (Public / Lite)

- **Description**: Lightweight public file browser (no login/admin). All visitors can view files; can be configured for anonymous upload (use with caution).
- **Features**:
  - No login required.
  - All config via `config.json`.
  - Streamlined code.
  - Quick deployment.
- **Implementation**:
  - Remove/comment login/session/admin code in `index.php`.
  - Set `show_file_list_logged_out` to `true` in `config.json`.
  - (Optional) Allow anonymous uploads by backend config and removing upload auth checks.
- **Use Cases**: Public resource sharing, personal static hosting, media galleries, file drop-off points.
- **Security Warning**: High risk. Deploy in restricted environments and configure backend permissions. Do NOT use for sensitive data or unauthenticated writes unless fully understood.

---

## Installation

1. **Requirements**: PHP 7.4+ (8.0+ recommended), appropriate PHP extensions for your backend (e.g., curl, ssh2, simplexml).
2. **Download**: Download and extract to your web server's root or subdirectory.
3. **File Permissions**: Ensure `log/` and `config.json`, `users.json` are writable by your server (`chmod 0755 log`, `chmod 0644 config.json users.json`).
4. **Security Configuration**:
   - `.htaccess`: Deny access to `.json` files to protect sensitive info (auto-added if possible).
   - **Default Password**: Change the default admin password (`admin/admin`) immediately after first login.
5. **Access**: Open your project URL in a browser (e.g., http://yourdomain.com/path/to/xiaopan/ or http://localhost/xiaopan/).

---

## Usage

1. **Login**: First use the default admin account `admin/admin`.
2. **Configuration**: After login, go to "Settings" to configure the application, session, security, preview, and desired storage backends.
3. **File Management**: Browse, upload, create, rename, and delete files in the file list view.
4. **Account Management**: Change your password and 2FA settings in the "Account" menu.

---

## Configuration (`config.json`)

- `config.json` is the core config file for global and backend settings. Modify as needed.
- `users.json` stores user credentials (hashed password, 2FA email). Do NOT edit manually; manage accounts via the web interface.
