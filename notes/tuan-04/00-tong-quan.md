# Tuần 4 — Tổng Quan & Mục Lục

> Từ "file `.ttl` trên máy" sang "dịch vụ sống, có SPARQL endpoint" — bước bắt buộc để nhiều ứng dụng (API, agent, dashboard) cùng truy vấn một nguồn dữ liệu thật.

## Mục tiêu tuần này

Có một triple store chạy thật (Fuseki), nạp dữ liệu từ các tuần trước vào, có SPARQL endpoint sống, và một API layer đơn giản bọc lại cho ứng dụng khác gọi.

## Mục lục

| # | File | Nội dung |
|---|---|---|
| 1 | [01-vi-sao-can-triple-store.md](01-vi-sao-can-triple-store.md) | Vì sao `rdflib` + file không đủ ở quy mô thật |
| 2 | [02-cac-lua-chon.md](02-cac-lua-chon.md) | So sánh Fuseki / GraphDB / Oxigraph |
| 3 | [03-cai-dat-fuseki.md](03-cai-dat-fuseki.md) | Cài & chạy Fuseki, nạp dữ liệu |
| 4 | [04-query-qua-http.md](04-query-qua-http.md) | Query SPARQL qua HTTP từ Python |
| 5 | [05-api-layer.md](05-api-layer.md) | Lớp API (FastAPI) bọc SPARQL endpoint |
| 6 | [06-bai-tap-thuc-hanh.md](06-bai-tap-thuc-hanh.md) | Bài tập thực hành |
| 7 | [07-faq.md](07-faq.md) | Câu hỏi thường gặp |
| 8 | [08-checklist.md](08-checklist.md) | Checklist hoàn thành |

---
*Trước đó: [Tuần 3](../tuan-03/00-tong-quan.md) · Bắt đầu: [01 — Vì sao cần triple store](01-vi-sao-can-triple-store.md)*
