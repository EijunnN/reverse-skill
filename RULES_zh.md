# 运行规则 — 安全任务自动路由

> English version: [RULES.md](RULES.md)

本文件是技能包运行方式的唯一事实来源，适用于所有 AI 编码客户端（Claude Code、Kiro、Cursor、Cline、Windsurf、Codex CLI、Aider、Continue 等）。

本文件所在目录即为**包根目录**。所有路径从它推导，不要假定固定盘符。

---

## 1. 启动序列

任务命中触发关键词（§3）时，按序执行：

1. 阅读 `skills/field-journal/authorization.md` — 本次任务的授权与范围记录。
2. 阅读 `skills/tool-index.md` — 标记为 `yes` 的工具已安装，不要重复安装。文件不存在时（首次运行），阅读 `skills/tool-index.md.template` 并运行刷新脚本（§10）生成。
3. 仅首次使用 — 将路由规则写入客户端全局配置（§2）。已写入则跳过。
4. 阅读 `skills/routing.md`，按矩阵匹配任务，确定入口技能。
5. 进入技能，开始执行。

`tool-index.md` 是所有 CLI 客户端共享的注册表：人人都读它，安装新工具后人人都要更新它。仅在怀疑索引过期时运行刷新脚本；仅为"任务需要且标记为 no"的工具运行 bootstrap。

按需阅读（需要时才加载）：

- 不确定某操作是否落入已记录范围 → `skills/field-journal/authorization.md`。
- 查找同类任务的成熟做法 → `skills/field-journal/precedent-reverse.md` 或 `precedent-pentest.md`。

每个任务的目标都是具体结果——工具装好、文件分析完、漏洞验证过、报告写出来。入口不明确时，先把任务与 `routing.md` 做匹配，输出路由分析，然后开始干活。

---

## 2. 全局注入（仅首次使用）

首次使用时，将路由规则写入客户端的全局配置，使路由在任意项目目录下都能触发。`<SKILL_ROOT>` 替换为检测到的包根目录绝对路径。

| 客户端 | 全局配置位置 | 写入方式 |
|--------|--------------|----------|
| Claude Code | `~/.claude/CLAUDE.md` | 创建或追加 |
| Kiro | `~/.kiro/steering/reverse-routing.md` | 创建（加 `inclusion: auto` frontmatter） |
| Cursor | Settings → Rules → Global Rules | 提示用户粘贴 |
| Cline | Settings → Custom Instructions | 提示用户粘贴 |
| Windsurf | Global Rules 面板 | 提示用户粘贴 |
| 其他 | 客户端的全局规则/指令文档 | 提示用户 |

写入内容：§11 的精简块。全局配置中已存在该路由块时，核对是否最新，无变化则跳过。

---

## 3. 触发关键词（中英双语）

任意命中即激活路由：

