# Vì Sao Cần Rời Protégé

Protégé tuyệt vời để **thiết kế** TBox (Tuần 1-2) — ít class, ít property, thay đổi không thường xuyên, cần con người suy nghĩ kỹ từng tiên đề. Nhưng:

- Không ai gõ tay **100.000 SKU** qua giao diện click-chuột
- Protégé không có cơ chế "đọc file CSV rồi tự sinh instance hàng loạt"
- Muốn tích hợp vào một pipeline tự động (cron job đồng bộ từ ERP mỗi đêm, hay agent AI gọi lúc runtime) thì phải có **API/code**, không phải thao tác tay trong GUI

Tuần này học cách **đọc, sửa, sinh triple bằng code** — đây là bước bắt buộc trước khi nói tới triple store quy mô lớn (Tuần 4) hay tích hợp AI (Tuần 5).

## Vai trò của Protégé từ giờ trở đi

Không bỏ hẳn Protégé — nó vẫn là công cụ tốt nhất để:
- **Xem lại** ontology trực quan khi cần debug
- **Thiết kế thay đổi TBox** mới (thêm class/restriction) — vẫn làm tay, vì đây là quyết định logic cần con người cân nhắc
- **Chạy reasoner thủ công** để kiểm tra nhanh một ý tưởng trước khi code hóa

Còn **populate dữ liệu** (ABox) và **truy vấn** sẽ chuyển hẳn sang code từ tuần này.

---
*Tiếp theo: [02 — rdflib](02-rdflib.md)*
