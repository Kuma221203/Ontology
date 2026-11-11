# Các Lựa Chọn Triple Store

| | Apache Jena Fuseki | GraphDB | Oxigraph |
|---|---|---|---|
| Giấy phép | Mã nguồn mở, miễn phí | Free tier + bản trả phí | Mã nguồn mở, miễn phí |
| Ngôn ngữ cài đặt | Java | Java | Rust (nhẹ, khởi động nhanh) |
| Giao diện quản trị | Có, đơn giản | Có, đầy đủ hơn (GUI quản trị tốt) | Tối giản, thiên về nhúng |
| Phù hợp | Học tập, dev, production vừa | Production, cần GUI quản trị mạnh | Dev nhẹ, nhúng vào ứng dụng nhỏ |
| Khuyến nghị cho project | **Dùng cho Tuần 4** — phổ biến nhất, tài liệu nhiều, dễ Docker hóa | Cân nhắc khi lên production thật, cần nhiều tính năng enterprise | Thử nghiệm nếu muốn gọn nhẹ, không cần Java |

## Vì sao chọn Fuseki cho giai đoạn học

- Mã nguồn mở hoàn toàn, không giới hạn tính năng theo gói trả phí
- Cộng đồng Apache Jena lớn, tài liệu/ví dụ nhiều, dễ tìm khi vướng lỗi
- Có sẵn Docker image chính thức, không cần tự cài Java thủ công (xem [03-cai-dat-fuseki.md](03-cai-dat-fuseki.md))
- Hỗ trợ đầy đủ chuẩn SPARQL 1.1 — kiến thức SPARQL học được áp dụng được cho mọi triple store khác (GraphDB, Oxigraph, Neptune...) không cần học lại

## Khi nào cân nhắc đổi sang GraphDB hoặc dịch vụ managed cloud

Khi project lên production thật, với yêu cầu như: cần GUI quản trị mạnh cho người không rành kỹ thuật, cần hỗ trợ enterprise (SLA, support), hoặc muốn không tự vận hành hạ tầng (dùng managed service như AWS Neptune) — đây là quyết định hạ tầng, không phải quyết định kỹ thuật RDF/SPARQL, nên để dành khi project thật sự cần triển khai production.

---
*Tiếp theo: [03 — Cài đặt Fuseki](03-cai-dat-fuseki.md)*
