# Câu Hỏi Thường Gặp — Tuần 3

**Hỏi:** `rdflib` và `owlready2` có thể dùng chung 1 file ontology không?
**Đáp:** Có — cả hai đều đọc/ghi được `.owl`/`.ttl` chuẩn, chỉ khác API thao tác. Thực tế hay dùng `owlready2` cho logic/reasoning, `rdflib` cho SPARQL/populate hàng loạt — xem bảng so sánh ở [03-owlready2.md](03-owlready2.md).

**Hỏi:** Chạy reasoner bằng `owlready2` có chậm với dữ liệu lớn không?
**Đáp:** Có — như đã nói ở phần TBox/ABox (trong hội thoại trước, chưa lưu file riêng) và nhắc lại ở [03-owlready2.md](03-owlready2.md): reasoner đầy đủ chỉ nên chạy trên TBox nhỏ hoặc tập dữ liệu vừa phải để test. Với ABox hàng trăm nghìn instance, thường không chạy lại reasoner mỗi lần (xem thêm Tuần 6).

**Hỏi:** SPARQL có thể JOIN nhiều "bảng" (nhiều loại entity) cùng lúc như SQL không?
**Đáp:** Có — và còn tự nhiên hơn SQL, vì không cần `JOIN ... ON` tường minh. Mỗi dòng pattern trong `WHERE` dùng chung biến (ví dụ `?sku`) sẽ tự động "nối" các entity lại — xem ví dụ CQ #1 ở [04-sparql.md](04-sparql.md), nối `WorkOrder` với `SKU` qua biến `?sku` dùng chung.

**Hỏi:** Có cần học thuộc toàn bộ cú pháp SPARQL không?
**Đáp:** Không — nắm vững `SELECT`/`WHERE`/`FILTER`/`OPTIONAL`/`GROUP BY` là đủ cho hầu hết Competency Questions của project này. Cú pháp nâng cao hơn (subquery, property path phức tạp) tra cứu khi cần, không cần nhớ trước.

---
*Tiếp theo: [07 — Checklist hoàn thành](07-checklist.md)*
