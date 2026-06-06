---
tags: [config, rules, pipeline, performance]
created: 2026-06-04
source: CLAUDE.md
---

# Quy tắc vận hành Pipeline

## Quy tắc bắt buộc

> **Mọi pipeline khi kết thúc PHẢI kích hoạt performance-reporter.**

```bash
"C:/Users/Administrator/AppData/Local/Programs/Python/Python311/python.exe" \
"C:/Users/Administrator/.claude/skills/performance-reporter-skill/scripts/report.py"
```

## Logic đánh giá kết quả

| Tình huống | Hành động |
|-----------|----------|
| Token tiêu thụ **thấp hơn** baseline | In banner `╔══╗` chúc mừng tối ưu hóa |
| Token tiêu thụ **cao hơn** baseline | Ghi nhận + đề xuất cách cải thiện |
| Sau pipeline lớn | Cập nhật baseline: `--update-baseline` |

## Cách cập nhật baseline

```bash
python scripts/report.py --update-baseline
```

## Stop Hook (tự động)

Stop hook trong `~/.claude/settings.json` tự động chạy reporter sau MỖI lần Claude dừng phản hồi — không cần gọi thủ công.

Kiểm tra trạng thái:
```bash
python ~/.claude/skills/performance-reporter-skill/scripts/install_hook.py --status
```

## Gợi ý tối ưu token

1. Dùng Skills có sẵn thay vì mô tả lại quy trình
2. Giữ context ngắn gọn
3. Tận dụng cache bằng cách tái sử dụng Skills
4. Cache Read Tokens tăng = Skills đang hoạt động hiệu quả
