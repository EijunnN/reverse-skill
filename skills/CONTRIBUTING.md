# 新增 Skill 指南

本文档定义向本包新增技能模块的标准流程。无论是人工新增，还是 AI 在任务中发现需要新增，都按此流程执行。

---

## 1. 什么时候该新增 skill

满足以下任一条件时，新增独立技能而不是塞进现有模块：

- 目标类型明确不同（如"固件逆向"、"内核分析"、"协议逆向"）。
- 工具链独立（如 Ghidra headless、Burp Suite、sqlmap）。
- 工作流有独立的阶段与产物（不是现有技能的子步骤）。
- 路由矩阵中找不到合适的现有入口。

如果只是对现有技能的补充（例如给 APK 逆向加一个新脚本），直接在对应目录下扩展即可，无需新建。

## 2. 质量标准（所有新 skill 必须满足）

1. **Frontmatter 完备**：`SKILL.md` 以 YAML frontmatter 开头，包含 `name` 与 `description`。`description` 是路由触发面，必须写清"做什么 + 何时使用"，并包含中英双语触发词。
2. **结构统一**：遵循 §3 的章节骨架；正文控制在 500 行以内，超出部分拆入 `references/` 并从 `SKILL.md` 明确引用（渐进式披露）。
3. **语气专业**：祈使句、客观、精确；RFC 2119 关键词（MUST / SHOULD / MAY）仅用于真正影响成败的约束，不滥用全大写制造紧迫感。
4. **语言一致**：单个文件以一种语言为主，不中英混排；用户可见的标签遵循 §3.3 的双语契约。
5. **不猜路径**：工具路径一律来自 `tool-index.md`；缺工具的唯一动作是 bootstrap。
6. **路由诚实**：路由未命中时提议新增技能，不强行归入不匹配模块。

## 3. SKILL.md 标准骨架

```text
skills/
└── <new-skill-name>/
    ├── SKILL.md              # 必须：技能入口文档
    ├── scripts/              # 可选：自动化脚本
    │   └── <workflow>.ps1
    └── references/           # 可选：参考资料、速查表
        └── <topic>.md
```

命名规范：目录名用小写英文 + 连字符（如 `firmware-reverse`、`kernel-analysis`）；不用中文目录名，不用下划线。

每个 `SKILL.md` 必须包含：

```markdown
---
name: <skill-name>
description: <一句话说明适用场景与触发条件，含中英触发词>
license: MIT
---

# <Skill 标题>

## 操作协议
1. 确认当前任务命中本技能的适用范围。
2. 阅读 `../tool-index.md`，校验工具可用性与实际路径。
3. 缺工具时调用 bootstrap，不猜路径。
4. 进入"工作流"第一步并执行。

## 适用范围
<!-- 什么任务应该路由到这里；什么任务应该路由到别处 -->

## 工具依赖
| 工具 | 是否必需 | 用途 | 可自动安装 |
|------|---------|------|-----------|

## 工作流
<!-- 标准执行步骤；每个阶段末尾嵌入"下一步菜单"（§3.4） -->

## 按需自举
<!-- 自动化能力边界、自举触发点、失败时的手动指引 -->

## 路由上下文
**上游入口**: ...
**下游出口**: ...
**同级关联模块**: ...

## 完成前自检
- [ ] 工作流中的每一步都已执行（而非只阅读）
- [ ] 工具路径来自 `tool-index.md`
- [ ] 产出了可复现证据（命令 / 脚本 / 截图 / 报告）
- [ ] RULES 要求的完成清单已回写
```

### 3.1 渐进式披露

- `SKILL.md` 只保留核心工作流与选择指引；具体技术细节、模式库、示例放入 `references/`，并在 `SKILL.md` 中说明何时阅读哪个文件。
- 引用层级保持一层：`SKILL.md` 直接链接所有 reference 文件，不做深层嵌套。
- 超过 100 行的 reference 文件在顶部加目录。

### 3.2 授权与先例

每个安全类新技能的"操作协议"中应包含对授权记录的引用：

```markdown
1. 阅读 `../field-journal/authorization.md`，确认任务落入已记录的授权范围。
```

