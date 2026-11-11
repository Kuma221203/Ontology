# Câu Hỏi Thường Gặp — Tuần 4

**Hỏi:** Có bắt buộc dùng Docker không?
**Đáp:** Không — Fuseki chạy được trực tiếp nếu có Java, tải bản standalone từ trang Apache Jena ([03-cai-dat-fuseki.md](03-cai-dat-fuseki.md)). Docker chỉ cho nhanh/gọn, không cần cài Java thủ công.

**Hỏi:** Triple store có thay thế được database quan hệ (Postgres/MySQL) hiện tại của nhà máy không?
**Đáp:** Không nên nghĩ vậy ở giai đoạn này — triple store đóng vai trò **lớp tri thức/ý nghĩa** (ontology + KG), còn hệ thống giao dịch thật (ERP/MES) vẫn là SQL như cũ. Hai hệ thống **song song**, đồng bộ qua script (Tuần 3), không thay thế nhau — đúng tinh thần đã bàn ở ví dụ "xuống PO cho Style A123" trước đó trong hội thoại.

**Hỏi:** Nếu dữ liệu trong triple store và ERP bị lệch nhau (đồng bộ trễ), AI dựa trên KG có trả lời sai không?
**Đáp:** Có thể — đây là rủi ro thật của mọi kiến trúc "đồng bộ định kỳ" (không realtime tuyệt đối). Giải pháp thường gặp: đồng bộ thường xuyên hơn với dữ liệu quan trọng/thay đổi nhanh, hoặc với truy vấn cực kỳ nhạy thời gian thì gọi thẳng API ERP thay vì qua KG. Đây là quyết định kiến trúc cần cân nhắc theo từng loại dữ liệu, không có câu trả lời chung cho mọi trường hợp.

**Hỏi:** API layer (FastAPI) có bắt buộc phải có không, hay agent có thể gọi thẳng SPARQL endpoint?
**Đáp:** Về mặt kỹ thuật agent có thể gọi thẳng, nhưng không khuyến khích — xem lý do bảo mật/kiểm soát ở [05-api-layer.md](05-api-layer.md).

---
*Tiếp theo: [08 — Checklist hoàn thành](08-checklist.md)*
