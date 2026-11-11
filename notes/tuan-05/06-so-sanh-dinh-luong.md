# So Sánh Định Lượng Với Baseline Vector-Only

Để chứng minh GraphRAG tốt hơn cho loại câu hỏi phù hợp (không chỉ lý thuyết), nên so sánh trên cùng bộ câu hỏi với một baseline "vector RAG thuần" (nhúng toàn bộ log sản xuất thành embedding, tìm đoạn gần nhất, không qua ontology).

## Bảng tiêu chí so sánh

| Tiêu chí | Vector RAG thuần (baseline) | GraphRAG |
|---|---|---|
| Độ chính xác số liệu (ví dụ đếm số SKU trễ) | Thường sai hoặc xấp xỉ — vector search không "đếm" được | Chính xác tuyệt đối (từ kết quả `COUNT` của query) |
| Khả năng trả lời câu hỏi có điều kiện logic phức tạp (nhiều điều kiện AND/OR) | Yếu — phụ thuộc đoạn text có "nói đúng ý" không | Mạnh — SPARQL biểu diễn điều kiện logic trực tiếp |
| Chi phí xây dựng ban đầu | Thấp — chỉ cần embedding + vector DB | Cao hơn — cần ontology + KG + pipeline extraction |
| Khả năng giải thích "vì sao" ra kết quả này | Khó — không rõ đoạn nào dẫn tới kết luận | Dễ — truy ngược được query + triple nguồn gốc |
| Độ trễ khi dữ liệu thay đổi | Phải re-embed lại đoạn text liên quan | Chỉ cần thêm/sửa triple tương ứng |

## Cách tự đánh giá (gợi ý phương pháp, không cần công cụ phức tạp)

1. Chọn 5-10 câu hỏi (dùng lại Competency Questions Tuần 2, ưu tiên câu có số liệu/điều kiện logic)
2. Chạy qua cả 2 hệ thống (GraphRAG của bạn và một baseline vector RAG đơn giản dựng tạm)
3. Tự chấm điểm đúng/sai (hoặc đúng một phần) cho từng câu, so sánh tỷ lệ đúng giữa 2 hệ thống
4. Ghi nhận cụ thể trường hợp vector RAG sai (ví dụ: đếm sai số lượng, bỏ sót điều kiện) — đây là bằng chứng thuyết phục hơn nhiều so với chỉ nói "GraphRAG tốt hơn" chung chung

Không cần một bộ eval phức tạp ở mức học tập này — mục tiêu là **tận mắt thấy** sự khác biệt cụ thể, không phải đạt một con số benchmark chuẩn hóa.

---
*Tiếp theo: [07 — Bài tập thực hành](07-bai-tap-thuc-hanh.md)*