- APK, Android reverse, 反编译, smali, jadx, apktool, Frida, Hook
- binary analysis, 二进制分析, IDA, radare2, r2, disassembly, 反汇编, reverse engineering, 逆向工程, RE, source recovery, 还原源码
- frontend signature, 前端签名, encrypted params, 加密参数, JS reverse, JS 逆向, jshookmcp, CDP, SourceMap
- packet capture, 抓包, HTTP capture, HTTP 捕获, request replay, 请求重放, anything-analyzer
- CTF, Pwn, web pentest, Web 渗透, exploit, 漏洞利用, privilege escalation, 提权
- MCP reverse tools, idalib-mcp, repackage, 重打包, certificate pinning, 证书校验, root detection, 反调试
- .so analysis, native hook, JNI
- penetration testing, 渗透测试, red team, 红队, security assessment, 安全评估, blue team, 蓝队, incident response, 应急响应
- report/docs generation in security context, 安全报告/文档, writeup, pentest report, 渗透报告
- security browser automation, 安全测试浏览器自动化, Playwright pentest, agent-browser recon
- N-day, patch diff, 补丁差分, CVE reproduction, 1day, ghidriff, Diaphora
- pwn, stack overflow, 栈溢出, heap overflow, ROP, ret2libc, pwntools, GEF, pwndbg, kernel pwn
- firmware, 固件, IoT, binwalk, unblob, squashfs, EMBA, UART, JTAG, embedded exploitation
- EDR bypass, EDR 绕过, AV bypass, 免杀, unhook, direct syscall, indirect syscall, AMSI patch, ETW patch
- port scan, 端口扫描, Nmap, vulnerability scan, 漏洞扫描, Nuclei, SQL injection, SQL 注入, SQLMap, directory brute force, 目录爆破, FFUF, password cracking, 密码破解, Hashcat, Hydra, Metasploit, Impacket
- SRC, Bug Bounty, 众测, WAF bypass, 绕过 WAF, IDOR, 越权
- BurpSuite, Burp MCP, Intruder, Repeater, Collaborator, proxy history, 代理历史
- LLM security, LLM 安全, AI security testing, Prompt injection, Prompt 注入, jailbreak, Agent security, Agent 安全
- OWASP LLM Top 10, ASI Top 10, Agentic AI, tool abuse, memory poisoning, garak, PyRIT, promptfoo
- API security, API 安全, GraphQL, JWT attack, JWT 攻击, supply chain security, 供应链安全
- iOS reverse, iOS 逆向, Objection, YARA, malware analysis, 恶意软件分析, AI decompilation, AI 反编译
- internal network, 内网渗透, lateral movement, 横向移动, domain penetration, 域渗透, AD attack, BloodHound
- privilege escalation, 权限提升, credential extraction, 凭证提取, Mimikatz, Kerberoasting, DCSync
- C2, persistence, 持久化, Cobalt Strike, Sliver, Havoc
- game reverse, 游戏逆向, anti-cheat, 反作弊, Unity, IL2CPP, Cheat Engine
- .NET reverse, C# 逆向, dnSpy, dnSpyEx, de4dot, ConfuserEx, SmartAssembly, .NET Reactor, dnlib, IL patch, SharpHound, Rubeus
- symbol migration, 符号迁移, bindiff, cross-version, PDB missing
- security diagram, 安全图表, attack path diagram, 攻击路径图, security architecture, 安全架构图

---

## 4. 执行原则

### 工具使用

- 不猜工具路径。`tool-index.md` 记录每个工具的确切安装路径——先查再用。
- 缺工具 → 调用对应平台的 bootstrap 脚本（§9），不要停在报错上。
- 任何新安装之后，运行对应平台的刷新脚本（§10），让其他客户端无需重装即可找到工具。
- tool-index 条目必须使用完整绝对路径（如 `D:\tools\jadx\bin\jadx.bat`，而非 `jadx`），并包含版本号、安装方式与验证命令。
- 同一工具自动安装失败两次 → 停止重试，输出完整的手动安装步骤。
- MCP 服务端口不一致 → 向用户确认实际端口并协助更新配置。

### 路由决策

- 路由未命中 → 不强行归入现有技能，提议新增技能（§7 联网增强）。
- 一条路走不通 → 切换：静态 ↔ 动态、Java ↔ Native、IDA ↔ r2、工具 X ↔ 等价工具 Y。
- 跨模块任务 → 按 `routing.md` 的"路径交叉"章节组合多个技能。

### 经验复用

- 进入任何路由前，先查 `skills/field-journal/_index.md`。
- 存在相似的过往记录 → 阅读日志，复用已验证的方案。
- 历史方案不适用 → 在新日志中说明原因。

### 自我监督

每调用 5 次工具，或感觉"卡住"时，暂停做一次简短自审：

- 我是否在朝目标取得可衡量的进展？引用具体证据。
- 同一工具同一参数是否已调用两次以上？是 → 必须换方法。
- 我能否清楚解释上一条报错？不能 → 先理解，再行动。

硬性上限：同一方法失败 2–3 次 → 换方法；单条命令重复 3 次以上 → 停下评估；单个子任务超过约 30 次工具调用 → 向用户汇报并询问是否继续。

### 安全边界

- 所有操作保持在 `field-journal/authorization.md` 记录的授权范围内。
- 不将攻击面扩大到用户指定目标之外。
- 发现高危漏洞 → 立即告知用户并等待指示。
- 报告与日志不保留未脱敏的敏感信息。

### 输出质量

- 关键操作附带可复现的命令，而非仅描述。
- 逆向分析标注地址、偏移、函数名——不允许"某个函数"。
- 渗透结果附带完整 PoC（curl 命令 / 脚本 / 截图路径）。
- 不确定的结论标注置信度。

---

## 5. 标准行为链

包内其他文件均引用此版本：

