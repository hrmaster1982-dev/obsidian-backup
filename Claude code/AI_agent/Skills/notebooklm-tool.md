---
tags: [skill, tool, notebooklm, python, cli]
created: 2026-06-04
file: scripts/notebooklm_tool.py
---

# notebooklm_tool.py — CLI Wrapper NotebookLM

Công cụ Python dòng lệnh bao bọc thư viện `notebooklm-py`, cho phép tự động hóa toàn bộ quy trình làm việc với Google NotebookLM.

## Cài đặt

```bash
pip install "notebooklm-py[browser]"
playwright install chromium
notebooklm login --browser-cookies edge   # hoặc chrome
```

## Cú pháp

```bash
python notebooklm_tool.py <lệnh> [tùy chọn]
```

## Lệnh Notebooks

```bash
# Tạo notebook mới
python notebooklm_tool.py create --title "Tên notebook"

# Liệt kê tất cả
python notebooklm_tool.py list

# Xem chi tiết
python notebooklm_tool.py get <notebook_id>

# Đổi tên
python notebooklm_tool.py rename <notebook_id> "Tên mới"

# Xóa
python notebooklm_tool.py delete <notebook_id> --yes
```

## Lệnh Sources

```bash
# Thêm URL (YouTube tự động nhận diện)
python notebooklm_tool.py add-source <nb_id> --url https://youtube.com/watch?v=xxx

# Thêm văn bản
python notebooklm_tool.py add-source <nb_id> --title "Tiêu đề" --text "Nội dung..."

# Thêm file (PDF, DOCX, TXT, Markdown, EPUB, audio, ảnh)
python notebooklm_tool.py add-source <nb_id> --file ./document.pdf
```

## Phân tích

```bash
python notebooklm_tool.py analyze <nb_id> --question "Câu hỏi phân tích"
```

## Tạo sản phẩm đầu ra

```bash
# Audio podcast (deep-dive/brief/critique/debate)
python notebooklm_tool.py generate <nb_id> audio --output podcast.mp3 --format deep-dive

# Sơ đồ tư duy
python notebooklm_tool.py generate <nb_id> mind-map --output map.json

# Thẻ ghi nhớ
python notebooklm_tool.py generate <nb_id> flashcards --quantity standard --difficulty medium

# Infographic
python notebooklm_tool.py generate <nb_id> infographic --orientation portrait --style professional

# Báo cáo
python notebooklm_tool.py generate <nb_id> report --format briefing

# Quiz
python notebooklm_tool.py generate <nb_id> quiz

# Slide
python notebooklm_tool.py generate <nb_id> slides --output slides.pdf
```

## Pipeline đầy đủ (một lệnh)

```bash
python notebooklm_tool.py pipeline \
  --title "Dự án AI" \
  --urls https://youtube.com/... https://arxiv.org/... \
  --files ./report.pdf \
  --analyze "Câu hỏi phân tích" \
  --generate audio mind-map flashcards \
  --output-dir ./results \
  --language vi
```

## Enum values (v0.6.0)

| Loại | Giá trị |
|------|---------|
| AudioFormat | `deep-dive` `brief` `critique` `debate` |
| AudioLength | `short` `default` `long` |
| InfographicOrientation | `portrait` `landscape` `square` |
| InfographicDetail | `concise` `standard` `detailed` |
| InfographicStyle | `auto` `sketch` `professional` `bento` `editorial` `scientific` |
| QuizQuantity | `fewer` `standard` |
| QuizDifficulty | `easy` `medium` `hard` |
| ReportFormat | `briefing` `study-guide` `blog` `custom` |

## Ghi chú

- Auth hết hạn → chạy `notebooklm login --browser-cookies edge`
- YouTube URL dài (>1h) có thể bị từ chối (thiếu transcript)
- `wait=True` mặc định cho add-source — đợi xử lý xong mới trả về
- Audio generation timeout sau 5 phút → dùng cơ chế poll riêng
