<p align="center">
  <img src="reverse-skill.png" alt="reverse-skill" width="140" />
</p>

<h1 align="center">reverse-skill</h1>
<h3 align="center">Reverse Engineering / Authorized Penetration Testing / Security Research Skill Router Pack</h3>

<p align="center"><em style="font-family: 'KaiTi', 'STKaiti', 'SimSun', serif; font-size: 1.3em; color: #999;">破暗而行，逆水为舟</em></p>

<p align="center">AI-powered routing + On-demand toolchain bootstrapping + Self-evolving knowledge base<br/>
逆向/渗透/安全技能路由包 — AI 自动路由 · 按需自举工具链 · 自动进化经验库</p>

<p align="center">
  <a href="https://github.com/EijunnN/reverse-skill/stargazers"><img src="https://img.shields.io/github/stars/EijunnN/reverse-skill?style=flat&logo=github" alt="stars"></a>
  <a href="https://github.com/EijunnN/reverse-skill/forks"><img src="https://img.shields.io/github/forks/EijunnN/reverse-skill?style=flat&logo=github" alt="forks"></a>
  <a href="https://github.com/EijunnN/reverse-skill/issues"><img src="https://img.shields.io/github/issues/EijunnN/reverse-skill?style=flat&logo=github" alt="issues"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-green" alt="license"></a>
</p>

<br/>

<p align="center">
  <a href="#关于项目">关于</a> ·
  <a href="#快速开始">快速开始</a> ·
  <a href="#使用说明">使用说明</a> ·
  <a href="skills/routing.md">路由矩阵</a> ·
  <a href="README_AI.md">AI 引导</a> ·
  <a href="#贡献">贡献</a>
</p>

<p align="center">
  🌐 <a href="README_EN.md">English</a>
</p>

<br/>

<a id="关于项目"></a>

## 关于项目

> **如果你是 AI Agent，请直接阅读 [README_AI.md](README_AI.md) 并严格按其中的引导执行。**

当 AI Agent（Claude Code、Codex CLI、Cursor 等）遇到 APK、二进制、前端 JS 加密、CTF 或渗透测试任务时，这套系统让它先路由到正确的方法论，再调用本机工具执行，而不是盲目猜命令。

```
用户任务 → RULES.md → Skill Router → 目标 Skill → 工具 / MCP / 脚本 → 报告 + 经验沉淀
```

**为什么需要这个项目：**

- AI Agent 面对 APK、ELF、JS、PCAP，不知道该用 jadx、Frida 还是 IDA。
- 工具路径、MCP 服务、脚本入口分散在不同机器，迁移困难。
- 同样的问题每次重新踩坑，经验无法复用。

完整路由矩阵：[skills/routing.md](skills/routing.md)

### 技术栈

<p align="left">
  <img src="https://skillicons.dev/icons?i=py,nodejs,powershell,bash,java,docker,git&theme=light" /><br/>
  <code>IDA Pro</code> · <code>radare2</code> · <code>Ghidra</code>
</p>

<a id="快速开始"></a>

## 快速开始

### 前置依赖

- **Java / JDK** — 运行 jadx、apktool
- **Node.js 22.12+** — JS 工具链和 MCP 服务
- **Python 3.x** — Frida 和辅助脚本
- **代码 AI 客户端** — Claude Code、Codex CLI、Cursor 等

### 安装

```bash
git clone https://github.com/EijunnN/reverse-skill.git
```

### 初次使用

> **初次部署只需让 AI 阅读 [README_AI.md](README_AI.md)，无需其他操作。**

各平台详细部署文档：

- **Kali Linux** → [kali/README-kali.md](kali/README-kali.md)
- **Ubuntu / Debian** → [docs/platforms/linux.md](docs/platforms/linux.md)
- **macOS** → [docs/platforms/macos.md](docs/platforms/macos.md)

<a id="使用说明"></a>

## 使用说明

### 支持场景

| 场景 | 入口 |
|------|------|
| APK / Android 逆向 | `skills/apk-reverse/` |
| 二进制逆向 (exe/dll/so/elf) | `skills/ida-reverse/` / `skills/radare2/` |
| 前端 JS 签名 / 加密参数 | `skills/js-reverse/` |
| HTTP 抓包 / 请求重放 | anything-analyzer + `skills/js-reverse/` |
| 渗透测试 / 漏洞扫描 | `skills/pentest-tools/` |
| CTF 竞赛 | `CTF-Sandbox-Orchestrator/`（40+ 子技能） |
| 固件 / IoT | `skills/firmware-pentest/` |
| 补丁差分 / N-day | `skills/patch-diff-exploit/` |
| Pwn / 漏洞利用 | `skills/pwn-chain/` |
| EDR 绕过（授权红队） | `skills/edr-bypass-re/` |
| LLM / AI 安全 | `skills/llm-security/` |
| OLLVM 脱密 | `skills/reverse-engineering/references/ollvm-deobfuscation.md` |
| 图表 / 报告 | `skills/diagram-generator/` / `skills/docs-generator/` |

