---
name: reverse-skill-router
description: 逆向工程、授权渗透测试与安全研究的技能路由总控。遇到 APK 逆向、二进制分析、前端 JS 逆向、CTF、渗透测试、固件分析、移动安全、API 安全等任务时首先进入本文件，按"目标类型 + 用户意图 + 工具链"三轴路由到对应子技能。Skill router for reverse engineering, authorized penetration testing, and security research — the entry point that dispatches each task to the right specialist module.
license: MIT
---

# 逆向工程技能路由总控

本目录是安全技能包的路由层：每个子目录是一个独立技能模块，模块内的 `SKILL.md` 描述其适用范围、工具链与工作流。

## 路由协议

收到安全类任务后，按以下顺序执行；不要停留在"已读、等待确认"的状态：

1. 阅读 [`routing.md`](routing.md)，按 **目标类型 + 用户意图 + 工具链** 三轴完成路由判定。
2. 阅读目标模块的 `SKILL.md`，提取第一个可执行动作。
3. 涉及本机工具时查阅 `tool-index.md`，校验工具是否可用及其实际路径；不凭记忆猜测路径。
4. 缺少工具时调用 `scripts/bootstrap-reverse.ps1` 按需补齐，然后继续任务。
5. 路由完成即开始执行。若路由无法命中，先补充方法论并提议新增技能，不强行归入不匹配的模块。

## 指令语义（RFC 2119）

| 关键词 | 含义 |
|--------|------|
| **MUST / MUST NOT** | 强制要求；违背即任务失败或越权 |
| **SHOULD** | 默认应当执行；不执行需说明理由 |
| **MAY** | 可选动作，按场景裁量 |

## 模块目录

| 模块 | 目录 | 适用场景 |
|------|------|---------|
| 通用逆向 | `reverse-engineering/` | GDB / Frida / angr / Unicorn / Qiling、反分析对抗、全语言平台逆向、CTF 模式库 |
| DSL 虚拟机逆向 | `reverse-engineering/dsl-vm-reverse/` | JavaScript 自定义指令集 VM（IIFE + switch-case 解释器）：风控引擎、自定义验证码 VM |
| APK 逆向 | `apk-reverse/` | APK 解包、jadx 反编译、smali 修改、Frida Hook、重打包签名安装 |
| .NET 逆向 | `dotnet-reverse/` | 托管 PE 逆向、dnSpyEx + de4dot 脱混淆、IL patch、Sharp* 工具分析 |
| IDA Pro 逆向 | `ida-reverse/` | IDA Pro MCP 服务：反编译、反汇编、数据流追踪、交叉引用 |
| 前端 JS 逆向 | `js-reverse/` | 浏览器端签名定位、加密参数分析、运行时采样、Node 补环境复现 |
| radare2 分析 | `radare2/` | CLI 二进制侦察、反汇编、patch：r2 / rabin2 / rasm2 / radiff2 |
| 恶意软件分析 | `malware-analysis/` | YARA / Sigma / 沙箱编排 / IOC 提取，静态 + 动态 + 行为三合一 |
| 跨版本符号迁移 | `binary-diff/` | 旧版符号迁移到新版、缺 PDB 推导、更新后批量迁移函数名 |
| N-day 补丁差分 | `patch-diff-exploit/` | 从厂商补丁定位漏洞点、编写 PoC（与 binary-diff 分工：本模块偏攻击侧） |
| RE → 利用链 | `pwn-chain/` | 从逆向走到可用 exploit：栈/堆/内核 pwn、pwntools、CTF 到真实远程的稳定化 |
| 固件渗透链 | `firmware-pentest/` | OWASP FSTM 九阶段：提取 → EMBA → 仿真 → fuzz → 实机利用 |
| EDR 绕过逆向 | `edr-bypass-re/` | 授权红队场景：逆向 EDR 的 hook 表 / ETW / AMSI → 直接 syscall 等对抗技术 |
| 渗透测试工具链 | `pentest-tools/` | Nmap / Nuclei / SQLMap / FFUF / Hashcat 等 20+ 工具，经 MCP 暴露给 AI |
| API 安全测试 | `api-security/` | REST / GraphQL / WebSocket：BOLA/IDOR、JWT/OAuth 攻击、十阶段方法论 |
| 移动安全 | `mobile-reverse/` | Android + iOS：Frida / Objection 插桩、SSL Pinning 与越狱检测绕过、OWASP MASTG |
| LLM/AI 安全 | `llm-security/` | OWASP LLM Top 10：Prompt 注入、工具滥用、记忆投毒、Agent 劫持 |
| 供应链安全 | `supply-chain-security/` | SBOM / SCA / CI-CD：依赖扫描、容器安全、构建完整性、漏洞可达性 |
| 攻击链编排 | `attack-chain/` | 多阶段攻击路径规划与执行的总指挥；跨阶段任务从这里开始 |
| CTF 竞赛全栈 | `../CTF-Sandbox-Orchestrator/` | 40+ 子技能：Web / 逆向 / Pwn / 云 / 容器 / AD / 取证 / 隐写 / 移动端 / 密码学 |
| 浏览器与桌面自动化 | `browser-automation/` | Playwright 浏览器操作 + Windows 桌面 UIA 操作 + 网络观察 |
| 图表生成 | `diagram-generator/` | 从自然语言生成 Mermaid / Graphviz / PlantUML：攻击路径图、数据流图、架构图 |
| 技术文档编写 | `docs-generator/` | 任务完成后生成逆向报告、渗透报告、CTF writeup |

