# Rainyun-Qiandao-V2 (Selenium)

![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/chizw/Rainyun-Qiandao/rainyun-sign.yml?style=flat-square)
![Python Version](https://img.shields.io/badge/python-3.9+-blue.svg?style=flat-square)
![License](https://img.shields.io/badge/license-GPL%20v3.0-green.svg?style=flat-square)
![GitHub Stars](https://img.shields.io/github/stars/chizw/Rainyun-Qiandao?style=flat-square)
![GitHub Forks](https://img.shields.io/github/stars/chizw/Rainyun-Qiandao?style=flat-square)

## Star 历史

[![Star History Chart](https://api.star-history.com/svg?repos=chizw/Rainyun-Qiandao&type=Date)](https://star-history.com/#chizw/Rainyun-Qiandao&Date)

## 项目概述

Rainyun-Qiandao-V2 是一个基于 Selenium 和 ICR（Image Captcha Recognition）的雨云自动签到工具，通过模拟浏览器操作和高级验证码识别，实现雨云账户的自动每日签到以赚取积分。

## 功能特性

![Features](https://img.shields.io/badge/features-18+-orange.svg?style=flat-square)

### 核心功能
- ✅ 自动完成雨云账户登录
- ✅ 使用 ICR 模块进行验证码自动识别（旋转分析+模板匹配）
- ✅ 支持自定义随机延时（5-20秒），避免被系统识别为自动化脚本
- ✅ 支持在本地环境和 GitHub Actions 中运行
- ✅ 集成 webdriver-manager 自动匹配 ChromeDriver
- ✅ 详细的日志记录，便于排查问题

### 多账户支持
- ✅ 支持多账户签到，每个账户独立运行，互不干扰
- ✅ 支持并发处理，可配置最大并发线程数
- ✅ 批量重试机制，等待所有账户完成后统一重试失败账户
- ✅ 账户处理间隔 5-15 秒随机延时

### 浏览器指纹
- ✅ 随机浏览器指纹（User-Agent、分辨率、语言、时区）
- ✅ 每个账户使用独立的随机指纹

### Cookie 缓存
- ✅ Cookie 缓存功能，支持免密登录
- ✅ GitHub Actions 缓存支持，持久化 Cookie

### 通知推送
- ✅ 支持统一通知，汇总所有账户签到结果
- ✅ 支持7种通知推送方式（Push+、SMTP、Bark、钉钉、飞书、Telegram、Server酱）

## 技术栈

![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=flat-square&logo=python&logoColor=white)
![Selenium](https://img.shields.io/badge/Selenium-4.27+-43B02A?style=flat-square&logo=selenium&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-4.9+-5C3EE8?style=flat-square&logo=opencv&logoColor=white)
![Chrome](https://img.shields.io/badge/Chrome-Latest-4285F4?style=flat-square&logo=google-chrome&logoColor=white)

- Python 3.9+
- Selenium WebDriver 4.27+
- ICR 验证码识别模块（旋转分析+模板匹配）
- OpenCV 图像处理
- Google Chrome 浏览器

## 安装步骤

### 1. 环境要求

![Requirements](https://img.shields.io/badge/requirements-Python%203.9%2B%20Chrome-blue.svg?style=flat-square)

- Python 3.9 或更高版本
- Google Chrome 浏览器

### 2. 克隆项目

```bash
git clone https://github.com/chizw/Rainyun-Qiandao.git
cd Rainyun-Qiandao
```

### 3. 安装依赖

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 4. 配置 ChromeDriver

本项目已集成自动匹配 ChromeDriver 的功能，无需手动下载和配置。工具将按以下顺序尝试：

1. 使用 webdriver-manager 自动安装匹配的 ChromeDriver
2. 使用系统路径中的 ChromeDriver
3. 尝试常见的备用路径

## 使用方法

### 本地运行

#### 方法一：通过环境变量配置

##### 单账户配置

```bash
# Windows (PowerShell)
$env:RAINYUN_USER = "your_username"
$env:RAINYUN_PASS = "your_password"

# Linux/macOS
export RAINYUN_USER="your_username"
export RAINYUN_PASS="your_password"

# 运行脚本
python rainyun.py
```

##### 多账户配置

支持多行格式，每行一个用户名/密码，数量需匹配：

```bash
# Windows (PowerShell)
$env:RAINYUN_USER = "user1`nuser2`nuser3"
$env:RAINYUN_PASS = "pass1`npass2`npass3"

# Linux/macOS
export RAINYUN_USER="user1\nuser2\nuser3"
export RAINYUN_PASS="pass1\npass2\npass3"

# 运行脚本
python rainyun.py
```

#### 方法二：通过 .env 文件配置（推荐）

创建 `.env` 文件（已添加到 .gitignore）：

```env
RAINYUN_USER=your_username
RAINYUN_PASS=your_password
DEBUG=false
HEADLESS=false
MAX_WORKERS=2
MAX_RETRIES=1
```

多账户配置：

```env
RAINYUN_USER=user1
user2
user3
RAINYUN_PASS=pass1
pass2
pass3
DEBUG=false
HEADLESS=false
MAX_WORKERS=2
MAX_RETRIES=1
```

### 使用 GitHub Actions 自动签到

![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-Automated-2088FF?style=flat-square&logo=github-actions&logoColor=white)

1. Fork 本仓库
2. 进入仓库的 `Settings` > `Secrets and variables` > `Actions`
3. 添加以下密钥：

| Secret名称 | 说明 | 必需 |
|-----------|------|------|
| RAINYUN_USER | 雨云用户名（支持多行，每行一个用户名） | ✅ |
| RAINYUN_PASS | 雨云密码（支持多行，每行一个密码，需与用户名数量匹配） | ✅ |

4. 可选的通知推送配置：

| Secret名称 | 说明 | 推送方式 |
|-----------|------|----------|
| PUSH_PLUS_TOKEN | Push+用户令牌 | 微信 |
| PUSH_PLUS_USER | Push+群组编码（可选） | 微信 |
| SMTP_SERVER | SMTP服务器地址 | 邮件 |
| SMTP_SSL | 是否使用SSL (true/false) | 邮件 |
| SMTP_EMAIL | 邮箱地址 | 邮件 |
| SMTP_PASSWORD | 邮箱密码 | 邮件 |
| SMTP_NAME | 发件人姓名 | 邮件 |
| BARK_PUSH | Bark设备码或完整URL | iOS |
| DD_BOT_SECRET | 钉钉机器人密钥 | 钉钉 |
| DD_BOT_TOKEN | 钉钉机器人令牌 | 钉钉 |
| FSKEY | 飞书Webhook密钥 | 飞书 |
| TG_BOT_TOKEN | Telegram机器人令牌 | Telegram |
| TG_USER_ID | Telegram用户ID | Telegram |
| PUSH_KEY | Server酱密钥 | Server酱 |

5. 工作流将每天 UTC 4 点（UTC+8 12点）自动运行，也可以手动触发

## 配置说明

### 环境变量

| 变量名 | 说明 | 默认值 | 必需 |
|--------|------|--------|------|
| RAINYUN_USER | 雨云用户名（支持多行，每行一个用户名） | - | ✅ |
| RAINYUN_PASS | 雨云密码（支持多行，每行一个密码，需与用户名数量匹配） | - | ✅ |
| HEADLESS | 是否以无头模式运行（true/false） | false | ❌ |
| DEBUG | 是否启用调试模式（true/false） | false | ❌ |
| MAX_WORKERS | 最大并发线程数 | 2 | ❌ |
| MAX_RETRIES | 最大重试次数 | 1 | ❌ |
| GITHUB_ACTIONS | 在 GitHub Actions 环境中自动设置为 true，用于强制无头模式 | false | ❌ |
| NOTIFY_ONLY_FAILURE | 仅在有失败账号时推送通知（true/false） | false | ❌ |

### 关键设置

- 随机延时设置为 5-20 秒，可在代码中调整
- 超时时间设置为 15 秒，可在代码中修改
- 验证码识别使用 ICR 模块，支持旋转分析和模板匹配

## Cookie 缓存功能

### 本地缓存

- Cookie 保存在 `cookies/` 目录
- 每个账户使用独立的 Cookie 文件（基于用户名哈希）
- 支持 7 天免登录

### GitHub Actions 缓存

- 使用 `actions/cache` 持久化 Cookie
- 缓存键基于用户名生成
- 每次运行自动恢复和保存 Cookie

## 并发与重试机制

### 并发处理

```
开始并发处理 N 个账户...
    ↓
[Worker-1] 处理账户 1
[Worker-2] 处理账户 2  (并发执行)
    ↓
收集失败账户
    ↓
等待 5-15 秒
    ↓
第 1 轮重试失败账户
    ↓
...
```

### 重试策略

- 等待所有账户完成后统一重试
- 每轮重试间隔 5-15 秒随机时间
- 可配置最大重试次数

## 常见问题

### 1. Linux 系统怎么使用？

参考 [Linux 环境配置指南](https://github.com/SerendipityR-2022/Rainyun-Qiandao/issues/1#issuecomment-3096198779)。

### 2. 找不到元素或等待超时，报错 `NoSuchElementException`/`TimeoutException`

- 网页加载缓慢，尝试延长超时等待时间
- 更换连接性更好的网络环境
- 确认 Chrome 浏览器版本与系统兼容

### 3. 验证码识别失败

ICR 模块使用旋转分析和模板匹配算法，识别率较高。脚本会自动重试，多次尝试后通常能成功通过验证。

### 4. 依赖安装失败

确保 Python 版本为 3.9+，使用以下命令安装依赖：

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

如果遇到依赖冲突，可以尝试：

```bash
pip install --upgrade pip setuptools wheel
pip install -r requirements.txt
```

### 5. GitHub Actions 中 Chrome 初始化失败

- 确保 Chrome 和 ChromeDriver 版本匹配
- 检查 GitHub Actions 日志中的错误信息
- 项目已优化 Chrome 选项配置，支持 headless 模式

### 6. 多账户并发时出现端口冲突

项目已实现线程锁机制，确保 Chrome 实例按顺序初始化，避免端口冲突。

## GitHub Actions 优化

项目已集成 GitHub Actions 缓存功能，以加快每次运行的速度，主要缓存：

- Python 依赖
- Chrome 浏览器
- Cookie 缓存

## 版本历史

### v2.5 (2026-02-23)

- ✨ 新增随机浏览器指纹功能
- ✨ 新增 Cookie 缓存功能，支持免密登录
- ✨ 新增并发处理支持，可配置最大线程数
- ✨ 新增批量重试机制，等待所有账户完成后统一重试
- ✨ 新增页面加载超时处理和重试机制
- 🐛 修复 GitHub Actions 中 Chrome 初始化失败问题
- 🐛 修复多账户并发时的端口冲突问题
- 🐛 修复登录流程检测问题
- 📝 更新 GitHub Actions 工作流配置

### v2.4 (2026-02-12)

- ✨ 新增云端版本配置

### v2.3 (2026-02-11)

- ✨ 新增 ICR 验证码识别模块（旋转分析+模板匹配）
- ✨ 支持多账户签到
- ✨ 集成 7 种通知推送方式
- 🐛 修复 wait 变量作用域问题
- 🐛 修复 ChromeDriver 执行格式错误
- 🐛 修复依赖冲突问题
- 📝 更新 GitHub Actions 配置

## 贡献指南

欢迎提交 Issue 和 Pull Request 来改进本项目：

1. Fork 本仓库
2. 创建您的特性分支 (`git checkout -b feature/amazing-feature`)
3. 提交您的更改 (`git commit -m 'Add some amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 开启一个 Pull Request

## 许可证

![License](https://img.shields.io/badge/license-GPL%20v3.0-green.svg?style=flat-square)

本项目采用 GNU GENERAL PUBLIC LICENSE 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情。

## 免责声明

- 本工具仅用于学习和个人使用
- 使用本工具应遵守雨云官方的用户协议和相关规定
- 作者不对因使用本工具可能产生的任何后果负责

## 工具依赖

- [Selenium](https://www.selenium.dev/) - 自动化测试工具
- [webdriver-manager](https://github.com/SergeyPirogov/webdriver_manager) - ChromeDriver 自动管理工具
- [OpenCV](https://opencv.org/) - 开源计算机视觉库

## 鸣谢

- [SerendipityR-2022](https://github.com/SerendipityR-2022) - 项目初始版本
- [LeapYa](https://github.com/LeapYa/Rainyun-Qiandao) - 多账号并发签到、浏览器指纹随机化注入、Cookie 持久化与失效检测、Docker ChromeDriver 适配逻辑

---

![GitHub last commit](https://img.shields.io/github/last-commit/chizw/Rainyun-Qiandao?style=flat-square)
![GitHub issues](https://img.shields.io/github/issues/chizw/Rainyun-Qiandao?style=flat-square)
![GitHub closed issues](https://img.shields.io/github/issues-closed/chizw/Rainyun-Qiandao?style=flat-square)
