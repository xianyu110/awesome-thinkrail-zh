# ThinkRail 小白中文教程

> JetBrains 孵化的 **Agentic IDE** 入门指南（社区非官方教程）  
> 上游：[`JetBrains/thinkrail`](https://github.com/JetBrains/thinkrail) · 官网：[thinkrail.ai](https://thinkrail.ai/)  
> 教程校对日期：2026-10-27（Asia/Shanghai）· 对照上游稳定版 **v0.2.0**

[![GitHub stars](https://img.shields.io/github/stars/xianyu110/awesome-thinkrail-zh?style=social)](https://github.com/xianyu110/awesome-thinkrail-zh)
[![Upstream](https://img.shields.io/badge/upstream-JetBrains%2Fthinkrail-blue)](https://github.com/JetBrains/thinkrail)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Pages](https://img.shields.io/badge/GitHub%20Pages-online-brightgreen)](https://xianyu110.github.io/awesome-thinkrail-zh/)

---

## 这是什么？

**ThinkRail** 是 JetBrains 在 GitHub 上开源（Apache-2.0）的「会越用越懂你项目」的智能体 IDE：

- 底层跑的是 [`pi`](https://www.npmjs.com/package/@earendil-works/pi-coding-agent) coding agent
- 用 **git worktree** 做隔离工作区，多个 Agent 并行改代码、互不踩主工作区
- 旁边有 Monaco 编辑器、文件树、终端、Specs（`SPEC.md` 知识图）、Skills / MCP
- 可用 **桌面安装包** 或 **`thinkrail` CLI**（浏览器里打开同一套 UI）
- 内置英文 + **简体中文**界面（Settings → Language）

本仓库只提供**中文小白教程**（MIT），**不包含**上游软件本体，也不二次分发官方安装包。

---

## 官方链接（请以官方为准）

| 资源 | 链接 |
|------|------|
| 官网 | https://thinkrail.ai/ |
| 博客 | https://thinkrail.ai/blog/ |
| 源码 | https://github.com/JetBrains/thinkrail |
| Releases / 校验和 | https://github.com/JetBrains/thinkrail/releases |
| 安装脚本（macOS/Linux） | `install.sh`（仓库根目录） |
| 安装脚本（Windows） | `install.ps1`（仓库根目录） |

> 版本与资源文件名会变。安装前请打开 [Releases](https://github.com/JetBrains/thinkrail/releases) 核对最新 tag 与 `SHA256SUMS`。

---

## 环境要求（摘要）

- 需要本机已安装 **git**，且能连上你选择的模型供应商
- **macOS**：官方写明 **macOS 14+**（Apple Silicon / Intel 各有 DMG）
- **Windows**：x64；桌面版是「完整 setup ZIP → 在同目录运行 setup」
- **Linux**：x64 / ARM64；桌面依赖 glibc 2.38+、GTK3、WebKitGTK 4.1 等（Ubuntu 24.04+）
- 模型：可在应用内登录 **[JetBrains AI](https://www.jetbrains.com/ai/)**，或按 `pi` 的方式接入**你自己的供应商凭据**（ThinkRail 本身不另建账号体系）

---

## 安装：命令行 CLI（推荐新手先跑通）

### macOS / Linux（Git Bash 亦可）

```bash
curl -fsSL https://raw.githubusercontent.com/JetBrains/thinkrail/main/install.sh | bash
```

安装指定渠道 / 版本示例（来自官方 README）：

```bash
# 夜间构建
curl -fsSL https://raw.githubusercontent.com/JetBrains/thinkrail/main/install.sh | bash -s -- --channel nightly

# 钉死稳定版版本号（版本号须属于该 channel）
curl -fsSL https://raw.githubusercontent.com/JetBrains/thinkrail/main/install.sh | bash -s -- --channel stable --version 0.2.0
```

### Windows（cmd / PowerShell 同一条）

```powershell
powershell -c "irm https://raw.githubusercontent.com/JetBrains/thinkrail/main/install.ps1 | iex"
```

PowerShell 切换渠道示例：

```powershell
$env:THINKRAIL_CHANNEL='nightly'; irm https://raw.githubusercontent.com/JetBrains/thinkrail/main/install.ps1 | iex
```

### 启动

```bash
thinkrail
# 或直接打开某个 git 仓库作为项目
thinkrail ~/code/my-repo

thinkrail --version
thinkrail --help
```

状态数据默认在 `~/.thinkrail`。卸载：

```bash
thinkrail uninstall
# 需要连同本地状态一起删时再加 --remove-data；-y 跳过确认
```

更新：应用内 **Settings → Updates**，或命令行 `thinkrail update`。

---

## 安装：桌面版

到 [Releases](https://github.com/JetBrains/thinkrail/releases) 下载对应资产（名称随版本变化，以下为 v0.2.0 形态）：

| 平台 | 典型文件名 |
|------|------------|
| macOS Apple Silicon | `thinkrail-desktop-darwin-arm64.dmg` |
| macOS Intel | `thinkrail-desktop-darwin-x64.dmg` |
| Windows x64 | `thinkrail-desktop-windows-x64.zip`（解压后**就地**运行 setup） |
| Linux x64 / ARM64 | `thinkrail-desktop-linux-*.tar.gz` → 解压后运行 `installer` |

Linux 桌面依赖示例（Ubuntu 24.04+，摘自官方）：

```bash
sudo apt install libgtk-3-0 libwebkit2gtk-4.1-0 libayatana-appindicator3-1 librsvg2-2
```

> macOS：部分旧 DMG 可能未公证，Gatekeeper 会拦截——以当前 Release 说明为准。  
> Windows / macOS CLI：官方有签名说明；Linux 工件通常未签名。

---

## 第一次演示任务（约 10 分钟）

1. 准备一个**已有 git 仓库**的小项目（或 `git init` 一个玩具目录）。
2. 运行 `thinkrail ~/你的项目路径`。
3. 在应用内完成模型登录：**JetBrains AI**，或按提示配置 `pi` 已有凭据（`~/.pi/agent`）。
4. 新建一条 Chat，发一句简单任务，例如：  
   `请阅读仓库 README，用中文写一份不超过 10 行的项目摘要，不要改代码。`
5. 观察：Agent 是否只在独立 **workspace / worktree** 里干活；满意后再考虑合并分支。
6. （可选）打开 Specs 面板，看项目旁的 `SPEC.md` 图；需要工具时在 Settings → Extensions / MCP 里按需开启。

终端风格示意（非真实截图）：

```text
$ thinkrail ~/code/demo-app
ThinkRail host started · open UI in browser / desktop
▶ workspace: demo-app / feature-summary (worktree)
▶ chat: "请阅读 README，用中文写 10 行摘要，不要改代码。"
✓ agent finished · review Changes · merge when ready
```

---

## 常见报错与处理

| 现象 | 可能原因 | 处理 |
|------|----------|------|
| `thinkrail: command not found` | PATH 未刷新 | 重开终端；或看安装脚本提示的 prefix；Windows 注意安装器是否改 PATH |
| 提示需要 git | PATH 上没有 git | 安装 Git for Windows / Xcode CLT / 发行版 git |
| 无法发起对话 / 模型错误 | 未登录供应商 | JetBrains AI 登录，或配置 `pi` 已有 provider；检查 Settings → Environment |
| macOS「无法打开，因为无法验证开发者」 | Gatekeeper / 未公证包 | 以官方当前签名说明为准；系统设置里按提示放行，或改用 CLI 安装脚本 |
| Windows SmartScreen | 新发布包信誉不足 | 核对 Release 的 `SHA256SUMS` 后再运行 |
| Linux 桌面打不开 | 缺 WebKit/GTK 或 glibc 过旧 | 按官方依赖安装；优先 Ubuntu 24.04+ |
| 想关掉额外分析 | 隐私偏好 | 首次启动可关「Share additional usage data」；CLI：`thinkrail --no-analytics` 或 `THINKRAIL_NO_ANALYTICS=1` |

官方还说明：用法分析**不含**提示词、代码、凭据；错误上报也不含路径与项目内容。细节见上游 README「Analytics & privacy」。

---

## 模型 / API 合规说明（国内读者）

- ThinkRail **不内置**某一家「必须用」的境外中转；它吃的是 **JetBrains AI** 或你在 `pi` 里配置的供应商。
- 本教程**不硬推**未备案的境外模型中转。若你需要 OpenAI 兼容接口，可优先考虑：
  - 本地：**Ollama** / LM Studio 等本地端（若你的 `pi` 配置支持对应协议）
  - 国内可用的兼容端 / DeepSeek 等（以你账号与当地法规为准）
  - 若使用聚合 API，可自行评估 [TryAllAPI](https://tryallapi.com/register?aff=5A6A) 等 OpenAI 兼容入口（轻提示，非必须）
- 相关工具与周边若需要选购，店铺入口：[https://wzyp.cn/shop/maynorai](https://wzyp.cn/shop/maynorai)（可选）。

本教程不编造菜单名、套餐价；一切以官方界面与供应商控制台为准。

---

## 从源码开发（进阶，可选）

官方要求：**Bun 1.4.4**、**Node.js ≥ 22.19**，并已配置可用的 `pi` provider：

```bash
git clone https://github.com/JetBrains/thinkrail.git
cd thinkrail
bun install
bun run dev
```

详见上游 [`CONTRIBUTING.md`](https://github.com/JetBrains/thinkrail/blob/main/CONTRIBUTING.md)。

---

## 许可证说明

| 内容 | 协议 |
|------|------|
| **本教程文案**（本仓库 README / docs） | [MIT](LICENSE) |
| **ThinkRail 上游软件** | [Apache License 2.0](https://github.com/JetBrains/thinkrail/blob/main/LICENSE) |

本仓库与 JetBrains / ThinkRail **无官方隶属关系**，仅为社区中文导读。

---

## 维护

- 作者：Maynor（[@xianyu110](https://github.com/xianyu110) / Maynor996）
- 发现问题：请优先查 [上游 Issues](https://github.com/JetBrains/thinkrail/issues)；教程笔误可开本仓 Issue
- 在线页：https://xianyu110.github.io/awesome-thinkrail-zh/