### 关键文件

| 文件 | 用途 |
|------|------|
| [README_AI.md](README_AI.md) | AI Agent 配置引导（Agent 必读） |
| [RULES.md](RULES.md) | 全局运行规则 |
| [skills/SKILL.md](skills/SKILL.md) | 路由总控入口 |
| [skills/routing.md](skills/routing.md) | 路由矩阵（场景 → Skill） |
| [skills/field-journal/authorization.md](skills/field-journal/authorization.md) | 授权与交战规则记录 |
| [skills/tool-index.md](skills/tool-index.md) | 本机工具索引（自动生成） |

### 仓库结构

```
.
├── README.md               # 本文件
├── README_EN.md            # English version
├── README_AI.md            # AI Agent 配置引导
├── RULES.md                # 全局运行规则
├── RULES_zh.md             # 全局运行规则（中文）
├── skills/
│   ├── SKILL.md            # 路由总控入口
│   ├── routing.md          # 路由矩阵
│   ├── CONTRIBUTING.md     # 新增技能标准流程
│   ├── field-journal/      # 经验日志与授权记录
│   ├── scripts/            # bootstrap 与 tool-index 脚本
│   ├── apk-reverse/        # APK 逆向
│   ├── js-reverse/         # JS 逆向
│   ├── ida-reverse/        # IDA Pro 工作流
│   ├── radare2/            # radare2 分析
│   ├── reverse-engineering/ # 通用逆向方法论
│   ├── dotnet-reverse/     # .NET 逆向
│   ├── malware-analysis/   # 恶意软件分析
│   ├── mobile-reverse/     # 移动安全（Android + iOS）
│   ├── pentest-tools/      # 渗透测试工具链
│   ├── api-security/       # API 安全
│   ├── supply-chain-security/ # 供应链安全
│   ├── attack-chain/       # 攻击链编排
│   ├── pwn-chain/          # 漏洞利用链
│   ├── patch-diff-exploit/ # N-day 分析
│   ├── firmware-pentest/   # 固件 / IoT
│   ├── edr-bypass-re/      # EDR 绕过逆向
│   ├── binary-diff/        # 符号迁移
│   ├── browser-automation/ # 浏览器与桌面自动化
│   ├── diagram-generator/  # 图表生成
│   ├── docs-generator/     # 报告生成
│   └── llm-security/       # LLM / AI 安全
├── CTF-Sandbox-Orchestrator/ # CTF 子技能（40+）
├── docs/                     # 概览与架构文档
└── kali/                     # Kali 辅助脚本
```

<a id="贡献"></a>

## 贡献

欢迎贡献。新增技能请遵循 [skills/CONTRIBUTING.md](skills/CONTRIBUTING.md) 的标准流程；一般改进按常规 PR 流程：

1. Fork 项目
2. `git checkout -b feature/AmazingFeature`
3. `git commit -m 'Add some AmazingFeature'`
4. `git push origin feature/AmazingFeature`
5. 提交 Pull Request

<a id="许可证"></a>

## 许可证

本项目主体采用 **MIT License**（详见 [LICENSE](LICENSE)）。

**子模块与第三方依赖：**

- **CTF-Sandbox-Orchestrator/**：**GNU GPLv3**
- **Pentest Swarm AI**：原始项目为 **AGPL-3.0**，本仓库仅通过命令行 / MCP 调用，不包含其源代码
- 其他工具（jadx、frida、nmap、burpsuite-mcp 等）遵循各自官方许可

## 免责声明

本技能包仅面向**已授权**的安全研究与测试场景（SRC / Bug Bounty、自有资产、合约授权、CTF 靶场、公开研究）。使用者须确保其操作获得目标方授权并遵守适用法律；作者与贡献者不对任何滥用行为承担责任。

## 致谢

本项目 fork 自 [zhaoxuya520/reverse-skill](https://github.com/zhaoxuya520/reverse-skill)，感谢原作者及所有上游贡献者的工作。也感谢所有开源安全工具的作者——本仓库集成的每一个工具都是社区智慧的结晶。

## 联系方式

- **Issues**：[GitHub Issues](https://github.com/EijunnN/reverse-skill/issues)
