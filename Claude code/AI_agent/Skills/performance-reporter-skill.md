---
tags: [skill, performance, token, monitoring, hook]
created: 2026-06-04
location: ~/.claude/skills/performance-reporter-skill/
trigger: /performance-reporter
eval: 32/32 PASS
---

# performance-reporter-skill

Tự động đọc JSONL transcript sau mỗi phiên Claude Code, trích xuất metrics token và in bảng so sánh Markdown với baseline đã lưu.

## Trigger

```
/performance-reporter
/performance-reporter --update-baseline
/performance-reporter --reset
```

Hoặc chạy tự động qua Stop hook sau mỗi lần Claude dừng.

## Cài đặt Stop Hook (một lần)

```bash
cd ~/.claude/skills/performance-reporter-skill
python scripts/install_hook.py
```

Thêm vào `~/.claude/settings.json`:
```json
{
  "hooks": {
    "Stop": [{ "hooks": [{ "type": "command",
      "command": "\"C:\\...\\python.exe\" \"C:\\...\\report.py\""
    }]}]
  }
}
```

## Metrics theo dõi

| Metric | Nguồn JSONL |
|--------|------------|
| Input Tokens | `message.usage.input_tokens` |
| Output Tokens | `message.usage.output_tokens` |
| Cache Read Tokens | `message.usage.cache_read_input_tokens` |
| Cache Write Tokens | `message.usage.cache_creation_input_tokens` |
| Commands | Số `tool_use` blocks trong assistant content |
| Success Rate | `tool_result.is_error == false` / tổng |
| Est. Cost | Theo giá Sonnet 4.6 |

## Output mẫu

```
## 📊 Performance Report — 2026-06-04 14:46

| Metric              | Baseline (14:43)  | This Run | Delta   |      |
|---------------------|-------------------|----------|---------|------|
| 🔢 Input Tokens     | 1.1K              | 1.2K     | +3.0%   | ↑ 🔴 |
| ⚡ Cache Read Tokens | 18.95M            | 42.73M   | +125.6% | ↑ 🟢 |
| ✅ Success Rate     | 88.2%             | 89.5%    | +1.3pp  | ↑ 🟢 |
| 💰 Est. Cost (USD)  | $10.05            | $21.09   | +109.8% | ↑ 🔴 |
```

### Banner chúc mừng (khi token giảm)
```
╔══════════════════════════════════════════════════════════════╗
║      🎉  CHÚC MỪNG — HỆ THỐNG ĐÃ ĐƯỢC TỐI ƯU HÓA!  🎉      ║
╠══════════════════════════════════════════════════════════════╣
║  💰  Tiết kiệm: $2.96  (23.2% so với baseline)               ║
║  🚀  Skills đang hoạt động hiệu quả — tiếp tục phát huy!     ║
╚══════════════════════════════════════════════════════════════╝
```

## Cấu hình (assets/config.json)

```json
{
  "pricing": {
    "input_per_million": 3.00,
    "output_per_million": 15.00,
    "cache_write_per_million": 3.75,
    "cache_read_per_million": 0.30
  }
}
```

Cập nhật pricing khi dùng model khác (Opus, Haiku).

## Lệnh thường dùng

```bash
# Xem báo cáo
python scripts/report.py

# Cập nhật baseline
python scripts/report.py --update-baseline

# Xóa baseline
python scripts/report.py --reset

# Kiểm tra hook
python scripts/install_hook.py --status
```
