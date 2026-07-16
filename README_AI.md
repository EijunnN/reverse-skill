# AI Agent Bootstrap — Cybersecurity Skills Router

> **For AI agents only.** Human readers: see [README.md](README.md) (中文) or [README_EN.md](README_EN.md) (English).

This file is the bootstrap guide for AI agents (Claude Code, Codex CLI, Cursor, Cline, Windsurf, Kiro, etc.). It covers first-run setup, platform detection, deployment routing, and client integration.

The package can live in any directory; agents detect its real path from the location of this file. Throughout this document, `<SKILL_ROOT>` refers to that absolute installation path.

---

## 1. First-Run Setup (Execute on First Use)

> `skills/tool-index.md` is gitignored and does **not** exist in a fresh clone. Generate it **before** reading `RULES.md`, or routing will fail when it looks up tool availability.

Generate the tool index for the current machine:

```bash
# Windows
powershell -ExecutionPolicy Bypass -File skills/scripts/refresh-tool-index.ps1

# Linux / macOS
bash skills/scripts/refresh-tool-index.sh

# Kali Linux
bash kali/scripts/refresh-tool-index.sh
```

This produces `skills/tool-index.md` and `skills/tool-index.json`.

### Configuration Sequence

```text
0. Run refresh-tool-index (above) → generates skills/tool-index.md
1. Detect the package root (the directory containing this file)
2. Detect the local OS / distribution → follow the matching deployment document (table below)
3. Verify toolchain, script entry points, MCP configuration, and path conventions per that document
4. Read RULES.md → execute its startup sequence (authorization record, global injection, routing)
5. Enter the routed skill → begin the actual task
```

### Platform Deployment Routing

| Detection | Signal | Deployment Document | Script Entry |
|-----------|--------|--------------------|--------------|
| Windows | PowerShell, `$env:OS` | this file | `skills/scripts/*.ps1` |
| Kali Linux | `/etc/os-release` contains `kali` | `kali/README-kali.md` | `kali/scripts/bootstrap-reverse.sh`, `kali/scripts/refresh-tool-index.sh` |
| Ubuntu / Debian / Mint / Pop!_OS | `/etc/os-release` distro ID | `docs/platforms/linux.md` | `skills/scripts/bootstrap-reverse.sh`, `skills/scripts/refresh-tool-index.sh` |
| macOS | `uname -s` = `Darwin` | `docs/platforms/macos.md` | `skills/scripts/bootstrap-reverse.sh`, `skills/scripts/refresh-tool-index.sh` |
| Other / unknown | cannot identify confidently | `docs/PLATFORMS.md` | closest platform, then continue |

### Setup Report Format

After platform detection and `RULES.md` are loaded, report the configuration once, then proceed to the user's task — the report is a milestone, not the endpoint.

```markdown
**Reverse-skill pack configured**

- Installation path: C:\Users\xxx\Desktop\reverse-skill
- System detected: Windows / Kali / generic Linux / macOS
- Deployment document: <document actually read>
- Tools available: node, python, pip, ...
- Tools to auto-install on demand: jadx, radare2, ...
- Tools requiring manual install: zipalign, apksigner, IDA Pro
- Tool index: <tool-index.md path>
- Rules written to: <global config location>
```

---

## 2. What This Package Is

Not a single-tool installer — a **security-task skill router** for code agents: classify the task, enter the right workflow, then drive real tools. It solves two problems:

1. When the agent meets APK / binary / frontend JS / packet-capture / CTF tasks, it routes to the correct methodology and sub-skill before touching tools.
2. It consolidates local tools, MCP servers, script entry points, and workflows into a portable asset that migrates cleanly across machines.

## 3. Platform Support

| Platform | Status | Entry |
|----------|--------|-------|
| Windows | Full primary path | `README_AI.md`, PowerShell scripts |
| Kali Linux | Specialized support | `kali/README-kali.md`, `kali/scripts/` |
| Ubuntu / Debian | Generic support | `docs/platforms/linux.md`, `skills/scripts/` |
| macOS | Generic support | `docs/platforms/macos.md`, `skills/scripts/` |

List available bootstrap capabilities:

