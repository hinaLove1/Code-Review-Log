# Code Review Log (Gemini)

## 私密仓库code-re的介绍

这是一个基于 Gemini 的自动化代码评审工具，设计用于集成到 GitHub Actions 中，自动分析 Git Diff 并提供架构师级别的代码评审建议。

## 🚀 功能特性

- **AI 代码评审**：利用 Google Gemini (2.5-flash/pro) 模型，模拟高级架构师对代码变更进行深度评审。
- **自动化集成**：完美适配 GitHub Actions，支持在 `push` 或 `pull_request` 时自动触发。
- **评审日志持久化**：自动将评审建议生成 Markdown 文件，并推送到指定的远程 GitHub 仓库，方便长期追踪。
- **实时消息通知**：评审完成后，通过微信公众号模板消息实时通知相关人员。
- **环境隔离**：所有敏感信息（API Key、Tokens）均通过环境变量/Secrets 传递，代码无硬编码。

## 🛠️ 项目结构

- `code-review-sdk`: 核心逻辑模块，包含 Git 操作、AI 调用、微信通知及业务编排。
- `code-review-test`: 示例测试模块，用于验证 SDK 功能。

## ⚙️ 配置要求

在 GitHub 仓库的 **Settings -> Secrets and variables -> Actions** 中配置以下 Secrets：

### 1. GitHub 相关
| Secret 名称 | 说明 | 示例 |
| :--- | :--- | :--- |
| `CODE_TOKEN` | 具有读写权限的 GitHub PAT (Personal Access Token) | `ghp_xxxxxx` |
| `CODE_REVIEW_LOG_URI` | 存储评审日志的仓库地址 (不含 .git 后缀) | `https://github.com/user/code-review-log` |

### 2. AI 相关
| Secret 名称 | 说明 |
| :--- | :--- |
| `API_KEY` | Google Gemini API Key |

### 3. 微信通知相关 (可选)
| Secret 名称 | 说明 |
| :--- | :--- |
| `WEIXIN_APPID` | 微信测试号/公众号 AppID |
| `WEIXIN_SECRET` | 微信测试号/公众号 AppSecret |
| `WEIXIN_TOUSER` | 接收通知的用户 OpenID |
| `WEIXIN_TEMPLATE_ID` | 微信模板消息 ID |

## 📖 使用指南

### GitHub Actions 集成

项目中提供了两种 workflow 配置：

1. **本地构建模式 (`main-maven-jar.yml`)**：
   - 每次运行时现场编译并打包 SDK。
   - 适用于 SDK 本身正在频繁迭代的情况。

2. **远程下载模式 (`main-remote-jar.yml`)**：
   - 从指定的 Release 地址下载预编译好的 `code-review-sdk-1.0.jar` 运行。
   - 运行速度快，适合作为通用工具集成到其他项目中。

### 本地开发调试

1. 确保安装了 Java 1.8+ 和 Maven。
2. 在本地环境变量中配置上述 Secrets 对应的变量。
3. 执行编译安装：
   ```bash
   mvn clean install
   ```
4. 运行 SDK：
   ```bash
   java -jar code-review-sdk/target/code-review-sdk-1.0.jar
   ```

## 📝 评审示例

评审结果将以 Markdown 格式保存，内容包含：
- 变更项目与分支信息。
- 提交者及提交消息。
- AI 给出的具体优化建议和潜在风险提示。

---
*本项目仅供学习与技术研究使用。*