## 阶段门禁与下一步菜单

每个子技能完成一个阶段后，应提供 3–6 个编号的下一步选项，由用户选择方向后再跨阶段推进。选项要求：

- 以数字编号，每项是一个具体可执行的动作，而非抽象方向。
- 至少包含一个"导出报告 / 写 writeup"选项。
- 至少包含一个"继续深入"或"换一种方法"选项。
- 必要时包含一个"暂停 / 确认证据"出口。

示例：

```text
## 建议下一步（选一个编号）

1. 对 sub_140001000 做深度反编译，还原算法
2. 用 Frida 动态 Hook 验证参数猜想
3. 导出当前已命名函数，生成符号迁移 YAML
4. 生成当前阶段的分析报告
5. 换 radare2 做轻量侦察对比
6. 暂停，先确认前面的证据
```

## 工具自举（On-Demand Bootstrap）

工作流发现缺少工具时不要直接报错，统一调用：

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File "<skill-root>\scripts\bootstrap-reverse.ps1" -Capability @('工具名') -StartServices
```

支持的能力：jadx、apktool、frida、frida-ps、idalib-mcp、jshookmcp、anything-analyzer、idapro、r2、rabin2、adb、agent-browser、ghidra-mcp、nmap、seclists、proxycat、burpsuite-mcp、pentestswarm、binwalk、unblob、emba、firmadyne、qemu-static、pwntools、ropgadget、one_gadget、bindiff、ghidriff、syswhispers3、pe-sieve、garak、pyrit、osv-scanner、trivy、syft、gitleaks、objection、yara、floss。

自举完成后会自动刷新 `tool-index.md`。

## 授权与操作边界

执行任何逆向 / 渗透操作前，先阅读 [`field-journal/authorization.md`](field-journal/authorization.md)：它定义了本技能包适用的授权场景（SRC / Bug Bounty、自有资产、合约授权、CTF 靶场、公开研究）与操作边界。授权上下文由操作者负责声明并承担相应责任；技能包内的所有方法论均以此类授权场景为前提。

## 经验沉淀（Auto-Evolution）

每次完成逆向 / 渗透任务后，将可复用的经验回写到 `field-journal/`：

- 模板：`field-journal/_template.md`
- 索引：`field-journal/_index.md`
- 既有案例：`field-journal/precedent-reverse.md`、`field-journal/precedent-pentest.md`
- 新任务开始前先查索引，复用已有经验；详细规则见 `../RULES.md` 的完成清单（Completion Checklist）一节。

## 完成前自检

- [ ] 完成了路由三轴匹配（目标类型 + 用户意图 + 工具链）
- [ ] 路由成功后阅读了目标技能的 `SKILL.md`
- [ ] 路由未命中时提议了新增技能，而非强行匹配
- [ ] 基于 `tool-index.md` 使用了真实工具路径
- [ ] 任务经验已回写 `field-journal/`

## 目录动态扩充

本目录会持续增长。发现新的子目录时，阅读其 `SKILL.md` 即可了解用途。新增技能请按 [`CONTRIBUTING.md`](CONTRIBUTING.md) 的标准流程操作，确保路由矩阵、bootstrap 系统与 tool-index 同步更新。

## 关联资源

- `tool-index.md`：本机工具可用性、实际路径、版本与脚本引用的唯一事实来源（首次克隆后需用 `scripts/refresh-tool-index.sh` 生成）。
- 包根目录的 `../README.md`：面向 Claude Code、Codex CLI 及其他代码 AI 客户端的安装与接入说明。
