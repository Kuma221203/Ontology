# Theo Dõi Tiến Độ Học Ontology

> File này do Claude duy trì để theo dõi bạn đã học/làm tới đâu trong lộ trình. Cập nhật sau mỗi lần học hoặc mỗi lần bạn báo cáo kết quả bài tập.

**Quy ước trạng thái:** `Chưa bắt đầu` · `Tài liệu đã soạn (chưa học)` · `Đang dạy lý thuyết` · `Chờ thực hành` · `Hoàn thành` · `Đã kiểm tra (test)`

**Lưu ý quan trọng:** Toàn bộ tài liệu Tuần 2-6 đã được soạn sẵn trước (xem `notes/README.md`) theo yêu cầu của người dùng — không cần đợi học xong tuần trước mới soạn tuần sau. Nhưng trạng thái `Tài liệu đã soạn (chưa học)` **khác** `Đang dạy lý thuyết`/`Hoàn thành` — chỉ chuyển sang các trạng thái sau khi nội dung thực sự được trao đổi/dạy trực tiếp với người dùng, không tự suy ra là "đã học" chỉ vì file tồn tại.

**Quy tắc tương tác đã thống nhất:** người dùng sẽ tự báo cáo tiến độ học của mình và đặt câu hỏi trong phạm vi kiến thức đã xác nhận — Claude cần đối chiếu đúng trạng thái trong bảng này để canh giải thích đúng trình độ, không giả định người dùng đã biết phần chưa được xác nhận.

**Quy ước test:** Sau khi một **giai đoạn/tuần** đạt trạng thái `Hoàn thành` (tức là bạn đã báo lại kết quả bài tập và checklist trong file ghi chú tương ứng được tick đủ), Claude sẽ tạo một bộ test (lý thuyết + thực hành) lưu tại `notes/tests/<tên-giai-đoạn>-test.md`, rồi cập nhật dòng tương ứng trong bảng dưới sang `Đã kiểm tra (test)`.

---

## Bảng tiến độ

| Giai đoạn / Tuần | Chủ đề | Trạng thái | Ghi chú note | Test |
|---|---|---|---|---|
| Giai đoạn 0 | Khung tư duy: ontology vs taxonomy/schema/vocabulary, chọn scope | Đang dạy lý thuyết | `notes/giai-doan-0.md` | — |
| Tuần 1 | RDF/OWL nền tảng, Protégé, class/property/restriction/cardinality, OWA, UNA, defined class | Đã kiểm tra (test) | `notes/tuan-01/` (10 file theo chủ đề, xem `00-tong-quan.md`) — bài tập 16 bước đã hoàn thành | `notes/tests/tuan-01-test.md` |
| Tuần 2 | Methodology, Competency Questions, reuse GS1/ISA-95 | Tài liệu đã soạn (chưa học) | `notes/tuan-02/` (7 file, xem `00-tong-quan.md`) | — |
| Tuần 3 | Lập trình hóa Python (rdflib, owlready2, SPARQL) | Tài liệu đã soạn (chưa học) | `notes/tuan-03/` (7 file) | — |
| Tuần 4 | Triple store / Graph DB | Tài liệu đã soạn (chưa học) | `notes/tuan-04/` (8 file) | — |
| Tuần 5 | Tích hợp AI (GraphRAG, LLM extraction) | Tài liệu đã soạn (chưa học) | `notes/tuan-05/` (9 file) | — |
| Tuần 6 | Validation (SHACL), versioning, đánh giá | Tài liệu đã soạn (chưa học) | `notes/tuan-06/` (6 file) | — |

---

## Chi tiết trạng thái hiện tại

### Giai đoạn 0
- Đã dạy: bảng phân biệt ontology/taxonomy/schema/vocabulary, ví dụ đối chiếu cụ thể, lý do quan trọng với vai trò AI Engineer.
- Đã giao: đọc "Ontology Development 101" (phần "Determine the domain and scope") — **chưa có xác nhận đã đọc xong**.
- Scope project đã chốt: Order/Style/SKU/BOM + quy trình cutting→sewing→QC→packing→shipment.
- Lưu tại: `notes/giai-doan-0.md`.

### Tuần 1 — HOÀN THÀNH (2026-11-11)
- Nội dung: 10 file theo chủ đề trong `notes/tuan-01/` (mục lục: `00-tong-quan.md`): RDF triple/serialization, IRI/namespace, class hierarchy (disjoint, multiple inheritance), object vs data property (toàn bộ đặc tính logic + property chain), Open World Assumption + Unique Name Assumption (sâu, có ví dụ hợp nhất dữ liệu ERP/MES), restriction/cardinality nâng cao + defined class (EquivalentClasses), cài đặt/tour Protégé, FAQ, checklist.
- Protégé 5.6.9 Windows đã cài, bài tập 16 bước (Phần A/B/C) đã hoàn thành — người dùng xác nhận.
- Có thêm 3 câu hỏi mở rộng tangent đã giải đáp trong chat (chưa lưu file riêng): phép ẩn dụ RDF/ontology ~ binary/software; cách ontology tích hợp vào AI (3 pattern: GraphRAG truy xuất chọn lọc, structured output contract, reasoning/validation layer) minh họa qua ví dụ "xuống PO cho Style A123"; TBox vs ABox và chi phí lưu trữ triple store ở quy mô 100K SKU (~60MB, dictionary encoding).
- **Test đã tạo**: `notes/tests/tuan-01-test.md` (10 câu lý thuyết + 1 bài thực hành BOM/Fabric/Trim mới, có đáp án ẩn trong `<details>`).

### Câu hỏi mở (chưa có câu trả lời)
- Trong doc "Lộ Trình Học Ontology — Nhà Máy May Mặc" có một comment hỏi: bạn đã có quyền export dữ liệu thật (đơn hàng/BOM) từ hệ thống nhà máy chưa, hay nên dùng dữ liệu mẫu cho Tuần 3? → Ảnh hưởng cách chuẩn bị dữ liệu ở Tuần 3, nên trả lời trước khi tới tuần đó.

---

## Lịch sử cập nhật

- 2026-11-11 — Khởi tạo file. Giai đoạn 0 + Tuần 1 đã dạy lý thuyết, đang chờ phản hồi bài tập.
- 2026-11-11 — Tuần 1 hoàn thành, test đã tạo. Theo yêu cầu người dùng, soạn sẵn toàn bộ tài liệu Tuần 2-6 (`notes/tuan-02/` đến `notes/tuan-06/`, tổng 37 file) dù chưa bắt đầu dạy — không cần đợi tuần trước xong mới soạn tuần sau. Thêm `notes/README.md` làm mục lục tổng. Thống nhất quy tắc: người dùng tự báo tiến độ + đặt câu hỏi trong phạm vi đã xác nhận học, Claude canh giải thích đúng trình độ dựa trên bảng tiến độ này (không suy ra "đã học" chỉ vì tài liệu tồn tại).
