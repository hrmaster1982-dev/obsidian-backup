---
tags: [config, setup, environment]
created: 2026-06-04
---

# Cấu hình dự án agent-skill-creator

## Thông tin cơ bản

| Mục | Giá trị |
|-----|--------|
| Project dir | `C:\Claude_Workspace\agent-skill-creator` |
| GitHub repo | https://github.com/hrmaster1982-dev/agent-skill-creator |
| Branch | `main` |
| Last commit | `0047d8f` — 48 files, 7.348 insertions |

## Python Environment

```
Python: C:\Users\Administrator\AppData\Local\Programs\Python\Python311\python.exe
Version: 3.11.9
```

**Packages đã cài:**
- `notebooklm-py[browser]` v0.6.0
- `playwright` v1.60.0
- `yt-dlp` (youtube-search-skill)
- `openpyxl` (Excel export)
- `rookiepy` (browser cookie extraction)

## NotebookLM Authentication

```bash
# Đăng nhập lần đầu hoặc khi hết hạn
notebooklm login --browser-cookies edge

# Nếu Edge không hoạt động
notebooklm login   # mở Playwright browser

# File auth
~/.notebooklm/profiles/default/storage_state.json
```

**Tài khoản**: hrmaster1982@gmail.com

## Project Settings (.claude/settings.json)

```json
{
  "env": {
    "PYTHON_EXEC": "C:/Users/Administrator/AppData/Local/Programs/Python/Python311/python.exe",
    "PYTHONUTF8": "1",
    "PYTHONIOENCODING": "utf-8"
  }
}
```

## Global Settings (~/.claude/settings.json)

```json
{
  "theme": "dark",
  "hooks": {
    "Stop": [{
      "hooks": [{
        "type": "command",
        "command": "\"C:\\...\\python.exe\" \"C:\\...\\performance-reporter-skill\\scripts\\report.py\""
      }]
    }]
  }
}
```

## Skills đã cài (~/.claude/skills/)

```
~/.claude/skills/
├── agent-skill-creator/        # Factory tạo skill
├── youtube-search-skill/       # Tìm kiếm YouTube
├── prepare-notebooklm-skill/   # Tổng hợp tài liệu
├── performance-reporter-skill/ # Báo cáo token (Stop hook)
└── yt-research-pipeline-skill/ # Pipeline nghiên cứu
```

## Cấu trúc project

```
agent-skill-creator/
├── .claude/settings.json       # Env vars
├── CLAUDE.md                   # Quy tắc vận hành
├── scripts/
│   ├── notebooklm_tool.py      # CLI wrapper NotebookLM
│   ├── notebooklm_demo.py      # Demo usage
│   └── notebooklm_requirements.txt
├── skills/                     # Backup skills
│   ├── performance-reporter-skill/
│   └── yt-research-pipeline-skill/
└── exports/
    ├── Research/               # Markdown research files
    ├── ObsidianVault/          # This vault
    └── notebooklm_ai_agent/    # NotebookLM artifacts
```

## GitHub CLI

```bash
# Đã cài qua winget
gh --version  # 2.93.0

# Auth
gh auth status  # hrmaster1982-dev

# Push
git push  # → hrmaster1982-dev/agent-skill-creator
```
