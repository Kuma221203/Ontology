# Cài Đặt & Làm Quen Giao Diện Protégé

## Cài đặt

Bản Windows chính thức (bundle sẵn JVM, không cần cài Java riêng) đã tải về `Downloads/Protege-5.6.9-win.zip`.

1. Giải nén zip
2. Chạy `Protege.exe` trong thư mục vừa giải nén
3. File → New ontology → đặt Ontology IRI, ví dụ `http://nhamay.vn/ontology/sanxuat` (xem cách chọn namespace ở file [02](02-iri-namespace.md))

## Tour các tab chính

| Tab | Dùng để làm gì |
|---|---|
| **Active Ontology** | Metadata của ontology, import ontology khác (sẽ dùng khi import file GS1 EPCIS ở Tuần 2) |
| **Entities** | Xem tổng hợp mọi class/property/individual trong 1 màn hình, tìm kiếm nhanh |
| **Classes** | Cây class hierarchy, panel "Description" bên phải để thêm SubClassOf/EquivalentClasses/restriction |
| **Object Properties** | Danh sách object property, panel "Characteristics" để tick Functional/Inverse/Transitive... |
| **Data Properties** | Tương tự, cho data property |
| **Individuals by class** | Xem/tạo instance, gán property values trực tiếp cho từng instance |
| **DL Query** | **Công cụ thử nghiệm nhanh** — gõ một class expression (ví dụ `hasSKU some SKU`), Protégé liệt kê ngay những instance/class nào thỏa mãn, **không cần tạo class thật**. Rất hữu ích để thử một restriction trước khi "chốt" vào ontology. |

## Trực quan hóa (plugin)

Protégé hỗ trợ plugin **OntoGraf** hoặc **VOWL** để xem ontology dưới dạng đồ thị (node-edge) thay vì cây text — hữu ích khi ontology lớn dần và bạn cần nhìn tổng quan cấu trúc. Vào View → Views → OntoGraf nếu đã cài plugin (một số bản Protégé có sẵn, một số cần cài qua Plugins → Plugin Manager).

## Reasoner menu

- **Start reasoner / Stop reasoner**: bật/tắt reasoner
- **Synchronize reasoner**: chạy lại suy luận sau khi bạn vừa sửa ontology (Protégé không tự động re-run liên tục, phải bấm để cập nhật kết quả)
- **Reasoner khác nhau**: mặc định Protégé dùng **HermiT**. Có thể cài thêm **Pellet** (hỗ trợ tốt hơn một số tính năng như qualified cardinality trong vài phiên bản cũ) hoặc **ELK** (cực nhanh nhưng chỉ hỗ trợ một tập con rút gọn của OWL gọi là OWL EL profile — phù hợp ontology rất lớn, không cần ở quy mô project này)

## Phím tắt & mẹo

- `Ctrl+Shift+N`: tạo nhanh individual mới trong tab Individuals
- Đổi cách hiển thị tên entity: Protégé mặc định hiện tên ngắn (local name); có thể đổi sang hiện full IRI qua menu nếu cần debug namespace (File → Preferences → Renderer)
- Khi ontology lớn, dùng ô tìm kiếm (biểu tượng kính lúp góc trên) thay vì cuộn cây class thủ công

## Lỗi thường gặp khi mới cài

- **Java version**: bản `-win.zip` đã bundle JVM riêng nên thường không cần quan tâm, nhưng nếu dùng bản "platform-independent" (yêu cầu Java cài sẵn) thì cần Java 11+ trên máy.
- **Màn hình high-DPI làm chữ quá nhỏ**: chỉnh trong `Protege.l4j.ini` (file cấu hình cạnh `Protege.exe`) hoặc qua Preferences trong app.
- **Ontology không lưu đúng format mong muốn**: khi Save As, chú ý chọn "Turtle Syntax" thay vì để mặc định RDF/XML nếu bạn muốn file dễ đọc/dễ diff trong git sau này (Tuần 6).

---
*Tiếp theo: [08 — Bài tập thực hành đầy đủ](08-bai-tap-thuc-hanh.md)*
