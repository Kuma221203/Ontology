# Bài Tập Thực Hành Tuần 6

1. Viết ít nhất 3 SHACL shape cho các class quan trọng (`WorkOrder`, `SKU`, `Order`) — mỗi shape có ít nhất 2 constraint (`minCount`, `datatype`, hoặc tương tự) theo mẫu [01-shacl.md](01-shacl.md)
2. Chạy `pyshacl` validate trên dữ liệu thật đã populate từ Tuần 3-5 — cố tình để vài instance thiếu trường bắt buộc, xác nhận SHACL báo lỗi đúng chỗ
3. Cài ROBOT, chạy `robot convert` chuẩn hóa file ontology, commit vào git với message mô tả rõ thay đổi
4. Thử `robot diff` giữa 2 version ontology (ví dụ bản Tuần 1 vs bản hiện tại) — xem danh sách axiom đã thay đổi qua 6 tuần
5. Hoàn thành bảng Coverage Competency Questions (mục 1 của [03-danh-gia-chat-luong.md](03-danh-gia-chat-luong.md)) cho toàn bộ CQ đã viết ở Tuần 2
6. Hoàn thiện bảng so sánh Retrieval quality (mục 3 của file 03) với ít nhất 5 câu hỏi

## Nộp lại

Báo cho mình: kết quả SHACL validate (bao nhiêu lỗi phát hiện, có đúng như kỳ vọng không), kết quả `robot diff`, và 2 bảng đánh giá (coverage + retrieval quality) hoàn chỉnh.

---
*Tiếp theo: [05 — FAQ](05-faq.md)*
