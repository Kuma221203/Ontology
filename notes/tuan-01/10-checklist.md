# Checklist Hoàn Thành Tuần 1

## Kiến thức
- [x] Đọc xong phần "Determine the domain and scope" của Ontology Development 101 (Giai đoạn 0)
- [x] Hiểu RDF triple (Subject-Predicate-Object) và vì sao graph không cần schema cứng — [01](01-rdf-triple.md)
- [x] Hiểu IRI/namespace giải quyết vấn đề gì so với primary key trong SQL — [02](02-iri-namespace.md)
- [x] Phân biệt được SubClassOf (điều kiện cần) vs EquivalentClasses (điều kiện cần và đủ) — [03](03-class-hierarchy.md), [06](06-restriction-cardinality.md)
- [x] Phân biệt Object Property vs Data Property, biết ít nhất 4 đặc tính logic (Functional, Inverse, Symmetric, Transitive) — [04](04-object-vs-data-property.md)
- [x] Giải thích được Open World Assumption bằng lời của chính mình, có ví dụ cụ thể — [05](05-open-world-assumption.md)
- [x] Giải thích được Unique Name Assumption (owl:sameAs / owl:differentFrom) và vì sao nó liên quan tới việc hợp nhất dữ liệu ERP/MES — [05](05-open-world-assumption.md)

## Thực hành (Protégé)
- [x] Protégé cài đặt và chạy được — [07](07-cai-dat-protege.md)
- [x] 3 class (`Style`, `SKU`, `Order`) + instance mẫu — Phần A
- [x] `hasSKU` / `belongsToStyle` là cặp inverse property — Phần B
- [x] Restriction `exactly 1` và `min 1` đã khai trên `SKU`/`Style` — Phần B
- [x] `hasSizeLabel` là Functional — Phần B
- [x] Đã tự tay tạo ra một tình huống `inconsistent` (Functional vi phạm) và đọc được "Explain" — Phần B, bước 12
- [x] `SKU` và `Order` đã khai `disjointWith`, đã tự demo được inconsistency khi gán nhầm type — Phần C, bước 13
- [x] Class `CompleteSKU` dùng Equivalent To đã tạo, đã quan sát reasoner tự phân loại instance — Phần C, bước 14
- [x] Đã thử UNA: 2 individual giống dữ liệu nhưng khác IRI, đã dùng "Different Individuals" — Phần C, bước 15
- [x] Đã thử tab DL Query với ít nhất 2 biểu thức khác nhau — Phần C, bước 16

## Khi tất cả đã tick
Báo lại kết quả (đặc biệt nội dung "Explain" ở bước 12/13, kết quả bước 14). Tuần 1 sẽ được đánh dấu **Hoàn thành** trong `PROGRESS.md`, và một bộ test lý thuyết + thực hành sẽ được tạo trước khi chuyển sang Tuần 2.

---
*Xem lại: [00 — Tổng quan & mục lục](00-tong-quan.md)*
