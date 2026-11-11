# Nhắc Lại Kiến Trúc 3 Lớp

```
Người dùng hỏi (tiếng Việt, tự nhiên)
            ↓
        LLM / Agent
            ↓↑  (query có cấu trúc  ↕  trả về sự thật chính xác)
   Ontology + Knowledge Graph
            ↓↑
   Dữ liệu thực tế (ERP, MES, log sản xuất...)
```

Đây là sơ đồ đã bàn khi thảo luận ví dụ "xuống PO cho Style A123" — tuần này code hóa cụ thể **2 mũi tên ở giữa**:

- **Mũi tên AI → Ontology/KG**: LLM cần biết cách "hỏi" graph một cách chính xác — xem [03-llm-sinh-sparql.md](03-llm-sinh-sparql.md)
- **Mũi tên Dữ liệu thật → Ontology/KG**: dữ liệu text tự do (log sản xuất, báo cáo QC) cần được LLM "dịch" thành triple đúng vocabulary — xem [04-llm-extraction.md](04-llm-extraction.md)

Hai mũi tên này ghép lại thành một pipeline hoàn chỉnh ở [05-pipeline-hoan-chinh.md](05-pipeline-hoan-chinh.md).

## Vì sao tuần này nằm sau Tuần 1-4, không làm trước

Pipeline này **phụ thuộc hoàn toàn** vào những gì đã xây:
- Ontology (Tuần 1-2) định nghĩa đúng vocabulary để LLM extract dữ liệu vào
- Triple store + API (Tuần 3-4) là nơi dữ liệu thực sự "sống" và được truy vấn

Không có nền tảng đó, một pipeline "AI + ontology" chỉ là demo rời rạc, không grounded trên dữ liệu thật — đây là lý do lộ trình đặt Tuần 5 ở vị trí này, không sớm hơn.

---
*Tiếp theo: [02 — GraphRAG vs Vector RAG](02-graphrag-vs-vector-rag.md)*