```bash
bash skills/scripts/bootstrap-reverse.sh --list   # generic Linux/macOS
bash kali/scripts/bootstrap-reverse.sh            # Kali entrypoint
```

See [docs/PLATFORMS.md](docs/PLATFORMS.md) for the full support matrix.

---

## 4. Package Contents

```text
<SKILL_ROOT>/
├── README_AI.md                  # This file
├── CTF-Sandbox-Orchestrator/     # Full CTF competition stack (40+ sub-skills)
└── skills/                       # Main skills directory
    ├── SKILL.md                  # Master routing entry
    ├── routing.md                # Scenario → skill dispatch (routing matrix)
    ├── CONTRIBUTING.md           # Standard process for adding skills
    ├── tool-index.md             # Tool registry (auto-generated)
    ├── scripts/                  # Bootstrap & tool-index scripts
    ├── field-journal/            # Authorization record & experience logs
    ├── apk-reverse/              # APK reverse engineering
    ├── attack-chain/             # Multi-stage attack-chain orchestration
    ├── binary-diff/              # Cross-version symbol migration
    ├── browser-automation/       # Browser + desktop automation
    ├── diagram-generator/        # Diagrams (Mermaid / Graphviz / PlantUML)
    ├── docs-generator/           # Report generation
    ├── dotnet-reverse/           # .NET / C# reversing
    ├── edr-bypass-re/            # EDR bypass reversing (authorized red team)
    ├── firmware-pentest/         # Firmware pentest chain (OWASP FSTM)
    ├── ida-reverse/              # IDA Pro reversing
    ├── js-reverse/               # Frontend JS reversing
    ├── llm-security/             # LLM / AI security testing
    ├── malware-analysis/         # Malware analysis (YARA / Sigma / sandbox)
    ├── mobile-reverse/           # Mobile security (Android + iOS)
    ├── patch-diff-exploit/       # N-day patch diff → exploitation
    ├── pentest-tools/            # Pentest toolchain
    ├── pwn-chain/                # RE → usable exploit (stack / heap / kernel)
    ├── radare2/                  # radare2 CLI reversing
    ├── reverse-engineering/      # General RE methodology
    └── supply-chain-security/    # SBOM / SCA / CI-CD security
```

Keep `CTF-Sandbox-Orchestrator/` at the package root (the default layout) so the relative paths in `routing.md` (`../CTF-Sandbox-Orchestrator/...`) resolve from `skills/`. If you place it elsewhere, adjust those paths manually.

---

## 5. Quick Start

### Minimal: Put the Pack in Place

1. Place the directory anywhere, e.g. `<SKILL_ROOT>/`
2. Open `skills/SKILL.md`
3. For each task, read in order: `SKILL.md` → `routing.md` → the target module's `SKILL.md` → `tool-index.md` (only when confirming local tools)

### Full: Auto-Routing from Any Code CLI

You need at least:

- A code CLI supporting custom rules / system prompts / project instructions / hooks
- A way to inject "read the routing file first for security tasks" into the model context
- MCP or an equivalent tool bridge, if external capabilities are needed
- This package's `SKILL.md`, `routing.md`, and `tool-index.md`

If you already have Claude hooks, Codex CLI project instructions, Cursor Rules, Cline custom instructions, or Windsurf Rules, update any stale paths to the current installation path.

---

## 6. Dependencies

### Core Clients and Runtimes

| Component | Required? | Project | Purpose |
|-----------|-----------|---------|---------|
| Claude Code | Recommended | https://github.com/anthropics/claude-code | Main AI client, best fit for this package |
| Node.js 22.12+ | For JS/MCP | https://nodejs.org/ | `npx`, `jshookmcp`, local JS reproduction |
| Python 3.x | Common | https://www.python.org/ | Frida, helper scripts |
| Java / JDK | For APK | https://adoptium.net/ | jadx, apktool |

### APK / Android Tools

| Component | Project | Purpose |
|-----------|---------|---------|
| jadx | https://github.com/skylot/jadx | Java decompilation |
| apktool | https://apktool.org/ | APK unpacking / rebuilding |
| Android platform-tools | https://developer.android.com/tools/releases/platform-tools | `adb` |
| Android Build-Tools | https://developer.android.com/tools/releases/build-tools | `apksigner`, `zipalign` |

