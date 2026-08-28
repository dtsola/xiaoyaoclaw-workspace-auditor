# OpenClaw Workspace Auditor 🩺

<div align="center">
  <strong>Read-only workspace health check</strong> | <a href="README.md">中文</a>
</div>

<p align="center">
  <img src="./assets/readme/hero.svg" width="100%" alt="OpenClaw Workspace Auditor — read-only workspace health check: directory compliance, task PROGRESS.md health, memory-log gaps, knowledge-base index orphans, junk files. Zero-dependency Python script, graded report with fix suggestions.">
</p>

> Read-only workspace auditor for OpenClaw agents: scans directory compliance, task/PROGRESS.md health, memory-log gaps, knowledge-base index orphans and junk files; outputs a severity-graded report (🔴/🟡/🟢) with fix suggestions. Zero-dependency (Python stdlib only), never modifies a file.
> 工作区「质检员」：只读扫描健康度——目录合规、任务进度、记忆日志、知识库索引、垃圾文件，输出分级报告 + 修复建议，永不修改任何文件。

![license](https://img.shields.io/badge/license-MIT-green)
[![ClawHub downloads](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fclawhub.ai%2Fapi%2Fv1%2Fskills%2Fxiaoyaoclaw-workspace-auditor&query=skill.stats.downloads&label=ClawHub%20downloads&color=blue)](https://clawhub.ai/dtsola/skills/xiaoyaoclaw-workspace-auditor)

## Why

An OpenClaw agent workspace quietly degrades over time, and **you rarely notice**:
- 🗂️ **Messy root**: non-md files at the workspace root, arbitrary naming, missing standard directories
- 🧟 **Zombie tasks**: directories without PROGRESS.md, stale status, unfinished for weeks
- 🕳️ **Memory gaps**: no daily logs for days, missing MEMORY.md
- 🌑 **Knowledge black holes**: new files in `knowledge/` not in `data_structure.md` — the retriever can't find them
- 🗑️ **Junk pile**: stale files in `tmp/`, oversized files eating disk

Manual inspection is slow and never complete. This skill fixes it in one shot: **one zero-dependency script, 5 check categories, a graded report, and a fix suggestion for every finding.**

## Features

- 🩺 **Read-only by design**: never modifies/deletes/moves a file — fixes are executed only after you confirm (transparent red line)
- 🗂️ **5 check categories**: directory compliance (initializer) · task health (tracker PROGRESS.md) · memory health (daily logs) · knowledge-base health (kb-retriever index) · junk/temp files
- 📊 **Graded report**: 🔴 violation / 🟡 warning / 🟢 ok / ⏭️ skipped, each with a fix suggestion
- 🐍 **Zero dependency**: Python stdlib only — no third-party packages, no API keys, no network, data never leaves your machine
- 🪜 **Progressive dependency**: if a sibling skill isn't installed, its checks are skipped with a hint instead of false positives — install more, audit deeper
- 🖥️ **Cross-platform**: identical behavior on Windows & macOS (pure Python)
- 🔁 **Dual output**: Markdown report for humans, JSON for automation (cron-friendly)

## Install

```bash
# ClawHub (recommended)
clawhub install xiaoyaoclaw-workspace-auditor

# Or manually from GitHub
git clone https://github.com/dtsola/xiaoyaoclaw-workspace-auditor
# Put SKILL.md and scripts/ into your skills directory
```

## Usage

1. Put the skill into your OpenClaw skills directory
2. Tell your agent: **"audit the workspace"** / "health-check the workspace"
3. The agent runs the scan and gives you a graded report with fix suggestions

Or run the script directly:

```bash
python scripts/scan_workspace.py --report    # Markdown report (default)
python scripts/scan_workspace.py --json      # JSON output (for automation)
python scripts/scan_workspace.py --days 60   # custom stale threshold (default 30)
```

## 🚀 Quick Start (3 steps, 5 minutes)

### Step 1: Install

```bash
clawhub install xiaoyaoclaw-workspace-auditor
```

### Step 2: Run a scan

Tell your agent: **"audit the workspace"**. Within seconds you get a graded report — non-compliant directories, zombie tasks, knowledge-base black holes, stale tmp files.

### Step 3: Fix on your command

Every finding carries a 💡 fix suggestion. **The agent only suggests, never acts on its own** — it waits for your confirmation (or routes you to the right sibling skill).

### Daily habits

| Scenario | How |
|---|---|
| Weekly checkup | Say "audit the workspace" — done in 30s |
| Automation | Cron `scan_workspace.py --json` weekly, alert on anomalies |
| One category | Read the matching report section |
| Fix KB black holes | Rebuild index via kb-retriever's `build_index.py` |
| Clean tmp | Report lists stale files; agent cleans only after you confirm |

## vs. Manual Inspection

| | Manual browsing | **xiaoyaoclaw-workspace-auditor** |
|---|---|---|
| Coverage | Memory-based, always misses something | ✅ Full 5-category scan, deterministic rules |
| Consistency | Different result every time | ✅ Regex + path matching, reproducible |
| Effort | Browse then judge yourself | ✅ Graded report + fix suggestion per finding |
| Safety | Easy to delete the wrong thing | ✅ Read-only; deletion only after your confirm |
| Machine-readable | No | ✅ JSON output, cron-friendly |

## Structure

```text
xiaoyaoclaw-workspace-auditor/
├── SKILL.md                  # Main skill (triggers / workflow / red lines)
├── DESIGN.md                 # Design doc (check rules / degradation matrix)
├── scripts/
│   └── scan_workspace.py     # The only scanner (zero-dep, 5 categories, dual output)
├── assets/readme/hero.svg    # README cover
├── README.md / README.en.md  # Bilingual docs
└── LICENSE                   # MIT
```

## License

MIT © dtsola

## Customize

- **Thresholds**: `--days` (stale days, default 30), `--max-file` (MB, default 50)
- **Scope**: scans standard directories; add custom checks by extending the `Auditor` class in `scan_workspace.py` (one method per category)

## Sister Projects (The Five-Piece Set)

OpenClaw workspace autonomy — Home → Content → Status → Knowledge → **Health**:

| Piece | Role | Repo |
|---|---|---|
| 🏠 workspace-initializer | Home / directory standards | [dtsola/xiaoyaoclaw-workspace-initializer](https://github.com/dtsola/xiaoyaoclaw-workspace-initializer) |
| 🧠 memory-distill | Content / memory distillation | [dtsola/xiaoyaoclaw-memory-distill](https://github.com/dtsola/xiaoyaoclaw-memory-distill) |
| 📊 task-progress-tracker | Status / progress tracking | [dtsola/xiaoyaoclaw-task-progress-tracker](https://github.com/dtsola/xiaoyaoclaw-task-progress-tracker) |
| 📚 kb-retriever | Knowledge / KB retrieval | [dtsola/xiaoyaoclaw-kb-retriever](https://github.com/dtsola/xiaoyaoclaw-kb-retriever) |
| 🩺 **workspace-auditor** | **Health / workspace audit** | **dtsola/xiaoyaoclaw-workspace-auditor (this repo)** |

## 小遥Claw

🚀 Install AI assistants on your own computer: [https://www.yuque.com/dtsola/igp1aa/adcicbai2zlem0bz](https://www.yuque.com/dtsola/igp1aa/adcicbai2zlem0bz)

## Author

**dtsola** · Indie developer · [GitHub](https://github.com/dtsola)

## Community

<img src="./assets/readme/community-qr.png" width="160" alt="Community QR">
