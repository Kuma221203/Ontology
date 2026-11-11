# GraphRAG vs Vector RAG — Khác Nhau Ở Bước Truy Xuất

| | Vector RAG (quen thuộc) | GraphRAG |
|---|---|---|
| Lưu trữ | Embedding của đoạn text | Triple trong knowledge graph |
| Truy xuất | Tìm đoạn **tương đồng ngữ nghĩa** (cosine similarity) | Query **chính xác** theo cấu trúc (SPARQL) |
| Phù hợp | Câu hỏi mở, văn bản tự do, tài liệu dài | Câu hỏi có cấu trúc rõ (số liệu, quan hệ, điều kiện logic) |
| Nhược điểm | Có thể lấy nhầm đoạn "gần giống" nhưng sai ý — không có khái niệm "đúng/sai tuyệt đối" | Cần câu hỏi dịch được sang truy vấn có cấu trúc — không linh hoạt với câu hỏi hoàn toàn mở |
| Ví dụ câu hỏi hợp | "Quy trình xử lý khiếu nại khách hàng là gì?" | "SKU nào đang trễ tiến độ ở công đoạn may?" |

## Vì sao không chọn 1 trong 2, mà kết hợp

Trong thực tế, hệ thống tốt **kết hợp cả hai**:
- **Vector RAG** cho câu hỏi mở, tra trong tài liệu dạng văn bản (SOP, hướng dẫn, email, báo cáo dài) — nơi khó/không đáng hình thức hóa hết thành ontology
- **GraphRAG** cho câu hỏi cần dữ liệu chính xác, có cấu trúc, có thể tính toán/đếm/so sánh — tra trong KG

Một hệ thống agent thực tế thường có **cả 2 loại "tool"**: một tool tìm kiếm vector trên tài liệu, một tool query trên KG — LLM tự quyết định gọi tool nào dựa trên dạng câu hỏi.

## Minh họa bằng chính Competency Questions đã viết

| Competency Question (Tuần 2) | Vector RAG hay GraphRAG? |
|---|---|
| "SKU nào đang trễ tiến độ ở công đoạn may?" | **GraphRAG** — cần đếm/so sánh ngày chính xác |
| "Quy trình xử lý đơn hàng gấp (rush order) là gì?" (giả sử có SOP dạng văn bản) | **Vector RAG** — tra trong tài liệu hướng dẫn |
| "Công đoạn nào gây nhiều lỗi KCS nhất trong tháng?" | **GraphRAG** — cần `COUNT`/`GROUP BY` |

---
*Tiếp theo: [03 — LLM sinh truy vấn có cấu trúc](03-llm-sinh-sparql.md)*
