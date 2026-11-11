# Methodology — Vì Sao Cần Phương Pháp Luận

## Vì sao không nên vẽ ontology tự do

Không có phương pháp, ontology dễ rơi vào 1 trong 2 thái cực:
- **Quá rộng**: model cả những thứ không ai hỏi tới — tốn công thiết kế, tốn công bảo trì, mà không ai dùng.
- **Quá hẹp**: thiếu khái niệm cần thiết, phát hiện ra giữa chừng, phải sửa đi sửa lại cấu trúc đã lỡ dùng ở nhiều chỗ.

Methodology giải quyết việc này bằng cách bắt đầu từ **câu hỏi thực tế cần trả lời** (xem [02-competency-questions.md](02-competency-questions.md)) thay vì bắt đầu từ "liệt kê mọi khái niệm tôi nghĩ ra".

## METHONTOLOGY — quy trình tuần tự, kiểm soát chặt

Phương pháp luận cổ điển, gồm các pha:

1. **Specification** (Đặc tả): xác định domain, scope, mục đích sử dụng, danh sách Competency Questions
2. **Conceptualization** (Khái niệm hóa): mô hình hóa các khái niệm bằng ngôn ngữ tự nhiên có cấu trúc (glossary, bảng quan hệ) — chưa viết OWL vội
3. **Formalization** (Hình thức hóa): chuyển mô hình khái niệm sang biểu diễn hình thức (OWL/Description Logic)
4. **Implementation**: hiện thực trong công cụ (Protégé)
5. **Maintenance**: bảo trì, cập nhật khi nghiệp vụ thay đổi

Phù hợp khi: xây ontology từ đầu, team nhỏ, muốn kiểm soát chặt từng bước trước khi "code hóa".

## NeOn — nhấn mạnh tái sử dụng

Khác METHONTOLOGY (thiết kế từ số 0), NeOn đặt câu hỏi **"đã có ai làm việc này chưa?"** trước khi bắt tay thiết kế:

1. Tìm ontology/vocabulary sẵn có liên quan đến domain
2. Đánh giá: tái dùng trực tiếp? tái dùng một phần? chỉ tham khảo cấu trúc?
3. Chỉ thiết kế mới phần **thực sự không có gì sẵn** (greenfield)
4. Hợp nhất (merge/align) phần tái dùng với phần tự thiết kế

## Project này dùng phương pháp nào

**Kết hợp cả hai, nghiêng về NeOn:**
- Tinh thần NeOn: đã làm bước "tìm ontology sẵn có" ở `research/ontology-manufacturing-standards.md` — biết rõ GS1 EPCIS/CBV tái dùng được, ISA-95 chỉ tham khảo, phần core phải tự thiết kế (xem [03-quyet-dinh-reuse.md](03-quyet-dinh-reuse.md)).
- Công cụ của METHONTOLOGY: bước "Specification" bằng Competency Questions ([02-competency-questions.md](02-competency-questions.md)) — áp dụng cho đúng phần core cần tự thiết kế.

Nói cách khác: **không thiết kế lại cái đã có (NeOn), nhưng cái phải tự thiết kế thì làm bài bản có đặc tả rõ ràng (METHONTOLOGY)**.

---
*Tiếp theo: [02 — Competency Questions](02-competency-questions.md)*
