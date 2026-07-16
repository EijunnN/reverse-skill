<p align="center">
  <img src="reverse-skill.png" alt="reverse-skill" width="140" />
</p>

<h1 align="center">reverse-skill</h1>
<h3 align="center">Reverse Engineering / Authorized Penetration Testing / Security Research Skill Router Pack</h3>

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
  <a href="#about">About</a> ·
  <a href="#quick-start">Quick Start</a> ·
  <a href="#usage">Usage</a> ·
  <a href="skills/routing.md">Routing Matrix</a> ·
  <a href="README_AI.md">AI Bootstrap</a> ·
  <a href="#contributing">Contributing</a>
</p>

<p align="center">
  🌐 <a href="README.md">中文</a>
</p>

<br/>

<a id="about"></a>

## About

> **If you are an AI agent, read [README_AI.md](README_AI.md) and follow its bootstrap sequence exactly.**

When an AI agent (Claude Code, Codex CLI, Cursor, etc.) faces an APK, a binary, obfuscated frontend JS, a CTF challenge, or a pentest task, this system routes it to the right methodology first, then drives the local toolchain — instead of guessing commands.

```
User task → RULES.md → Skill Router → Target Skill → Tools / MCP / Scripts → Report + Knowledge Base
```

**Why this project exists:**

- AI agents facing APK, ELF, JS, or PCAP don't know whether to reach for jadx, Frida, or IDA.
- Tool paths, MCP services, and script entry points are scattered across machines and hard to migrate.
- The same pitfalls get rediscovered on every task; hard-won experience never compounds.

Full routing matrix: [skills/routing.md](skills/routing.md)

### Tech Stack

<p align="left">
  <img src="https://skillicons.dev/icons?i=py,nodejs,powershell,bash,java,docker,git&theme=light" /><br/>
  <code>IDA Pro</code> · <code>radare2</code> · <code>Ghidra</code>
</p>

<a id="quick-start"></a>

## Quick Start

### Prerequisites

- **Java / JDK** — runs jadx and apktool
- **Node.js 22.12+** — JS toolchain and MCP services
- **Python 3.x** — Frida and helper scripts
- **An AI coding client** — Claude Code, Codex CLI, Cursor, etc.

### Install

```bash
git clone https://github.com/EijunnN/reverse-skill.git
```

### First Use

> **For first deployment, simply have the AI read [README_AI.md](README_AI.md) — nothing else is required.**

Platform-specific deployment guides:

- **Kali Linux** → [kali/README-kali.md](kali/README-kali.md)
- **Ubuntu / Debian** → [docs/platforms/linux.md](docs/platforms/linux.md)
- **macOS** → [docs/platforms/macos.md](docs/platforms/macos.md)

<a id="usage"></a>

## Usage

### Supported Scenarios

| Scenario | Entry Point |
|----------|-------------|
| APK / Android reversing | `skills/apk-reverse/` |
| Binary reversing (exe/dll/so/elf) | `skills/ida-reverse/` / `skills/radare2/` |
| Frontend JS signatures / encrypted params | `skills/js-reverse/` |
| HTTP capture / request replay | anything-analyzer + `skills/js-reverse/` |
| Pentesting / vulnerability scanning | `skills/pentest-tools/` |
| CTF competitions | `CTF-Sandbox-Orchestrator/` (40+ sub-skills) |
| Firmware / IoT | `skills/firmware-pentest/` |
| Patch diffing / N-day | `skills/patch-diff-exploit/` |
| Pwn / exploitation | `skills/pwn-chain/` |
| EDR bypass (authorized red team) | `skills/edr-bypass-re/` |
| LLM / AI security | `skills/llm-security/` |
| OLLVM deobfuscation | `skills/reverse-engineering/references/ollvm-deobfuscation.md` |
| Diagrams / reports | `skills/diagram-generator/` / `skills/docs-generator/` |

### Key Files

