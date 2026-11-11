# Bài Tập Thực Hành Tuần 3

1. Viết script Python dùng `rdflib`, load lại file `.ttl` đã xuất từ Protégé (Tuần 1-2, gồm cả phần EPCIS đã import)
2. Tạo file CSV mẫu (10-15 dòng) giả lập dữ liệu Order/Style/SKU, viết script populate instance từ CSV đó vào graph (theo mẫu [02-rdflib.md](02-rdflib.md))
3. Dùng `owlready2`, load cùng ontology, chạy `sync_reasoner_hermit()` từ code — xác nhận không có lỗi (giống Protégé Tuần 1)
4. Viết 2-3 câu SPARQL trả lời đúng Competency Questions đã viết ở Tuần 2 (dùng mẫu `FILTER`, `OPTIONAL`, `GROUP BY` ở [04-sparql.md](04-sparql.md))
5. Serialize kết quả ra file `.ttl` mới, mở lại bằng Protégé để xác nhận dữ liệu populate đúng

## Nộp lại

Báo cho mình: script populate chạy thành công (bao nhiêu triple sinh ra), kết quả reasoner từ `owlready2`, và kết quả của 2-3 câu SPARQL (đối chiếu có đúng ý Competency Question hay không).

---
*Tiếp theo: [06 — FAQ](06-faq.md)*
