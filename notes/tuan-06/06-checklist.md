# Checklist Hoàn Thành Tuần 6 — & Tổng Kết Toàn Khóa

## Checklist Tuần 6

- [ ] Hiểu rõ SHACL (Closed World) khác OWL reasoner (Open World) ở mục đích gì, dùng khi nào
- [ ] Viết được ít nhất 3 SHACL shape, chạy `pyshacl` validate thành công, phát hiện đúng lỗi khi cố tình thiếu dữ liệu
- [ ] Dùng được ROBOT để chuẩn hóa format và so sánh (`diff`) 2 version ontology
- [ ] Có quy trình version control TBox qua git rõ ràng (TBox commit, ABox không commit — chỉ commit script sinh ABox)
- [ ] Hoàn thành bảng Coverage Competency Questions cho toàn bộ CQ Tuần 2
- [ ] Hoàn thiện báo cáo so sánh Retrieval quality (GraphRAG vs vector-only) với số liệu cụ thể

## Sản phẩm cuối của toàn bộ 6 tuần

Nếu checklist Tuần 1-6 đều hoàn thành, bạn có:

1. **Một ontology OWL hoàn chỉnh** cho domain sản xuất may mặc (Order/Style/SKU/BOM/WorkOrder/QC), qua được reasoner consistency check, có tài liệu Competency Questions + scope note (Tuần 1-2)
2. **Pipeline code hóa đầy đủ** — từ `rdflib`/`owlready2` (Tuần 3) tới triple store sống có API (Tuần 4)
3. **Pipeline AI tích hợp thật** — LLM extraction + GraphRAG trả lời câu hỏi nghiệp vụ, grounded trên KG (Tuần 5)
4. **Lớp validate + versioning** đảm bảo chất lượng dữ liệu và khả năng bảo trì lâu dài (Tuần 6)
5. **Lớp traceability event** (cắt → may → KCS → đóng gói → xuất hàng) mô hình theo chuẩn GS1 EPCIS/CBV đã tái dùng từ Tuần 2

## Bước tiếp theo sau khóa học

- Mở rộng scope ontology sang domain khác (tồn kho, nhân sự) — lặp lại đúng quy trình Tuần 1-2 (methodology, CQ, reuse check) cho domain mới
- Đưa pipeline Tuần 3-5 từ "chạy script thủ công" sang tự động hóa (cron job, CI/CD) thật sự cho production
- Nếu triển khai production thật, cân nhắc lại các quyết định hạ tầng đã để ngỏ ở Tuần 4 (managed triple store, bảo mật API, v.v.)

---
*Xem lại: [00 — Tổng quan & mục lục](00-tong-quan.md) · Toàn khóa: [Giai đoạn 0](../giai-doan-0.md)*
