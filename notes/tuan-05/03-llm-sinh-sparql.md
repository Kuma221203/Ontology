# LLM Dịch Câu Hỏi Tự Nhiên Sang Truy Vấn Có Cấu Trúc

Hai cách tiếp cận cho mũi tên "AI → Ontology/KG":

## Cách A — LLM tự sinh SPARQL

```python
system_prompt = """
Bạn là trợ lý dịch câu hỏi tiếng Việt sang SPARQL.
Ontology có các class: Style, SKU, Order, WorkOrder...
Property: belongsToStyle, hasSizeLabel, dueDate, currentStage...
Chỉ trả về câu SPARQL, không giải thích.
"""
# Gọi LLM với system_prompt + câu hỏi người dùng → nhận SPARQL → chạy qua endpoint Tuần 4
```

**Ưu điểm:** linh hoạt, trả lời được cả câu hỏi chưa lường trước (không cần viết sẵn tool cho từng câu).
**Nhược điểm:** rủi ro sai cú pháp SPARQL, rủi ro LLM "bịa" ra property/class không tồn tại trong ontology (hallucinate vocabulary), rủi ro injection nếu không kiểm soát.

## Cách B — Function calling / tool use (khuyến nghị)

```python
tools = [{
    "name": "get_skus_by_style",
    "description": "Lấy danh sách SKU thuộc một Style",
    "parameters": {"style_id": "string"}
}]
# LLM chỉ chọn tool + điền tham số, KHÔNG tự viết SPARQL
# Code đã viết sẵn (API Tuần 4) thực thi query cố định, an toàn
```

**Ưu điểm:** an toàn hơn nhiều — SPARQL không do LLM tự bịa, mỗi tool đã được test kỹ, dễ kiểm soát phạm vi truy cập (LLM chỉ gọi được đúng tập tool đã expose, không tự ý query bất kỳ phần nào của graph).
**Nhược điểm:** chỉ trả lời được câu hỏi đã có tool tương ứng — câu hỏi hoàn toàn mới, chưa viết tool, sẽ không trả lời được cho tới khi có người viết thêm tool.

## Khuyến nghị cho project này

**Bắt đầu với Cách B** — mỗi tool chính là một Competency Question đã viết ở Tuần 2, bọc bằng API đã viết ở Tuần 4 ([05-api-layer.md](../tuan-04/05-api-layer.md)). An toàn hơn nhiều vì SPARQL không do LLM tự bịa, tránh injection hoặc query sai làm sập hệ thống.

Cách A chỉ nên cân nhắc khi:
- Đã có validation chặt (kiểm tra SPARQL sinh ra hợp lệ trước khi chạy, giới hạn độ phức tạp query)
- Cần trả lời câu hỏi rất đa dạng, không thể liệt kê hết thành tool cố định trước

## Minh họa luồng Cách B hoàn chỉnh

```
Người dùng: "Style A123 đã sẵn sàng sản xuất chưa?"
        ↓
LLM chọn tool: get_incomplete_skus(style_id="StyleA123")
        ↓
Tool gọi API Tuần 4 → API chạy SPARQL cố định đã viết sẵn → trả JSON
        ↓
LLM nhận kết quả JSON, sinh câu trả lời tiếng Việt tự nhiên
```

---
*Tiếp theo: [04 — LLM trích xuất dữ liệu](04-llm-extraction.md)*
