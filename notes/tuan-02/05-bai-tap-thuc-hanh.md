# Bài Tập Thực Hành Tuần 2

1. Viết đủ **8-10 Competency Questions** theo mẫu bảng ở [02-competency-questions.md](02-competency-questions.md) (ít nhất 3 câu khác với ví dụ đã cho, tự nghĩ theo gợi ý tồn kho/năng suất/truy vết)
2. Từ danh sách CQ, liệt kê **toàn bộ class và property** cần có cho ontology — đối chiếu lại với 3 class đã có từ Tuần 1 (`Style`, `SKU`, `Order`): còn thiếu gì? (gợi ý: `WorkOrder`, `BOM`, `Fabric`, `Trim`, `QCEvent`, `DefectCode`)
3. Tải và import `epcis-ontology.ttl` + `cbv-ontology.ttl` vào file Protégé của bạn (Active Ontology → Import) — làm theo [04-import-ontology-ngoai.md](04-import-ontology-ngoai.md)
4. Tạo 1 instance `epcis:TransformationEvent` mẫu, biểu diễn "SKU001 được cắt từ lô vải X" theo đúng ví dụ ở file 04
5. Chạy reasoner sau khi import — xác nhận ontology gộp (nội bộ + EPCIS) vẫn consistent

## Nộp lại

Báo cho mình: danh sách 8-10 CQ hoàn chỉnh, danh sách class/property còn thiếu bạn tự suy ra được, và xác nhận import EPCIS + tạo event mẫu thành công (không cần chép lại toàn bộ, chỉ cần tóm tắt).

---
*Tiếp theo: [06 — FAQ](06-faq.md)*
