---
tags: [index, skills]
---

# Skills Index

Tất cả Agent Skills được cài tại `~/.claude/skills/` và lưu tại `skills/` trong project.

## Skills trong phiên này

| Skill | Trigger | Mục đích | Eval |
|-------|---------|---------|------|
| [[notebooklm-tool\|notebooklm_tool.py]] | CLI trực tiếp | Wrapper NotebookLM đầy đủ tính năng | — |
| [[performance-reporter-skill\|performance-reporter-skill]] | `/performance-reporter` | Báo cáo token + baseline | 32/32 |
| [[yt-research-pipeline-skill\|yt-research-pipeline-skill]] | `/yt-research-pipeline` | YouTube → NotebookLM → Obsidian | 33/33 |

## Skills từ trước

| Skill | Trigger | Mục đích |
|-------|---------|---------|
| youtube-search-skill | `/youtube-search` | Tìm video YouTube với V/Đk ratio |
| prepare-notebooklm-skill | `/prepare-notebooklm` | Tổng hợp research_data/ cho NotebookLM |

## Quy tắc vận hành

> **Mọi pipeline khi kết thúc phải chạy performance-reporter.**

```bash
"C:/Users/Administrator/AppData/Local/Programs/Python/Python311/python.exe" \
"C:/Users/Administrator/.claude/skills/performance-reporter-skill/scripts/report.py"
```

Xem chi tiết: [[operational-rules|Quy tắc vận hành]]