逆向类技能可引用 `../field-journal/precedent-reverse.md`，渗透类可引用 `../field-journal/precedent-pentest.md`，用于复用成熟做法。

### 3.3 语言行为契约

```markdown
## 语言行为契约

- **内部推理 / 工具选择 / 阶段控制**：使用 English。
- **用户可见消息 / 章节标签 / 报告 / 下一步菜单**：使用中文（除非用户要求其他语言）。
- **默认双语标签格式**：中文在前，英文在后，以 ` / ` 分隔。
```

常用双语标签：

| 中文 | English |
|------|---------|
| 当前阶段 | Current phase |
| 已验证事实 | Verified facts |
| 关键证据 | Key evidence |
| 推断与置信度 | Inference and confidence |
| 风险/漏洞候选 | Risk or vulnerability candidates |
| 建议下一步 | Suggested next steps |

### 3.4 下一步菜单模式

工作流的每个阶段结束时，提供 3–6 个编号的下一步选项，由用户选择方向后再跨阶段推进：

- 每个选项以数字编号，描述一项具体可执行动作。
- 至少包含一个"导出报告 / 写文档"选项。
- 至少包含一个"继续深入"或"换方法"选项。
- 必要时包含一个"暂停 / 提问"出口。
- 选项描述是面向用户的中文短语，不是内部指令。

```markdown
## 建议下一步（选一个编号）

1. 对 [关键函数] 做深度反编译，还原核心算法
2. 用 Frida 动态 Hook 验证 [参数猜想]
3. 导出当前分析结果，生成阶段性报告
4. 换 [备选工具] 做交叉验证
5. 暂停，先确认前面的证据
```

---

## 4. 接入 bootstrap 系统

### 4.1 在 `bootstrap-manifest.json` 中注册能力

打开 `scripts/bootstrap-manifest.json`，在 `capabilities` 数组中添加条目：

```json
{
  "name": "<tool-name>",
  "bootstrapKind": "<kind>",
  "canAutoInstall": true,
  "verifyCommand": "<tool-name>"
}
```

支持的 `bootstrapKind`：

| Kind | 适用场景 | 必填字段 |
|------|---------|---------|
| `github-release-zip` | GitHub Release 下载解压 | `repo`, `assetRegex`, `installDir` |
| `github-release-jar-wrapper` | Java JAR + bat wrapper | `repo`, `assetRegex`, `installDir`, `wrapperName` |
| `pip-package` | Python pip 安装 | `pipPackage` |
| `npm-mcp` | npx 启动的 MCP server | `npmPackage`, `mcpNames`, `mcpCommand`, `mcpArgs` |
| `local-http-mcp` | 本地 HTTP 服务型 MCP | `mcpUrl`, `servicePort` |
| `winget-package` | Windows winget 安装 | `wingetId` |

### 4.2 在 `ToolDiscovery.ps1` 中注册工具

打开 `scripts/lib/ToolDiscovery.ps1`，在 `Get-ReverseToolCatalog` 函数中添加条目：

```powershell
[pscustomobject]@{
    Name = '<tool-name>'
    Skill = '<new-skill-name>'
    Purpose = '<中文用途说明>'
    VersionArgs = @('--version')
    Fallbacks = @(
        [pscustomobject]@{ Type = 'command'; Value = '<tool-name>' },
        [pscustomobject]@{ Type = 'path'; Value = (Join-Path $env:USERPROFILE 'Tools\<tool>\<executable>') }
    )
}
```

### 4.3 在 `refresh-tool-index.ps1` 中注册脚本引用

打开 `skills/scripts/refresh-tool-index.ps1`，在 `$scriptRefs` 哈希表中添加：

```powershell
'<tool-name>' = @('<new-skill-name>/scripts/<workflow>.ps1')
```

### 4.4 在入口脚本中接入 bootstrap

脚本中检测工具缺失时，调用 bootstrap 而不是直接 throw：

```powershell
$bootstrapScript = Join-Path $PSScriptRoot '..\..\scripts\bootstrap-reverse.ps1'

$spec = Resolve-ReverseToolSpec -Name '<tool-name>'
if (-not $spec.Available) {
    Write-Host 'INFO: <tool> not found, attempting auto-bootstrap...' -ForegroundColor Yellow
    & powershell.exe -NoProfile -ExecutionPolicy Bypass -File $bootstrapScript -Capability @('<tool-name>') -SkipRefresh
    $spec = Resolve-ReverseToolSpec -Name '<tool-name>'
    if (-not $spec.Available) {
        throw '<tool> still not available after bootstrap. Install manually: <url>'
    }
}
```

