# Vì Sao Cần Triple Store Thay Vì Chỉ Dùng File + `rdflib`

`rdflib` (Tuần 3) load **toàn bộ graph vào RAM** mỗi lần chạy script. Ổn với vài chục nghìn triple để học/test, nhưng có 3 vấn đề khi lên quy mô thật:

1. **Không scale** — load 600.000+ triple vào RAM mỗi lần chạy script rất chậm, lãng phí
2. **Không cho nhiều ứng dụng cùng truy vấn đồng thời** — mỗi script tự load file riêng, không có "một nguồn sự thật" sống để nhiều client cùng hỏi
3. **Không có persist/transaction đúng nghĩa database** — ghi đè file `.ttl` liên tục dễ race condition nếu 2 process ghi cùng lúc

Triple store giải quyết đúng 3 vấn đề này — và đây chính là nơi vật lý mà 600.000+ triple thực sự "sống" (đã bàn ở phần chi phí lưu trữ trước đó trong hội thoại).

## Vai trò trong kiến trúc tổng thể

```
Protégé (Tuần 1-2)        rdflib/owlready2 script (Tuần 3)         Triple store (Tuần 4)
  thiết kế TBox      →      sinh ABox từ ERP/MES         →      lưu trữ + phục vụ truy vấn sống
  (ít thay đổi)             (chạy định kỳ, vd. mỗi đêm)          (API, agent, dashboard cùng gọi)
```

Từ tuần này, `rdflib`/`owlready2` không còn đóng vai trò "lưu trữ chính" — chúng trở thành **công cụ sinh/nạp dữ liệu** vào triple store, hoặc dùng để query lại qua kết nối tới triple store (xem [04-query-qua-http.md](04-query-qua-http.md)).

---
*Tiếp theo: [02 — Các lựa chọn triple store](02-cac-lua-chon.md)*
