# LLM Trích Xuất Dữ Liệu Từ Văn Bản Tự Do, Populate KG

## Tình huống

Một log KCS viết tự do: *"SKU001 bị đứt chỉ ở công đoạn may, phát hiện lúc kiểm cuối chuyền."* — cần biến thành triple có cấu trúc, đúng ontology đã thiết kế.

## Dùng structured output để ép LLM trả về đúng schema

```python
extraction_prompt = f"""
Trích xuất thông tin lỗi QC từ văn bản sau thành JSON đúng schema:
{{"sku": "...", "defect_type": "...", "stage": "...", "checkpoint": "..."}}

Văn bản: "{raw_qc_log_text}"
"""
# Gọi LLM với response_format=json_schema (structured output) → nhận JSON có cấu trúc đảm bảo đúng field
```

## Map JSON sang triple theo đúng ontology

```python
from rdflib import Namespace, Literal, RDF

NM = Namespace("http://nhamay.vn/ontology/sanxuat#")

extracted = {"sku": "SKU001", "defect_type": "DutChi", "stage": "sewing", "checkpoint": "end-of-line"}

g.add((NM[extracted["sku"]], NM.hasDefect, NM[extracted["defect_type"]]))
g.add((NM[extracted["defect_type"]], NM.occursAtStage, Literal(extracted["stage"])))
```

## Điểm mấu chốt — vì sao schema phải khớp chính xác ontology

Schema JSON trong prompt **phải khớp chính xác** vocabulary của ontology (tên class/property đã thiết kế ở Tuần 1-2) — đây là lý do ontology đóng vai trò **"hợp đồng ngôn ngữ"** đã nói khi bàn về tích hợp AI trước đó trong hội thoại: đảm bảo LLM luôn sinh ra dữ liệu nhất quán, map thẳng được vào KG mà không cần "dịch" thủ công hay viết thêm tầng chuyển đổi.

## Kiểm tra chất lượng trích xuất bằng chính ontology

Sau khi ghi triple mới vào graph, chạy lại reasoner (hoặc ít nhất kiểm tra 1 query SPARQL) — nếu dữ liệu trích xuất sai dẫn tới vi phạm ràng buộc logic đã khai (ví dụ `Functional`, `disjoint` từ Tuần 1), reasoner sẽ bắt được ngay. Đây là lợi ích thực tế của việc có ontology chặt chẽ phía sau: nó làm **lớp kiểm tra tự động** cho chính kết quả trích xuất của LLM, không chỉ là nơi lưu trữ thụ động.

---
*Tiếp theo: [05 — Pipeline hoàn chỉnh](05-pipeline-hoan-chinh.md)*