---

## 5. 接入路由系统

### 5.1 更新路由矩阵

打开 `routing.md`（及其中文镜像 `routing_zh.md`），在对应表格中添加新行：

- "按目标类型"表：新的目标类型 → 推荐入口
- "按用户意图"表：用户可能说的话 → 对应技能
- "按工具链"表：新工具 → 对应模块

### 5.2 更新根 SKILL.md

打开 `skills/SKILL.md`，在"模块目录"表格中添加新行。

### 5.3 更新触发关键词

- `RULES.md` / `RULES_zh.md` 的触发关键词列表。
- 使用 Kiro 时：`.kiro/steering/reverse-routing.md`。

---

## 6. 刷新索引

完成上述步骤后运行：

**Windows**：
```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File "<SKILL_ROOT>\skills\scripts\refresh-tool-index.ps1"
```

**Kali Linux**：
```bash
bash "<SKILL_ROOT>/kali/scripts/refresh-tool-index.sh"
```

确认新工具出现在 `tool-index.md` 和 `tool-index.json` 中。

---

## 7. Kali 平台同步（如项目包含 `kali/`）

### 7.1 在 Kali manifest 中注册

打开 `kali/scripts/bootstrap-manifest.json`，添加对应条目（`bootstrapKind` 通常为 `apt-package` 或 `pip-package`）。

### 7.2 在 Kali tool-discovery.sh 中注册

打开 `kali/scripts/lib/tool-discovery.sh`，在 `TOOL_CATALOG` 数组中添加：

```bash
"<tool-name>|<skill-name>|<中文用途>|<version-args>|<fallback-commands>"
```

在 `SCRIPT_REFS` 中添加：

```bash
["<tool-name>"]="<skill-name>/SKILL.md"
```

### 7.3 在 Kali bootstrap 脚本中添加安装逻辑

打开 `kali/scripts/bootstrap-reverse.sh`，在 `ensure_capability()` 的 `case` 中添加新工具的安装逻辑。

### 7.4 更新 Kali RULES 触发关键词

打开 `kali/RULES-kali.md`，在触发关键词列表中添加新技能相关的词。

---

## 8. 验证清单

**通用（必须）**：

- [ ] `<new-skill>/SKILL.md` 存在且包含所有必需章节（含 frontmatter）
- [ ] 路由矩阵（`routing.md` 与 `routing_zh.md`）已更新
- [ ] 根 `SKILL.md` 的模块表已更新
- [ ] `RULES.md` / `RULES_zh.md` 触发关键词已更新
- [ ] `.kiro/steering/reverse-routing.md` 已更新（如使用 Kiro）

**Windows 平台**：

- [ ] `scripts/bootstrap-manifest.json` 已注册新工具
- [ ] `scripts/lib/ToolDiscovery.ps1` 已注册新工具（含 fallback path）
- [ ] `skills/scripts/refresh-tool-index.ps1` 的 `$scriptRefs` 已更新

**Kali 平台（如有 `kali/`）**：

- [ ] `kali/scripts/bootstrap-manifest.json` 已注册新工具
- [ ] `kali/scripts/lib/tool-discovery.sh` 的 `TOOL_CATALOG` 和 `SCRIPT_REFS` 已更新
- [ ] `kali/scripts/bootstrap-reverse.sh` 的 `ensure_capability()` 已添加安装逻辑
- [ ] `kali/RULES-kali.md` 触发关键词已更新

**收尾**：

- [ ] 入口脚本已接入 bootstrap（缺工具时自动补齐）
- [ ] 运行 refresh-tool-index 后新工具出现在索引中

---

## 9. 示例：新增 "Ghidra Headless" skill

### 目录

```text
skills/ghidra-headless/
├── SKILL.md
├── scripts/
│   └── analyze.ps1
└── references/
    └── scripting-cheatsheet.md
```

### bootstrap-manifest.json 新增

