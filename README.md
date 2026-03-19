# Reverse Engineering Skills

Agent skills for binary analysis and reverse engineering. Works with Claude Code, Cursor, Codex, and other agents that support the [Agent Skills](https://agentskills.io/) format.

## Install

```bash
npx skills add Q7h2q9/reverse-skills
```

## Available Skills

| Skill | Description |
|-------|-------------|
| **rev-symbol** | Restore function symbols by analyzing code patterns, strings, constants, and cross-references |
| **rev-struct** | Reconstruct data structures by analyzing memory access patterns across functions |
| **jadx-analyze** | Decompile and analyze Android APK/DEX/AAB files using JADX |

## Prerequisites

- **rev-struct / rev-symbol**: Require IDA export data (`decompile/`, `strings.txt`, etc.) in the working directory
- **jadx-analyze**: Requires [JADX](https://github.com/skylot/jadx) installed locally

## License

MIT
