---
name: reverse-skill
description: Security task router and methodology guide for AUTHORIZED reverse engineering, binary/firmware analysis, penetration testing, malware analysis, and CTF challenges. Use when the task involves disassembly/decompilation (IDA, radare2, Ghidra), APK/mobile reverse engineering (jadx, apktool, frida), JS deobfuscation, pwn/exploit development, patch diffing, fuzzing, pentest tooling (nmap, nuclei, sqlmap, ffuf, hashcat), or CTF (web/pwn/crypto/forensics/stego). Routes the task to the right sub-skill and verifies local toolchain availability.
---

# reverse-skill — Security Skill Router (entry point)

This is the Claude Code entry point for the reverse-skill package. It only routes;
the real methodology lives in the package tree under `skills/`.

## Scope / authorization

This package is for **authorized** security work only: pentest engagements with
permission, CTF competitions, security research on systems you own or are
permitted to test, and education. Confirm authorization before acting on a
real-world target. Do not use it for unauthorized intrusion, destructive
actions, or evading defenses on systems you do not have permission to test.

## Routing contract — read these files in order when a security task arrives

1. Read `skills/SKILL.md` (master control — scenario/module matrix).
2. Read `skills/routing.md` (dispatch by target type + intent + toolchain) and
   pick the matching sub-skill.
3. Read the chosen sub-skill's `skills/<module>/SKILL.md` and extract the first
   executable step.
4. Only when you need to confirm a local tool's path/availability, read
   `skills/tool-index.md` — never guess tool paths from memory.
5. CTF tasks: route into `CTF-Sandbox-Orchestrator/` (40+ sub-skills).

All paths above are relative to this skill's own directory (the directory
containing this file), which is the package root `<SKILL_ROOT>`.

## Tool index

`skills/tool-index.md` is generated from a template by the refresh script and
reflects what is actually installed on this machine. To (re)generate it:

- Windows: `powershell -ExecutionPolicy Bypass -File skills/scripts/refresh-tool-index.ps1`
- Linux/macOS: `bash skills/scripts/refresh-tool-index.sh`

Many external tools (IDA Pro, jadx, apktool, frida, radare2, Android
build-tools) and MCP servers (ida-pro-mcp, jshookmcp, pentest-tools,
burp-mcp-full) are optional dependencies installed separately — see `README.md`
section 4 (Dependency Table). Missing tools are reported in `tool-index.md`.

## Reference docs (in this package)

- `README.md` / `README_EN.md` / `README_AI.md` — full install + dependency guide
- `RULES.md` / `RULES_zh.md` — agent behavior rules
- `docs/PLATFORMS.md` — platform-specific guidance
- `docs/ARCHITECTURE.md` / `docs/OVERVIEW.md` — design overview