```json
{
  "name": "ghidra",
  "bootstrapKind": "github-release-zip",
  "repo": "NationalSecurityAgency/ghidra",
  "assetRegex": "^ghidra_.*_PUBLIC_.*\\.zip$",
  "installDir": "%USERPROFILE%\\Tools\\ghidra",
  "docsUrl": "https://ghidra-sre.org/",
  "canAutoInstall": true,
  "verifyCommand": "analyzeHeadless"
}
```

### ToolDiscovery.ps1 新增

```powershell
[pscustomobject]@{
    Name = 'analyzeHeadless'
    Skill = 'ghidra-headless'
    Purpose = 'Ghidra 无头分析'
    VersionArgs = @()
    Fallbacks = @(
        [pscustomobject]@{ Type = 'command'; Value = 'analyzeHeadless' },
        [pscustomobject]@{ Type = 'path'; Value = (Join-Path $env:USERPROFILE 'Tools\ghidra\support\analyzeHeadless.bat') }
    )
}
```

### 路由矩阵新增

```markdown
| 二进制 (无 IDA) | `ghidra-headless/` — Ghidra 无头反编译 | `radare2/` — CLI 侦察 |
```

---

## 10. 新增带 MCP 服务的 Skill

### 10.1 确定 MCP 类型

| 类型 | 特征 | 示例 | `bootstrapKind` |
|------|------|------|-----------------|
| npx 启动型 | `npx -y @xxx/yyy` 拉起，无需本地项目 | jshookmcp | `npm-mcp` |
| 本地 HTTP 服务型 | 需 clone 项目、安装依赖、启动 dev server | anything-analyzer | `local-http-mcp` |
| pip 安装 + HTTP 型 | pip 安装后启动 HTTP 服务 | idalib-mcp | `pip-package` + 单独的 `local-http-mcp` 条目 |
| Docker 型 | `docker run` 启动 | 未来可能的 MCP | `docker-mcp`（需扩展 bootstrap 脚本） |
| 远程托管型 | 直连远程 URL，无需本地安装 | 云端 MCP 服务 | 无需 bootstrap，只需注册 URL |

### 10.2 在 bootstrap-manifest.json 中注册

**npx 启动型**：

```json
{
  "name": "<mcp-name>",
  "bootstrapKind": "npm-mcp",
  "npmPackage": "@scope/package@latest",
  "mcpNames": ["<mcp-server-name-in-config>"],
  "mcpCommand": "npx",
  "mcpArgs": ["-y", "@scope/package@latest"],
  "mcpEnv": { "ENV_VAR": "value" },
  "docsUrl": "https://github.com/...",
  "canAutoInstall": true,
  "verifyCommand": "npx"
}
```

**本地 HTTP 服务型**：

```json
{
  "name": "<mcp-name>",
  "bootstrapKind": "local-http-mcp",
  "repoUrl": "https://github.com/xxx/yyy",
  "installDir": "%USERPROFILE%\\Tools\\<project-name>",
  "startupDirCandidates": ["%USERPROFILE%\\Tools\\<project-name>"],
  "startCommand": "pnpm",
  "startArgs": ["dev"],
  "mcpNames": ["<mcp-server-name>"],
  "mcpUrl": "http://localhost:<port>/mcp",
  "servicePort": <port>,
  "docsUrl": "https://github.com/xxx/yyy",
  "canAutoInstall": true,
  "verificationMode": "service-or-registration"
}
```

**pip + HTTP 服务型**（两个条目：pip 安装 + 服务注册）：

```json
{
  "name": "<tool-name>",
  "bootstrapKind": "pip-package",
  "pipPackage": "<package-name>",
  "canAutoInstall": true,
  "verifyCommand": "<executable>"
},
{
  "name": "<service-name>",
  "bootstrapKind": "local-http-mcp",
  "dependsOn": ["<tool-name>"],
  "mcpNames": ["<mcp-server-name>"],
  "mcpUrl": "http://127.0.0.1:<port>/mcp",
  "servicePort": <port>,
  "startScript": "%SKILL_ROOT%\\<skill-dir>\\scripts\\start.ps1",
  "canAutoInstall": true,
  "verificationMode": "service-and-registration"
}
```

### 10.3 MCP 注册逻辑

