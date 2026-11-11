# Bài Tập Thực Hành Tuần 5

1. Viết 5-10 đoạn "log sản xuất" giả lập (văn bản tự do tiếng Việt) mô phỏng log KCS/sản xuất thật
2. Viết script dùng LLM (structured output) trích xuất thông tin từ các log đó, map sang triple đúng ontology, ghi vào triple store Tuần 4 (theo [04-llm-extraction.md](04-llm-extraction.md))
3. Viết 1 hàm "tool" (Cách B ở [03-llm-sinh-sparql.md](03-llm-sinh-sparql.md)) cho LLM gọi để trả lời 1 Competency Question
4. Ghép thành 1 pipeline nhỏ theo khung ở [05-pipeline-hoan-chinh.md](05-pipeline-hoan-chinh.md): nhận câu hỏi tiếng Việt → LLM chọn tool → query → LLM trả lời tự nhiên
5. Làm so sánh định lượng (dù đơn giản) theo [06-so-sanh-dinh-luong.md](06-so-sanh-dinh-luong.md) trên ít nhất 3 câu hỏi

## Nộp lại

Báo cho mình: pipeline chạy thành công với ví dụ cụ thể (1 log đầu vào → triple sinh ra → câu hỏi → câu trả lời), và kết quả so sánh định lượng (dù chỉ 3 câu).

---
*Tiếp theo: [08 — FAQ](08-faq.md)*
