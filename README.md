# Auto BitBrowser 管理系统

![License](https://img.shields.io/badge/license-MIT-blue.svg) ![Python](https://img.shields.io/badge/python-3.12-blue.svg)

这是一个基于 **FastAPI + Vue 3 + Playwright/BitBrowser API** 的自动化管理系统，面向 Google 账号批处理场景，支持账号管理、窗口管理、2FA 设置/修改、资格验证、年龄验证与绑卡订阅等任务。

本项目为对 **https://github.com/Leclee/auto_bitbrowser** 的二次开发版本。

系统使用 **BitBrowser 指纹浏览器**，通过本地 API（默认 `127.0.0.1:54345`）进行窗口与指纹配置控制。

## 📌 项目说明 / Project Note

**中文**：
- 只是看到别人的项目做着玩玩，没有弄很多号，也不是号商。
- 主要想做自动绑卡与年龄验证。
- 项目有很多 bug，后续也不会维护，开发者只是玩玩。

**English**:
- This is just a for-fun fork of someone else's project. I don't run many accounts and I'm not a seller.
- The main goal is automated card binding and age verification.
- The project has many bugs and won't be maintained; it's just a hobby project.

---

## 🎯 使用背景与目标

我的主要目标是完善这个系统：为了省钱购买了大量**没有 2FA** 的邮箱账号，但这带来了很大麻烦（需要批量设置 2FA）。

此外，之所以加入**年龄验证**，是因为我发现很多账号在完成年龄验证后就拥有了学生资格，从而可以批量处理后续流程。

目前系统已支持：

* 自动设置 2FA
* 自动修改 2FA
* 自动资格验证（学生资格）
* 自动绑卡订阅
* 年龄验证（使用虚拟卡验证）



---

## ✨ 功能特性 (Features)

* **Web 管理界面**：账号管理、筛选搜索、批量导入/导出、实时日志与进度展示。
* **浏览器窗口管理**：创建、恢复、同步、打开/关闭窗口。
* **任务编排与并发**：按账号顺序执行任务，支持并发数配置与实时进度推送。
* **2FA 自动化**：自动设置 2FA、修改 2FA，并将密钥同步到指纹浏览器配置。
* **资格验证**：自动获取 SheerID 链接并完成资格验证，识别账号状态。
* **年龄验证**：使用虚拟卡完成年龄验证，提升学生资格覆盖率。
* **绑卡订阅**：自动处理多层 iframe，完成绑卡与订阅流程。
* **多语言适配**：自动切换账号语言为英文，减少多语言导致的操作失败。
* **数据统一**：SQLite 为单一数据源，自动同步历史文本文件。

## 🛠️ 安装与使用 (Installation & Usage)

### 环境准备

- **Python**: 3.11+（推荐 3.12）
- **Node.js**: 18+
- **uv**: Python 环境管理工具（用于一键创建虚拟环境并安装依赖）
- **BitBrowser**: 本地已安装并可访问 API（默认 `127.0.0.1:54345`）

### 项目环境配置

```bash
# 1) 安装依赖工具（示例）
pip install uv

# 2) 创建并同步 Python 依赖（会生成 .venv）
uv sync

# 3) 安装前端依赖
cd web/frontend
npm install
```

### 方式一：一键启动 Web UI (推荐)

```bash
./start_web.sh
```

启动后访问：
- 前端: http://localhost:5173
- 后端 API: http://localhost:8000
- API 文档: http://localhost:8000/docs

### 方式二：手动启动

```bash
# 后端
uv run python -m uvicorn web.backend.main:app --reload --port 8000

# 前端
cd web/frontend
npm install
npm run dev
```

## ⚙️ 配置文件说明 (Configuration)

> **🔒 安全提示**: 以下配置文件包含敏感信息（账号密码、2FA密钥、卡号等），请确保：
> 1. **不要提交到 Git 仓库**：这些文件已在 `.gitignore` 中配置
> 2. **妥善保管**：建议加密存储或使用密码管理器
> 3. **定期备份**：避免数据丢失
> 4. **使用示例文件**：参考 `accounts_example.txt` 创建自己的配置

请在程序运行目录下创建以下文件：

> **Web 配置**：在 Web 管理界面的“配置”页可填写 SheerID API Key 与虚拟卡信息，系统会优先使用数据库配置；未配置时才会回退到 `cards.txt`。

### 1. `accounts.txt` (账号信息)

**📌 分隔符配置方式**

在文件**第一行**配置分隔符（取消注释即可）：

```text
# 分隔符配置（取消注释其中一行）
分隔符="----"
# 分隔符="---"
# 分隔符="|"
# 分隔符=","
```

**📋 账号格式说明**

格式（字段顺序固定）：`邮箱[分隔符]密码[分隔符]辅助邮箱[分隔符]2FA密钥`

```text
# 标准格式（使用 ---- 分隔）
分隔符="----"
example1@gmail.com----MyPassword123----backup1@email.com----ABCD1234EFGH5678
example2@gmail.com----P@ssw0rd!%%99----backup2@email.com----WXYZ9012STUV3456

# 只有邮箱和密码（辅助邮箱和2FA可选）
example3@gmail.com----ComplexP@ss#2024

# 使用竖线分隔
分隔符="|"
example4@gmail.com|AnotherPass!|QRST5678UVWX1234

# 使用三短横线
分隔符="---"
example5@gmail.com---My#Pass@456---helper@email.com---LMNO3456PQRS7890
```

**✅ 重要说明**：
- **字段顺序固定**：邮箱 → 密码 → 辅助邮箱 → 2FA密钥
- **密码支持特殊字符**：`@#$%^&*`等都可以
- **辅助邮箱和2FA是可选的**：可以只填邮箱和密码
- **注释**：以 `#` 开头的行会被忽略
- **一个文件只能用一种分隔符**

**💡 推荐分隔符**：
- `----` (四短横线) - 推荐，最清晰
- `---` (三短横线) - 也很好用
- `|` (竖线) - 简洁
- `,` (逗号) - 需注意密码中不能有逗号

### 2. `proxies.txt` (代理IP)

支持 Socks5/HTTP，一行一个：

```text
socks5://user:pass@host:port
http://user:pass@host:port
```

### 3. `cards.txt` (虚拟卡信息) 🆕

格式：`卡号 月份 年份 CVV`（空格分隔）

```text
5481087170529907 01 32 536
5481087143137903 01 32 749
```

**说明**：
- **卡号**：13-19位数字
- **月份**：01-12（两位数）
- **年份**：年份后两位，如2032年填32
- **CVV**：3-4位安全码
- 每行一张卡，用于一键绑卡订阅功能

💳 **虚拟卡推荐**：[HolyCard](https://www.holy-card.com/) - 支持Gemini订阅、GPT Team、0刀Plus，一张低至2R

### 4. 输出文件 (程序自动生成)

* **accounts.db**: SQLite 数据库文件（所有账号信息的核心存储）。
* **sheerIDlink.txt**: 成功提取的验证链接 (有资格待验证已提取链接)。
* **有资格待验证号.txt**: 有资格但还未提取验证链接的账号。
* **已验证未绑卡.txt**: 已通过学生验证但未绑卡的账号。
* **已绑卡号.txt**: 已完成绑卡订阅的账号。
* **无资格号.txt**: 检测到无资格 (不可用) 的账号。
* **超时或其他错误.txt**: 提取超时或发生错误的账号。
* **sheerID_verified_success.txt**: 验证成功的 SheerID 链接。
* **sheerID_verified_failed.txt**: 验证失败的链接及原因。
* **2fa_codes.txt**: 生成的 2FA 验证码。

### 5. Web 管理界面

程序启动后访问：

1. 前端页面：`http://localhost:5173`
2. 后端 API：`http://localhost:8000`
3. API 文档：`http://localhost:8000/docs`

赞赏：
![赞赏](zanshang.jpg)
---

## ⚠️ 免责声明 (Disclaimer)

* 本工具仅供学习与技术交流使用，请勿用于非法用途。
* 请遵守比特浏览器及相关平台的使用条款。
* 开发者不对因使用本工具产生的任何账号损失或法律责任负责。

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

# Auto BitBrowser Management System (English)

This is a **FastAPI + Vue 3 + Playwright/BitBrowser API** automation system for batch Google-account operations: account management, window management, 2FA setup/reset, eligibility verification, age verification, and card subscription binding.

This repository is a secondary development of **https://github.com/Leclee/auto_bitbrowser**.

The system uses **BitBrowser** and controls it via the local API (default `127.0.0.1:54345`).

## Project Note

- This is just a for-fun fork of someone else's project. I don't run many accounts and I'm not a seller.
- The main goal is automated card binding and age verification.
- The project has many bugs and won't be maintained; it's just a hobby project.

## Features

- Web UI: account management, search/filter, batch import/export, real-time logs/progress.
- Browser window management: create/restore/sync/open/close.
- Task orchestration with configurable concurrency.
- 2FA automation: setup/reset and sync to browser config.
- SheerID eligibility: link extraction + verification + status detection.
- Age verification using virtual cards.
- Card binding/subscription with iframe handling.
- Multi-language handling and auto language switching.

## Installation & Usage

### Requirements

- Python 3.11+ (recommended 3.12)
- Node.js 18+
- `uv` for Python env management
- BitBrowser installed and API reachable

### Environment Setup

```bash
pip install uv
uv sync
cd web/frontend
npm install
```

### Quick Start (Web UI)

```bash
./start_web.sh
```

Access:
- Frontend: http://localhost:5173
- Backend API: http://localhost:8000
- API Docs: http://localhost:8000/docs

### Manual Start

```bash
# Backend
uv run python -m uvicorn web.backend.main:app --reload --port 8000

# Frontend
cd web/frontend
npm run dev
```

## Configuration

- `accounts.txt`: email / password / recovery email / 2FA secret (optional)
- `proxies.txt`: proxy list (optional)
- `cards.txt`: virtual cards (fallback if Web config not set)
- Web config page: SheerID API key + card info (preferred)

## Outputs

- `accounts.db`, `sheerIDlink.txt`, `已验证未绑卡.txt`, `已绑卡号.txt`, etc.

## Disclaimer

This project is for learning and experimentation only. Please comply with BitBrowser and platform policies. The author is not responsible for any account loss or legal issues.