bootstrap 脚本内置通用的 MCP 配置合并能力：读取客户端配置（如 `~/.claude/mcp.json`）、合并不覆盖、保存。标准类型只需在 manifest 中声明。

需要 auth token 或自定义 header 时：

```json
{
  "mcpHeaders": {
    "Authorization": "Bearer <PLACEHOLDER_TOKEN>"
  }
}
```

bootstrap 会把 headers 写入配置；用户随后将 `<PLACEHOLDER_TOKEN>` 替换为真实值。

### 10.4 启动脚本（本地服务型）

```powershell
# <skill-name>/scripts/start.ps1
param(
    [int]$Port = <default-port>
)

$ErrorActionPreference = 'Stop'
. (Join-Path $PSScriptRoot '..\..\scripts\lib\ToolDiscovery.ps1')

if (Test-ReverseTcpPort -Port $Port) {
    Write-Output "OK:already-running:$Port"
    return
}

$projectDir = "<找到项目的逻辑>"
Start-Process -FilePath "<启动命令>" -ArgumentList @("<参数>") -WorkingDirectory $projectDir -WindowStyle Hidden

$deadline = (Get-Date).AddSeconds(60)
while ((Get-Date) -lt $deadline) {
    if (Test-ReverseTcpPort -Port $Port) {
        Write-Output "OK:started:$Port"
        return
    }
    Start-Sleep -Seconds 2
}

Write-Output "ERR:timeout:$Port"
```

### 10.5 失败引导

在 `SKILL.md` 中包含"MCP 服务不可用时的手动配置指引"：

```markdown
### MCP 服务手动配置

1. [安装前置依赖]
2. [获取项目/安装包]
3. [启动服务]
4. [验证端口可达]
5. [在 AI 客户端中注册 MCP]
```

### 10.6 多客户端 MCP 配置位置

| 客户端 | 配置文件位置 |
|--------|-------------|
| Claude Code | `~/.claude/mcp.json` |
| Kiro | `.kiro/settings/mcp.json`（workspace）或 `~/.kiro/settings/mcp.json`（全局） |
| Cursor | Cursor Settings → MCP |
| Cline | Cline 设置面板 |

bootstrap 默认写入 Claude Code 的配置路径；其他客户端在引导中说明对应位置。

### 10.7 完整示例："sqlmap-mcp"

```json
{
  "name": "sqlmap-mcp",
  "bootstrapKind": "local-http-mcp",
  "mcpNames": ["sqlmap"],
  "mcpUrl": "http://localhost:8775/mcp",
  "servicePort": 8775,
  "docsUrl": "https://github.com/xxx/sqlmap-mcp",
  "canAutoInstall": false,
  "verificationMode": "service-or-registration",
  "manualInstallHint": "需要 Docker：docker run -d -p 8775:8775 xxx/sqlmap-mcp"
}
```

`canAutoInstall: false` 表示 bootstrap 不尝试自动安装，但会注册 MCP URL、检测端口，并在不在线时输出 `manualInstallHint`。

### 10.8 验证清单（MCP 相关）

- [ ] `bootstrap-manifest.json` 中有对应条目
- [ ] `mcpNames` 与实际注册到客户端的 server name 一致
- [ ] `servicePort` 与实际服务端口一致
- [ ] `mcpUrl` 格式正确（含 `/mcp` 路径或实际 endpoint）
- [ ] 本地服务型有 `scripts/start.ps1` 或等价启动脚本
- [ ] `SKILL.md` 中有手动配置引导
- [ ] `canAutoInstall` 准确反映自动化能力（不虚标）
- [ ] 运行 `refresh-tool-index.ps1` 后，capability 视图可见新 MCP 的注册与在线状态

---

## 11. AI 自动新增 skill 的触发条件

AI 在执行任务中发现以下情况时，应主动提议新增技能：

1. 路由矩阵中找不到匹配的现有入口。
2. 需要的工具链与现有所有技能都不重叠。
3. 工作流足够独立，值得单独维护。
4. 同类任务预计会反复出现。

提议时应说明：建议的技能名称、覆盖场景、所需工具、与现有技能的关系（互补 / 替代 / 上下游）。用户确认后，按本文档流程执行。
