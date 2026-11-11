# Ghép Thành Pipeline GraphRAG Hoàn Chỉnh

```
Log sản xuất (text tự do)
        ↓ (LLM extraction — file 04)
Triple mới → ghi vào triple store (Tuần 4)
        ↓
Người dùng hỏi câu hỏi nghiệp vụ
        ↓ (LLM chọn tool / sinh SPARQL — file 03)
Query chạy trên triple store → kết quả chính xác
        ↓
LLM nhận kết quả, sinh câu trả lời tiếng Việt tự nhiên
        ↓
Trả lời người dùng — GROUNDED trên dữ liệu thật, không hallucinate
```

## Khung code tổng hợp (rút gọn)

```python
def handle_user_question(question: str):
    # 1. LLM chọn tool phù hợp (Cách B, file 03)
    tool_call = llm.chat(question, tools=TOOLS)

    # 2. Thực thi tool — gọi API Tuần 4
    result = execute_tool(tool_call)

    # 3. LLM tổng hợp câu trả lời tự nhiên từ kết quả thật
    answer = llm.chat(
        f"Dựa trên dữ liệu sau, trả lời câu hỏi '{question}' bằng tiếng Việt: {result}"
    )
    return answer


def ingest_production_log(raw_text: str):
    # 1. LLM trích xuất có cấu trúc (file 04)
    extracted = llm.extract(raw_text, schema=QC_SCHEMA)

    # 2. Map sang triple, ghi vào graph
    add_triples_from_extraction(extracted)

    # 3. (Tùy chọn) chạy lại kiểm tra consistency
    check_consistency()
```

Hai hàm này là 2 "chiều" của kiến trúc 3 lớp: `ingest_production_log` đưa dữ liệu thật **vào** KG, `handle_user_question` lấy tri thức **ra** từ KG để trả lời.

## Khi nào chạy mỗi hàm

- `ingest_production_log`: chạy mỗi khi có log/báo cáo mới (có thể định kỳ theo batch, hoặc realtime nếu tích hợp trực tiếp vào quy trình nhập liệu)
- `handle_user_question`: chạy mỗi khi người dùng/agent cần trả lời — không phụ thuộc vào khi nào dữ liệu được ingest, miễn KG đã có đủ thông tin cần thiết tại thời điểm hỏi

---
*Tiếp theo: [06 — So sánh định lượng](06-so-sanh-dinh-luong.md)*
