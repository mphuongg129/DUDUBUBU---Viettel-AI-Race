# 🧠 Viettel AI Race - DUDUBUBUBU 🧩  
## 📄 Văn bản kỹ thuật (Extraction + QA Pipeline)

---

## 📘 Mô tả
Pipeline xử lý tự động **trích xuất nội dung PDF kỹ thuật và trả lời câu hỏi (QA)** bao gồm:
1. **Extraction**: đọc PDF, tách tiêu đề, bảng, hình ảnh → lưu thành Markdown.  
2. **QA**: đọc `question.csv`, kết hợp TF-IDF + BM25 + Semantic Embedding + CrossEncoder để tìm và chọn đáp án đúng.

Tất cả được tích hợp trong **`main.py`**, chạy end-to-end từ PDF đầu vào đến file kết quả `answer.md`.

---

## 📂 Cấu trúc thư mục

```
Viettel AI Race - DUDUBUBUBU/
│
├── main.py                     # Pipeline chính (Extract + QA)
├── requirements.txt            # Thư viện cần thiết
│
├── input/                      # Thư mục dữ liệu đầu vào
│   ├── question.csv
│   ├── Public427.pdf
│   ├── Public428.pdf
│   ├── ...
│   └── Public675.pdf
│
└── submissions/                # Kết quả đầu ra
    ├── Public_427/
    │   ├── main.md
    │   └── images/
    │       ├── image_1.png
    │       └── ...
    ├── Public_428/
    │   ├── main.md
    │   └── images/
    ├── ...
    ├── Public_675/
    │   ├── main.md
    │   └── images/
    │
    ├── answer.md               # Kết quả QA tổng hợp
    ├── pipeline.log            # Log quá trình chạy
    └── per_question_logs.json  # Ghi chi tiết điểm từng câu hỏi
```

---

## ⚙️ Cài đặt môi trường

### 1️⃣ Tạo và kích hoạt môi trường ảo
```bash
python -m venv .venv
.venv\Scripts\activate     # Windows
# hoặc
source .venv/bin/activate  # Linux / macOS
```

### 2️⃣ Cài đặt thư viện
```bash
pip install -r requirements.txt
```

---

## 📦 File `requirements.txt`

```text
# === Core NLP and Embedding ===
sentence-transformers==2.7.0
transformers==4.44.2
torch==2.2.2
numpy==1.26.4
scikit-learn==1.4.2
scipy==1.12.0

# === Text retrieval and BM25 ===
rank-bm25==0.2.2

# === PDF extraction & parsing ===
pdfplumber==0.11.3
PyMuPDF==1.24.3
camelot-py==0.11.0
pdfminer.six==20231228

# === Text / HTML / Markdown ===
beautifulsoup4==4.12.3
markdownify==0.13.1

# === Progress bar / Logging / System ===
tqdm==4.66.4
loguru==0.7.2
colorama==0.4.6

# === Pandas for CSV (questions file) ===
pandas==2.2.2

# === Optional (used for structured saving and JSON logs) ===
orjson==3.10.3

# === Utilities for environment / configs ===
python-dotenv==1.0.1
```

---

## 🚀 Cách chạy pipeline

```bash
python main.py
```

Sau khi chạy, toàn bộ output sẽ nằm trong thư mục `submissions/`.

---

## 📄 Output chính

| File / Folder | Mô tả |
|----------------|-------|
| `submissions/<PDF_NAME>/main.md` | Markdown nội dung trích từ PDF |
| `submissions/<PDF_NAME>/images/` | Ảnh trích từ PDF |
| `submissions/answer.md` | Kết quả QA theo định dạng `num_correct,answers` |
| `submissions/pipeline.log` | Nhật ký xử lý |
| `submissions/per_question_logs.json` | Chi tiết từng câu hỏi và điểm reranker |

---

## 🧠 Mô hình sử dụng

| Loại | Model | Mục đích |
|------|--------|----------|
| Embedding | `intfloat/multilingual-e5-small` | Semantic retrieval (đa ngôn ngữ) |
| Reranker | `cross-encoder/ms-marco-MiniLM-L-6-v2` | Xếp hạng lại các đoạn văn phù hợp |
| TF-IDF | `sklearn` | Truy xuất ngữ cảnh cơ bản |
| BM25 | `rank-bm25` | Weighted lexical retrieval |

---

## 💡 Ghi chú
- Thiết bị mặc định: `cpu`. Nếu có GPU → sửa trong `main.py`:
  ```python
  DEVICE = "cuda"
  ```
- Pipeline tương thích với nhiều file PDF trong cùng thư mục `input/`.
- Hỗ trợ markdown rõ ràng, giữ nguyên bảng (HTML table block), hình ảnh, và caption.

---

## ✅ Ví dụ đầu ra `answer.md`

```text
### TASK EXTRACT
# Public427
<markdown content>

### TASK QA
num_correct,answers
1,A
2,"A,B"
3,"A,B,C"
```

---

**📌 Tác giả:** Đội DUDUBUBUBU — Viettel AI Race 2025  
**📅 Phiên bản:** Final Submission — Technical Document Extraction + QA