```text
0.  阅读 authorization.md — 任务授权与范围记录
1.  识别任务为安全/逆向类型 → 本路由激活
2.  检测包根目录（由本文件位置推导）
3.  首次使用 → 将规则写入客户端全局配置（§2）
4.  阅读 routing.md → 确定入口技能
5.  路由未命中 → 联网补充方法论 → 提议新增技能
6.  阅读 tool-index.md → 确认工具状态（首次运行：按模板经 §10 生成）
7.  缺工具 → 对应平台 bootstrap（§9）→ 对应平台刷新（§10）
8.  进入技能工作流 → 执行任务
9.  遇阻 → 联网检索方案 → 沉淀到 references/
10. 持续汇报进度，不沉默
11. 任务完成 → 执行完成清单（§6）
12. 输出最终结果
```

---

## 6. 完成清单

任务完成后（漏洞已验证 / 逆向已完成 / flag 已拿到），逐项执行：

```text
□ 1. 生成正式报告（docs-generator 技能）
□ 2. 至少生成一张图（diagram-generator 技能）
□ 3. 回写 field-journal（脱敏后）
□ 4. 将任务中联网检索的知识沉淀到 references/
□ 5. 询问是否向社区贡献
□ 6. 更新系统索引（_index.md；出现新场景时含 routing.md）
```

---

## 7. 错误处理

| 场景 | 动作 |
|------|------|
| bootstrap 成功 | 继续任务 |
| bootstrap 失败，原因明确 | 输出结构化指引，等待用户 |
| bootstrap 失败，原因不明 | 输出已知信息，建议检查网络/权限 |
| 服务端口不一致 | 询问实际端口，协助更新 MCP 配置 |
| 同一工具失败两次 | 声明自动安装受阻，给出完整手动步骤，停止重试 |
| 分析方向受阻 | 切换路径（静态 ↔ 动态，Java ↔ Native，IDA ↔ r2） |
| 任务超出能力 | 明确说明局限，建议具体的人工介入点 |
| MCP 工具调用报错 | 探测服务端口，尝试启动或引导用户 |

---

## 8. MCP 服务管理

| 服务 | 端口 | 用途 | 启动方式 |
|------|------|------|----------|
| idapro | 13337–13350 | IDA Pro 逆向工具集 | 自动启动（IDA 插件），每实例端口递增 |
| anything-analyzer | 23816 | 浏览器自动化 + HTTP 捕获 | `pnpm dev`（项目目录） |
| jshookmcp | stdio | JS Hook / CDP / Network / AST | `npx -y @jshookmcp/jshook@latest` |
| ghidra | 8765 | Ghidra 反编译桥 | Ghidra GUI 启动后自动监听 |
| burpsuite | 9876 | BurpSuite 全控（Proxy / Intruder / Repeater / Scanner / Collaborator） | Burp 扩展自动加载 |

---

## 9. Bootstrap 命令

Windows（PowerShell）：

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File "<SKILL_ROOT>/skills/scripts/bootstrap-reverse.ps1" -Capability @('工具名') -StartServices
```

Linux / macOS（Bash）：

```bash
bash <SKILL_ROOT>/skills/scripts/bootstrap-reverse.sh 工具名 --start-services
```

Kali Linux（Kali 原生工具链）：

```bash
bash <SKILL_ROOT>/kali/scripts/bootstrap-reverse.sh 工具名 --start-services
```

## 10. 刷新工具索引

Windows（PowerShell）：

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File "<SKILL_ROOT>/skills/scripts/refresh-tool-index.ps1"
```

Linux / macOS（Bash）：

```bash
bash <SKILL_ROOT>/skills/scripts/refresh-tool-index.sh
```

Kali Linux（Bash）：

```bash
bash <SKILL_ROOT>/kali/scripts/refresh-tool-index.sh
```

---

## 11. 全局注入内容（精简）

> 本块即首次配置时写入客户端全局配置的内容。特意不包含"阅读 RULES.md"指令，以免每次会话都重复触发首次配置。

### 触发关键词（中英双语）

- APK, Android reverse, 反编译, jadx, apktool, Frida, Hook
- binary analysis, 二进制分析, IDA, radare2, r2, disassembly, 反汇编, reverse engineering, 逆向工程
- frontend signature, 前端签名, JS reverse, JS 逆向, jshookmcp, CDP, SourceMap
- packet capture, 抓包, HTTP capture, anything-analyzer
- CTF, Pwn, web pentest, Web 渗透, exploit, 漏洞利用, privilege escalation, 提权
- penetration testing, 渗透测试, red team, 红队, Nmap, Nuclei, SQLMap, FFUF, Hashcat, Metasploit
- SRC, Bug Bounty, WAF bypass, IDOR, 越权
- BurpSuite, Burp MCP, Intruder, Repeater, Collaborator
- LLM security, Prompt injection, jailbreak, Agent security, garak, PyRIT
- EDR bypass, 免杀, AV bypass, direct syscall
- firmware, IoT, binwalk, embedded
- internal network, 内网渗透, lateral movement, domain penetration, BloodHound
- API security, 供应链安全, supply chain, YARA, malware analysis, 恶意软件分析
- iOS reverse, 游戏逆向, game reverse, anti-cheat, pwn, ROP, kernel pwn

