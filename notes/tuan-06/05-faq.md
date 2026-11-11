# Câu Hỏi Thường Gặp — Tuần 6

**Hỏi:** Nếu đã có SHACL validate rồi, OWL restriction (Tuần 1) còn cần giữ lại không?
**Đáp:** Có — hai công cụ phục vụ mục đích khác nhau, không thay thế nhau. OWL vẫn cần cho suy luận (defined class như `CompleteSKU` tự động phân loại), SHACL chỉ lo phần validate "đủ dữ liệu chưa". Xem bảng so sánh ở [01-shacl.md](01-shacl.md).

**Hỏi:** `robot diff` và `git diff` khác nhau thế nào, dùng cái nào?
**Đáp:** Dùng cả hai cho mục đích khác nhau — `git diff` cho biết **dòng text nào đổi** (hữu ích review code review thông thường), `robot diff` cho biết **axiom ngữ nghĩa nào thực sự đổi** (hữu ích khi Protégé sắp xếp lại thứ tự block làm `git diff` "nhiễu"). Xem [02-versioning-robot.md](02-versioning-robot.md).

**Hỏi:** Coverage Competency Questions dưới 100% có phải là thất bại không?
**Đáp:** Không nhất thiết — một số CQ có thể cần dữ liệu chưa sẵn sàng (ví dụ chưa tích hợp đủ nguồn dữ liệu thật từ ERP), hoặc cần thêm thời gian viết tool riêng. Quan trọng là **biết rõ** CQ nào chưa trả lời được và vì sao, để lên kế hoạch tiếp theo — không phải đạt 100% ngay ở bản đầu tiên.

**Hỏi:** Sau Tuần 6, ontology coi như "xong" chưa?
**Đáp:** Không — như đã nói ở [01-methodology.md](../tuan-02/01-methodology.md), ontology phát triển lặp lại (iterative). Tuần 6 là một **chu kỳ hoàn chỉnh đầu tiên** (scope → thiết kế → code hóa → tích hợp AI → validate/đánh giá), không phải điểm kết thúc. Chu kỳ tiếp theo sẽ mở rộng scope (ví dụ thêm domain tồn kho/nhân sự) dựa trên đúng quy trình đã học.

---
*Tiếp theo: [06 — Checklist hoàn thành + tổng kết](06-checklist.md)*
