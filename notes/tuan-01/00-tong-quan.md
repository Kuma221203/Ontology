# Tuần 1 — Tổng Quan & Mục Lục

> Nền tảng RDF/OWL + Protégé. Mỗi mục dưới đây là một file riêng, đào sâu một chủ đề — đọc theo thứ tự, mỗi file có link "Tiếp theo" ở cuối.

## Mục tiêu tuần này

Hiểu đủ RDF/OWL để tự xây một ontology nhỏ có khả năng suy luận thật sự (không chỉ là một sơ đồ phân loại tĩnh), và làm quen thành thạo giao diện Protégé — công cụ sẽ dùng xuyên suốt các tuần đầu trước khi chuyển sang lập trình bằng Python ở Tuần 3.

## Mục lục

| # | File | Nội dung |
|---|---|---|
| 1 | [01-rdf-triple.md](01-rdf-triple.md) | RDF triple, các serialization (Turtle/N-Triples/RDF-XML/JSON-LD), vì sao graph truy vấn nhanh dù không có schema cứng |
| 2 | [02-iri-namespace.md](02-iri-namespace.md) | IRI, namespace, prefix, cách chọn namespace cho project, vì sao tránh được xung đột tên |
| 3 | [03-class-hierarchy.md](03-class-hierarchy.md) | Class vs Individual, multiple inheritance, disjoint classes, class expression |
| 4 | [04-object-vs-data-property.md](04-object-vs-data-property.md) | Object vs Data property, toàn bộ đặc tính logic (Functional, Inverse, Transitive...), property chain |
| 5 | [05-open-world-assumption.md](05-open-world-assumption.md) | **Phần quan trọng nhất** — Open World Assumption, Unique Name Assumption, so sánh với tư duy SQL |
| 6 | [06-restriction-cardinality.md](06-restriction-cardinality.md) | Restriction nâng cao, qualified cardinality, SubClassOf vs EquivalentClasses, defined class |
| 7 | [07-cai-dat-protege.md](07-cai-dat-protege.md) | Cài đặt, tour giao diện Protégé, DL Query, reasoner |
| 8 | [08-bai-tap-thuc-hanh.md](08-bai-tap-thuc-hanh.md) | Bài tập 16 bước, 3 phần (A: cơ bản, B: restriction/OWA, C: disjoint/defined class/UNA) |
| 9 | [09-faq.md](09-faq.md) | Câu hỏi thường gặp |
| 10 | [10-checklist.md](10-checklist.md) | Checklist tổng để tự kiểm tra trước khi báo hoàn thành |

## Gợi ý thứ tự đọc

Đọc 01 → 06 trước (lý thuyết), vừa đọc vừa cài Protégé ở 07, rồi làm bài tập 08 (có link tham chiếu ngược lại từng mục lý thuyết liên quan). Dùng 09 khi vướng, dùng 10 để tự chốt trước khi báo cáo kết quả.

---
*Trước đó: [Giai đoạn 0](../giai-doan-0.md) · Bắt đầu: [01 — RDF Triple](01-rdf-triple.md)*