### Dynamic / Browser / RE Tools

| Component | Project | Purpose |
|-----------|---------|---------|
| Frida / frida-tools | https://frida.re/ | Java / native dynamic instrumentation |
| IDA Pro | https://hex-rays.com/ida-pro/ | Decompilation, xrefs, data flow |
| radare2 | https://github.com/radareorg/radare2 | CLI reconnaissance, disassembly, diffing |

---

## 7. Scenario → Entry Point

| Scenario | Entry |
|----------|-------|
| APK / Android | `apk-reverse/SKILL.md` |
| exe / dll / so / elf | `ida-reverse/SKILL.md` or `radare2/SKILL.md` |
| Frontend signature / encrypted params | `js-reverse/SKILL.md` |
| HTTP capture / request replay | anything-analyzer + `js-reverse/` |
| Pentest / port & vulnerability scanning | `pentest-tools/SKILL.md` |
| Firmware / IoT / router pentest | `firmware-pentest/SKILL.md` |
| N-day / patch diff / CVE PoC | `patch-diff-exploit/SKILL.md` |
| Exploit writing / pwn | `pwn-chain/SKILL.md` |
| EDR / AV bypass (authorized red team) | `edr-bypass-re/SKILL.md` |
| Malware / YARA / IOC | `malware-analysis/SKILL.md` |
| Browser / desktop automation | `browser-automation/SKILL.md` |
| Symbol migration / cross-version compare | `binary-diff/SKILL.md` |
| Diagrams / attack-path diagrams | `diagram-generator/SKILL.md` |
| OLLVM deobfuscation | `reverse-engineering/references/ollvm-deobfuscation.md` |
| CTF challenge | dispatch via `CTF-Sandbox-Orchestrator/ctf-sandbox-orchestrator/SKILL.md` |

---

## 8. Startup and Verification

### Refresh the Tool Index

Do not trust a stale scan after migrating to a new machine — refresh first:

```powershell
powershell -File "<SKILL_ROOT>/skills/scripts/refresh-tool-index.ps1"
```

Verify the outputs: `skills/tool-index.md` and `skills/tool-index.json`. The `yes/no` flags reflect only the current machine's scan.

### IDA Pro Chain

```powershell
powershell -File "<SKILL_ROOT>/skills/ida-reverse/scripts/start.ps1"
powershell -File "<SKILL_ROOT>/skills/ida-reverse/scripts/open.ps1" -Path "C:\path\to\sample.exe" -TimeoutSeconds 600
```

### anything-analyzer

```bash
pnpm install && pnpm dev
```

The package assumes it eventually exposes an MCP endpoint such as `http://localhost:23816/mcp`.

### jshookmcp

Not a standalone entry — an enhanced execution surface for `js-reverse`:

```json
{
  "mcpServers": {
    "jshook": {
      "command": "npx",
      "args": ["-y", "@jshookmcp/jshook@latest"],
      "env": { "JSHOOK_BASE_PROFILE": "search" }
    }
  }
}
```

### APK Script Chain

Common scripts: `apk-reverse/scripts/decode.ps1`, `frida-run.ps1`, `rebuild-sign-install.ps1`, `manifest-summary.ps1`.

Verify after migration:

```powershell
jadx --version; apktool --version; adb version; frida-ps -U
```

---

## 9. Integration with AI Clients

### Principles

Regardless of client, integration means wiring four things:

1. This package directory
2. MCP (or equivalent) external tool endpoints
3. A stable instruction-injection method
4. The "route first, execute second" discipline

### MCP Example

```json
{
  "mcpServers": {
    "anything-analyzer": { "url": "http://localhost:23816/mcp" },
    "idapro": { "url": "http://127.0.0.1:13337/mcp" },
    "jshook": { "command": "npx", "args": ["-y", "@jshookmcp/jshook@latest"], "env": { "JSHOOK_BASE_PROFILE": "search" } },
    "burpsuite": { "command": "node", "args": ["<SKILL_ROOT>/burp-mcp-full/mcp-bridge.js"] }
  }
}
```

