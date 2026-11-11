# Bài Tập Thực Hành Tuần 4

1. Chạy Fuseki qua Docker (hoặc cài trực tiếp), tạo dataset `sanxuat`
2. Upload ontology + instance data đã có từ Tuần 1-3 vào dataset (qua giao diện web hoặc `curl`)
3. Query thử qua giao diện web Fuseki (tab Query) bằng 1 câu SPARQL đã viết ở Tuần 3
4. Viết script Python gọi SPARQL endpoint qua `requests` (theo [04-query-qua-http.md](04-query-qua-http.md))
5. Viết 1 API endpoint FastAPI đơn giản bọc lại 1 Competency Question (theo [05-api-layer.md](05-api-layer.md)), test bằng trình duyệt hoặc `curl`

## Nộp lại

Báo cho mình: xác nhận Fuseki chạy và có dữ liệu, kết quả query qua Python, và endpoint API hoạt động (gửi lại response mẫu nếu tiện).

---
*Tiếp theo: [07 — FAQ](07-faq.md)*
