# Giai Đoạn 0 — Khung Tư Duy

> Phần nền trước khi vào Tuần 1. Domain: ontology hóa nghiệp vụ sản xuất của một nhà máy may mặc (Order → Style → SKU → BOM → quy trình sản xuất).

## 0.1. Bốn khái niệm hay bị nhầm lẫn

| Khái niệm | Nó là gì | Ví dụ nhà máy may | Giới hạn |
|---|---|---|---|
| **Taxonomy** | Cây phân cấp, chỉ có quan hệ "là-một-loại-của" (is-a) | Garment → Áo → Áo sơ mi → Áo sơ mi dài tay | Không biểu diễn được ràng buộc/logic giữa các nhánh khác nhau |
| **Schema** | Cấu trúc dữ liệu (bảng, field, kiểu dữ liệu) — mô tả *hình dạng* dữ liệu | Bảng `orders(order_id, customer_id, due_date)` | Không mang nghĩa — máy không "hiểu" `customer_id` liên hệ gì với thế giới thực ngoài một con số |
| **Vocabulary** | Tập từ/thuật ngữ dùng chung | Danh sách mã lỗi KCS: "đứt chỉ", "lệch kim", "bẩn vải" | Chỉ là nhãn, không có quan hệ hay ràng buộc đi kèm |
| **Ontology** | Mô hình khái niệm + quan hệ + ràng buộc logic, **máy suy luận được** | "Mọi `WorkOrder` phải `thuộc về` đúng 1 `Order`; `SKU` không có `BOM` thì không thể `startCutting`" | Cần công cụ (reasoner) và kỷ luật thiết kế nhiều hơn 3 loại trên |

**Điểm khác biệt cốt lõi:** taxonomy chỉ trả lời được "A là loại gì của B". Ontology trả lời được "nếu A thì B", "A không thể tồn tại nếu thiếu C", "A và D mâu thuẫn nhau" — tức là có **suy luận logic** (reasoning), không chỉ phân loại tĩnh.

## 0.2. Vì sao điều này quan trọng — một tình huống cụ thể

Giả sử nghiệp vụ cần biểu diễn: *"Một SKU không thể bắt đầu công đoạn cắt nếu chưa có BOM đầy đủ."*

- **Trong schema (SQL):** bạn phải tự viết code kiểm tra (`IF NOT EXISTS (SELECT * FROM bom WHERE sku_id = ...) THEN RAISE ERROR`) — luật nằm trong code ứng dụng, không nằm trong dữ liệu.
- **Trong taxonomy:** không biểu diễn được — taxonomy không có khái niệm "điều kiện tiên quyết".
- **Trong vocabulary:** chỉ có thể liệt kê "BOM", "Cutting" là hai thuật ngữ, không nói được quan hệ giữa chúng.
- **Trong ontology:** bạn viết **một lần** thành tiên đề logic (`Cutting` yêu cầu SKU phải có `hasBOM some BOM`), và **mọi** công cụ đọc được ontology này (dashboard, agent AI, API khác) đều suy luận ra cùng một kết luận — không cần viết lại logic ở từng nơi.

Đây chính là lý do một AI Engineer nên quan tâm ontology: nó là nơi **tập trung hóa tri thức nghiệp vụ** mà LLM/agent có thể tra cứu và suy luận trên đó, thay vì nhúng luật nghiệp vụ rải rác trong prompt hay code.

## 0.3. Tài liệu nên đọc

**"Ontology Development 101: A Guide to Creating Your First Ontology"** — Natalya F. Noy & Deborah L. McGuinness.
Link: https://protege.stanford.edu/publications/ontology_development/ontology101.pdf

Chỉ cần đọc phần **"Determine the domain and scope of the ontology"** (đầu bài, ~2-3 trang) cho giai đoạn này. Phần còn lại (methodology chi tiết) sẽ dùng ở Tuần 2.

## 0.4. Phạm vi (scope) đã chốt cho project

**Trong phạm vi:**
- Order, Style, SKU, BOM (fabric/trim)
- Quy trình sản xuất: cutting → sewing → QC → packing → shipment
- Work order, WIP tracking cơ bản

**Ngoài phạm vi (chưa làm ở version đầu):**
- Tài chính, kế toán, lương
- Nhân sự (ngoại trừ gán operator vào work order nếu cần sau)
- Chi tiết máy móc/IoT (để dành nếu dùng W3C SSN/SOSA sau này)

---

*Tiếp theo: [Tuần 1 — Tổng quan & mục lục](tuan-01/00-tong-quan.md)*