### Minimum Prompt Requirements

However instructions are injected, the agent must know three entry files:

- `skills/SKILL.md`
- `skills/routing.md`
- `skills/tool-index.md`

### Claude Code

Best fit for this package. If `.claude/settings.local.json`, `.claude/mcp.json`, or hooks already exist, update stale paths to the current installation path.

### Codex CLI / Cursor / Cline / Windsurf / Others

Reusable by any client that (1) supports MCP or equivalent tool integration and (2) supports rules / custom instructions / project-level instruction files. Inject: package path, key entry files, MCP addresses, and "route first, execute second".

---

## 10. After Migration

Update these when moving the package:

- Absolute paths: `<SKILL_ROOT>/...`, `<user dir>/...`, tool directories
- IDA scripts: `skills/ida-reverse/scripts/start.ps1`, `open.ps1`
- Claude local hook: `.claude/settings.local.json`, `.claude/scripts/route-reverse.ps1`
- Tool index: re-run the platform refresh script (§1)

### Verification Checklist

```powershell
java -version; python --version; pip --version; node -v; npx -v
jadx --version; apktool --version; adb version; frida-ps -U
```

---

## 11. FAQ

**Q1: Can `skills/` live on another drive?**
Yes — update every absolute path that references it.

**Q2: `tool-index.md` says `yes`, why can't the agent call the tool?**
The index only records that the executable exists locally; the tool may not be registered in the MCP configuration.

**Q3: Is IDA Pro required?**
No. Binary analysis can start with `radare2`.

**Q4: anything-analyzer vs. jshookmcp?**
anything-analyzer: browser automation + HTTP capture. jshookmcp: JS runtime / CDP / Hook / AST.

---

## 12. Auto-Evolution Mechanism

### Write-Back Triggers

Write experience back to `field-journal/` when any of these holds:

1. Task completed with final output
2. New toolchain pitfall discovered
3. Bootstrap defect found and fixed
4. Scenario not covered by the routing matrix discovered
5. Task failed with a reason worth recording

> field-journal write-back and docs-generator reports are different things: the former accumulates experience **for the system**; the latter produces formal documents **for users and teams**.

### Write-Back Content

Each entry contains: date, scenario category, goal summary, complete execution chain, pitfalls, toolchain findings, key code/commands, improvement suggestions, reusable patterns, and evolution actions. Template: `field-journal/_template.md`.

### Index Maintenance and Reuse

Each new entry updates `field-journal/_index.md` (scenario category, keywords, summary). Before starting a new task, check the index and read relevant prior entries first.

---

## 13. When Bootstrap Fails

Never stay silent or retry endlessly. After two failed attempts, switch to structured manual guidance:

```markdown
⚠️ **[Tool] automatic installation failed — manual action required.**

**Problem**: [specific error]

**Possible causes**:
- [e.g., network unavailable]
- [e.g., missing prerequisite]

**Manual installation steps**:
1. [Step 1]
2. [Step 2]

**After verification succeeds, tell me and I will continue the current task.**
```

Port conflicts: ask for the actual port and help update the configuration.

---

## 14. Key Files (Read Order)

If you only read five files:

1. `README.md` — human introduction
2. `RULES.md` — global operating rules
3. `skills/SKILL.md` — master routing entry
4. `skills/routing.md` — scenario → skill dispatch
5. `skills/tool-index.md` — local tool status

Then, as needed:

6. `skills/field-journal/authorization.md` — authorization & rules of engagement record
7. `skills/field-journal/precedent-reverse.md` — reverse-engineering precedents
8. `skills/field-journal/precedent-pentest.md` — security-testing precedents
9. `skills/CONTRIBUTING.md` — when adding a new skill

---

## License

This project is primarily MIT-licensed; `CTF-Sandbox-Orchestrator/` is GNU GPLv3; integrated third-party tools follow their own licenses.

This package is intended only for legally authorized security research, learning, and CTF competitions. Users must ensure all operations stay within legal boundaries; unauthorized testing against others' systems is illegal. The authors accept no responsibility for misuse.
