# Đánh Giá Chất Lượng Ontology & Retrieval

## Ba loại đánh giá cần làm

1. **Coverage Competency Questions** — bao nhiêu % câu hỏi viết ở Tuần 2 thực sự trả lời đúng được bằng hệ thống đã xây?
2. **Consistency định kỳ** — ontology (TBox) có còn consistent sau mỗi lần sửa không?
3. **Retrieval quality** — so với baseline vector-only đã làm ở Tuần 5, GraphRAG tốt hơn cụ thể bao nhiêu?

## 1. Coverage Competency Questions

Cách đơn giản, không cần công cụ phức tạp — một bảng tự chấm:

| # | Competency Question | Trả lời đúng? | Ghi chú |
|---|---|---|---|
| 1 | SKU nào đang trễ tiến độ ở công đoạn may? | ✅ | Query SPARQL (Tuần 3) + tool (Tuần 5) hoạt động đúng |
| 2 | Lot vải nào đã dùng cho đơn hàng X? | ⚠️ | Trả lời được nhưng cần join 3 bước, chưa có tool riêng |
| ... | ... | ... | ... |

Tính tỷ lệ `✅` / tổng số CQ — đây là con số cụ thể cho biết ontology đã "đủ dùng" tới đâu so với mục tiêu ban đầu đặt ra ở Tuần 2.

## 2. Consistency định kỳ — đưa reasoner vào CI

```bash
# Chạy reasoner tự động mỗi khi TBox thay đổi (ví dụ trong CI/CD pipeline)
robot reason --input sanxuat.ttl --reasoner HermiT --output /dev/null
```

Nếu `robot reason` báo lỗi (ontology inconsistent), pipeline CI fail — ngăn một thay đổi TBox gây mâu thuẫn logic được merge vào nhánh chính. Đây là cách áp dụng tư duy "test tự động" quen thuộc của kỹ sư phần mềm vào ontology.

## 3. Retrieval quality — hoàn thiện so sánh đã bắt đầu ở Tuần 5

Mở rộng bảng so sánh đã làm ở [06-so-sanh-dinh-luong.md](../tuan-05/06-so-sanh-dinh-luong.md) thành báo cáo đầy đủ:

| Metric | Vector RAG thuần | GraphRAG |
|---|---|---|
| % câu trả lời đúng (trên toàn bộ CQ) | ... | ... |
| % câu có số liệu chính xác tuyệt đối | ... | ... |
| Thời gian trả lời trung bình | ... | ... |

Không cần đạt chuẩn benchmark học thuật — mục tiêu là có **bằng chứng cụ thể, đo được** để báo cáo lại (ví dụ cho sếp/đồng nghiệp) rằng công sức xây ontology 6 tuần qua mang lại giá trị gì so với cách làm cũ.

## SHACL đóng vai trò gì trong đánh giá

SHACL (file 01) không trực tiếp "đánh giá chất lượng" theo nghĩa 3 mục trên, nhưng là **điều kiện tiên quyết**: nếu dữ liệu không qua được SHACL validate, mọi số liệu đánh giá retrieval phía trên đều không đáng tin — vì có thể sai do dữ liệu thiếu/lỗi, không phải do kiến trúc GraphRAG kém. Luôn validate SHACL **trước** khi đo retrieval quality.

---
*Tiếp theo: [04 — Bài tập thực hành](04-bai-tap-thuc-hanh.md)*
