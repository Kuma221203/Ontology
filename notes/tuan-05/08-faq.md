# Câu Hỏi Thường Gặp — Tuần 5

**Hỏi:** Có nên bỏ hẳn vector RAG, chỉ dùng GraphRAG không?
**Đáp:** Không — dùng GraphRAG cho dữ liệu có cấu trúc (số liệu, quan hệ), giữ vector RAG cho tài liệu dạng văn bản dài (SOP, hướng dẫn, email) mà khó hình thức hóa hết thành ontology. Xem [02-graphrag-vs-vector-rag.md](02-graphrag-vs-vector-rag.md).

**Hỏi:** Nếu LLM trích xuất sai (gán nhầm defect cho sai SKU), ontology có tự phát hiện không?
**Đáp:** Một phần — nếu dữ liệu sai dẫn tới vi phạm ràng buộc logic đã khai (Functional, disjoint...), reasoner sẽ bắt được (xem [04-llm-extraction.md](04-llm-extraction.md)). Nhưng sai sót "hợp lý về logic nhưng sai về thực tế" (gán đúng SKU tồn tại nhưng không phải SKU thực sự có lỗi) thì ontology không tự phát hiện — cần review/validate thêm ở tầng khác (SHACL Tuần 6 phần nào giúp, nhưng không toàn diện).

**Hỏi:** Cách A (LLM tự sinh SPARQL) có bao giờ nên dùng không, hay luôn chọn Cách B?
**Đáp:** Cách B nên là mặc định cho mọi Competency Question đã biết trước. Cách A chỉ hữu ích khi cần trả lời câu hỏi hoàn toàn mới chưa có tool — ví dụ một "chế độ khám phá" cho người dùng nâng cao, có validation chặt kèm theo. Không nên là đường đi chính cho hệ thống production.

**Hỏi:** Pipeline ingest log có cần chạy real-time không, hay batch định kỳ là đủ?
**Đáp:** Tùy mức độ nhạy thời gian của loại câu hỏi. Với domain này, batch định kỳ (ví dụ mỗi giờ hoặc mỗi ca) thường đủ cho hầu hết Competency Questions — không cần over-engineer thành real-time streaming ngay từ đầu.

---
*Tiếp theo: [09 — Checklist hoàn thành](09-checklist.md)*