| File | Purpose |
|------|---------|
| [README_AI.md](README_AI.md) | AI agent bootstrap guide (agents read this first) |
| [RULES.md](RULES.md) | Global operating rules |
| [skills/SKILL.md](skills/SKILL.md) | Master routing entry |
| [skills/routing.md](skills/routing.md) | Routing matrix (scenario → skill) |
| [skills/field-journal/authorization.md](skills/field-journal/authorization.md) | Authorization & rules of engagement record |
| [skills/tool-index.md](skills/tool-index.md) | Local tool registry (auto-generated) |

### Repository Layout

```
.
├── README.md               # 中文版
├── README_EN.md            # This file
├── README_AI.md            # AI agent bootstrap guide
├── RULES.md                # Global operating rules
├── RULES_zh.md             # 全局运行规则（中文）
├── skills/
│   ├── SKILL.md            # Master routing entry
│   ├── routing.md          # Routing matrix
│   ├── CONTRIBUTING.md     # Standard process for new skills
│   ├── field-journal/      # Experience logs & authorization record
│   ├── scripts/            # Bootstrap & tool-index scripts
│   ├── apk-reverse/        # APK reversing
│   ├── js-reverse/         # JS reversing
│   ├── ida-reverse/        # IDA Pro workflows
│   ├── radare2/            # radare2 analysis
│   ├── reverse-engineering/ # General RE methodology
│   ├── dotnet-reverse/     # .NET reversing
│   ├── malware-analysis/   # Malware analysis
│   ├── mobile-reverse/     # Mobile security (Android + iOS)
│   ├── pentest-tools/      # Pentest toolchain
│   ├── api-security/       # API security
│   ├── supply-chain-security/ # Supply chain security
│   ├── attack-chain/       # Attack chain orchestration
│   ├── pwn-chain/          # Exploitation chains
│   ├── patch-diff-exploit/ # N-day analysis
│   ├── firmware-pentest/   # Firmware / IoT
│   ├── edr-bypass-re/      # EDR bypass reversing
│   ├── binary-diff/        # Symbol migration
│   ├── browser-automation/ # Browser & desktop automation
│   ├── diagram-generator/  # Diagram generation
│   ├── docs-generator/     # Report generation
│   └── llm-security/       # LLM / AI security
├── CTF-Sandbox-Orchestrator/ # CTF sub-skills (40+)
├── docs/                     # Overview & architecture docs
└── kali/                     # Kali helper scripts
```

<a id="contributing"></a>

## Contributing

Contributions are welcome. New skills must follow the standard process in [skills/CONTRIBUTING.md](skills/CONTRIBUTING.md); general improvements go through the usual PR flow:

1. Fork the project
2. `git checkout -b feature/AmazingFeature`
3. `git commit -m 'Add some AmazingFeature'`
4. `git push origin feature/AmazingFeature`
5. Open a Pull Request

<a id="license"></a>

## License

The main body of this project is released under the **MIT License** (see [LICENSE](LICENSE)).

**Submodules and third-party dependencies:**

- **CTF-Sandbox-Orchestrator/**: **GNU GPLv3**
- **Pentest Swarm AI**: the upstream project is **AGPL-3.0**; this repository invokes it via CLI/MCP only and does not contain its source code
- Other tools (jadx, frida, nmap, burpsuite-mcp, etc.) are governed by their own licenses

## Disclaimer

This skill pack is intended solely for **authorized** security research and testing (SRC / bug bounty programs, owned assets, contracted engagements, CTF ranges, public research). Users must ensure their operations are authorized by the target party and comply with applicable law. The authors and contributors accept no liability for misuse.

## Acknowledgments

This project is forked from [zhaoxuya520/reverse-skill](https://github.com/zhaoxuya520/reverse-skill) — thanks to the original author and all upstream contributors. Thanks also to every open-source security tool author; each tool integrated here is a product of the community's collective expertise.

## Contact

- **Issues**: [GitHub Issues](https://github.com/EijunnN/reverse-skill/issues)
