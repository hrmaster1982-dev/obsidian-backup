---
tags: [skill, youtube, notebooklm, research, pipeline, obsidian]
created: 2026-06-04
location: ~/.claude/skills/yt-research-pipeline-skill/
trigger: /yt-research-pipeline
eval: 33/33 PASS
---

# yt-research-pipeline-skill

Pipeline nghiên cứu tự động: YouTube search → NotebookLM → phân tích AI → tài liệu phụ → lưu Obsidian.

## Trigger

```
/yt-research-pipeline <chủ đề nghiên cứu>
```

## Ví dụ

```
/yt-research-pipeline AI agent frameworks 2026
/yt-research-pipeline machine learning optimization
/yt-research-pipeline "prompt engineering best practices"
/yt-research-pipeline chủ đề tiếng Việt bất kỳ
```

## Pipeline 5 bước

```
1. YouTube Search   → 10 video (yt-dlp, 6 tháng gần nhất)
       ↓               title · channel · subs · views · V/Dk · URL
2. NotebookLM       → Tạo notebook mới + nạp tất cả URL
       ↓
3. Phân tích        → Câu trả lời theo đúng câu hỏi gốc của người dùng
       ↓
4. Hỏi người dùng   → [1] Flashcards [2] Infographic [3] Mind Map [4] Audio [5] Không
       ↓
5. Lưu Obsidian     → Research/<slug>.md  +  performance-reporter
```

## Chạy trực tiếp (CLI)

```bash
# Phase 1: tìm kiếm + notebook + phân tích + lưu Obsidian
python scripts/run_pipeline.py "chủ đề" --phase1

# Phase 1 + supplements không tương tác
python scripts/run_pipeline.py "chủ đề" --supplements flashcards,mindmap

# Phase 2: chỉ tạo supplements (cần notebook-id từ Phase 1)
python scripts/run_pipeline.py "chủ đề" --phase2 \
    --notebook-id abc-123 \
    --supplements flashcards,infographic,mindmap,audio

# Override câu hỏi phân tích
python scripts/run_pipeline.py "chủ đề" --phase1 \
    --analysis-question "So sánh chi tiết..."
```

## Supplement codes

| Code | Loại | File xuất |
|------|------|----------|
| `flashcards` | Thẻ ghi nhớ JSON | `{slug}-flashcards.json` |
| `infographic` | Hình ảnh PNG | `{slug}-infographic.png` |
| `mindmap` | Sơ đồ JSON | `{slug}-mindmap.json` |
| `audio` | Podcast MP3 | `{slug}-podcast.mp3` |

## Output bảng nguồn

```markdown
| # | Tiêu đề | Kênh | Đăng ký | Lượt xem | Thời lượng | Ngày đăng | V/Đk | URL |
```

**V/Đk ratio > 1.0x** = nội dung viral ngoài cộng đồng core của kênh — tín hiệu nội dung đáng đọc.

## Cấu hình (assets/config.json)

```json
{
  "python_exec": "C:/Users/Administrator/.../python.exe",
  "yt_search_script": "...youtube-search-skill/scripts/search.py",
  "notebooklm_script": "...agent-skill-creator/scripts/notebooklm_tool.py",
  "obsidian_vault": null,
  "obsidian_research_dir": "Research",
  "yt_search_months": 6,
  "yt_search_limit": 10,
  "notebooklm_language": "vi"
}
```

`obsidian_vault: null` → auto-detect từ Documents/ObsidianVault, Documents/Obsidian, v.v.

## Xử lý lỗi thường gặp

| Lỗi | Giải pháp |
|-----|----------|
| NotebookLM auth expired | `notebooklm login --browser-cookies edge` |
| YouTube URL bị từ chối | Bỏ qua tự động, tiếp tục URL tiếp theo |
| Audio timeout 300s | Dùng poll script riêng với timeout 720s |
| Obsidian vault not found | Lưu vào `exports/Research/` thay thế |
