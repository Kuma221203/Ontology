# Câu Hỏi Thường Gặp — Tuần 2

**Hỏi:** Nếu sau này phát hiện 1 Competency Question không trả lời được, phải làm sao?
**Đáp:** Đó là dấu hiệu bình thường — quay lại bổ sung class/property/restriction cần thiết. Ontology luôn phát triển lặp lại (iterative), không phải thiết kế một lần là xong. Xem lại [01-methodology.md](01-methodology.md) — cả METHONTOLOGY lẫn NeOn đều có bước "maintenance/cập nhật" trong vòng đời.

**Hỏi:** Có bắt buộc phải import EPCIS ngay bây giờ không, hay để dành Tuần 4?
**Đáp:** Nên làm quen cách import ngay ở Tuần 2 (thao tác đơn giản trong Protégé, chạy tay vài instance mẫu). Việc *populate* dữ liệu thật theo EPCIS ở quy mô lớn sẽ làm bằng code ở Tuần 3, và đưa vào triple store thật ở Tuần 4.

**Hỏi:** Nếu một Competency Question liên quan tới cả phần core (nội bộ) lẫn phần EPCIS (traceability), class/property có bị lẫn namespace không?
**Đáp:** Không sao — một câu SPARQL (Tuần 3) hoàn toàn truy vấn được xuyên qua nhiều namespace cùng lúc, miễn cả hai phần đã nằm trong cùng 1 graph/ontology. Giữ namespace riêng chỉ để tránh *trùng tên khái niệm*, không ngăn cản truy vấn chung.

**Hỏi:** Làm sao biết một khái niệm nên dùng Cách 1 (subclass EPCIS) hay Cách 2 (để riêng) ở file 04?
**Đáp:** Tự hỏi: "nếu một hệ thống bên ngoài chỉ hiểu chuẩn EPCIS, không biết gì về ontology nội bộ của tôi, nó có cần hiểu khái niệm này không?" Nếu có (ví dụ sự kiện truy vết để báo cáo cho khách hàng/đối tác) → nên subclass. Nếu là khái niệm nội bộ thuần túy (ví dụ cách phân công chuyền may) → để riêng.

---
*Tiếp theo: [07 — Checklist hoàn thành](07-checklist.md)*