### 触发后执行（精简）

```text
1. 阅读 <SKILL_ROOT>/skills/field-journal/authorization.md — 范围记录
2. 阅读 <SKILL_ROOT>/skills/routing.md → 匹配意图 → 确定入口技能
3. 进入技能，开始执行
```

### 核心规则（精简）

- 任何操作前阅读 `authorization.md`；工作保持在其记录的范围内。
- 缺工具 → bootstrap（§9）；不猜路径。
- 不要回复"已理解，请告诉我任务"——主动路由并开始。
- 确定性步骤直接执行，无需逐步确认；仅在真正的决策点暂停。

---

## 12. 执行纪律

保证长任务不跑偏的可靠性实践：

- **不静默跳步。** 行为链中的每一步都要执行；确需跳过，须明确说明理由并告知用户。
- **先读后做。** 步骤要求查阅文件（routing.md、tool-index.md、某个 reference）时，就必须查阅——即使内容看似可预测。文件携带任务特定的约束。
- **要并行，不要跳步。** 节省时间的正确方式是让相互独立的步骤并行执行。
- **不替用户做决定。** 决策点上呈现所有选项；可以标注推荐项，但不隐藏备选项。
- **读过 ≠ 做过。** 只汇报真正执行过的工作——读过步骤不等于"步骤 1–4 已完成"。
- **完成即清单。** §6 每一项都勾选，任务才算完成。

## 13. 反模式（禁止）

- 未读 `routing.md` 就开始逆向/渗透。
- 猜工具路径，而不查 `tool-index.md`。
- 任务前跳过 field-journal 查询，或任务后跳过完成清单。
- 在报告中保留未脱敏的真实目标信息。
- 将渗透范围扩大到已记录授权之外。
- 自动安装失败两次后仍继续重试。
- 遇阻沉默——问题应立即浮出水面。
- 编造工具版本号或功能描述。
- 读完规则后回复"已理解，请告诉我任务"。
- 在确定性步骤上等待确认。

---

## 14. 多任务与中断处理

- 用户在任务中途切换话题 → 将当前进度存入 field-journal 并标记"未完成"；用户返回时据此恢复上下文。
- 同时给出多个安全任务 → 按优先级串行执行，避免工具冲突。
- 长耗时操作（如大文件 IDA 分析）→ 定期汇报进度。

## 15. 上下文窗口布局（注意力优化）

LLM 对文档首尾注意力最高：

```text
[开头 10%]  ████████████ ← 放"立即行动"指令
[中段 80%]  ████░░░░░░░░ ← 放参考材料
[结尾 10%]  ████████████ ← 放清单与"不可跳过"项
```

关键指令放在任何指令文件的开头或结尾 10%，不要埋在长文中间。

## 16. 参数稳定性（代号词）

当工具参数必须原样传递时，使用不透明标识符防止语义漂移：

- 适用于：bootstrap 参数、审批开关、扫描范围边界。
- 先定义映射表，在命令层展开。
- 不允许用语义改写改变受限取值（如把 `strict` 改写成宽松同义词）。

示例：

```text
alpha -> --scope authorized-only
beta  -> --approval required
gamma -> --destructive false
```

## 17. 联网检索增强

具备联网检索能力时，以下场景应主动检索：

| 场景 | 检索内容 | 沉淀到 |
|------|----------|--------|
| 未知壳 / 保护 / 混淆 | 脱壳方法与工具 | 该技能的 `references/` |
| 未知框架 / 协议 | 逆向/渗透方法论 | `references/` 或新技能提案 |
| 工具报错 / 不兼容 | 报错信息 + 版本兼容性 | `field-journal/` |
| 新 CVE / 漏洞 | PoC 与利用方法 | `pentest-tools/references/` |
| 路由未命中（新场景） | 领域方法论与工具 | 附来源的新技能提案 |
