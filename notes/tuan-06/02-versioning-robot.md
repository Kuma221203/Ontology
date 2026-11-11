# Version Control Ontology Bằng Git + ROBOT

## Vì sao TBox đưa vào git được, ABox thì không

Nhắc lại phân biệt TBox/ABox đã bàn trong hội thoại trước: **TBox** (ontology — class, property, restriction) nhỏ, ít thay đổi, là file text `.ttl` vài trăm dòng — hoàn toàn hợp để commit vào git như mọi file code khác. **ABox** (600.000+ triple dữ liệu thật) sống trong triple store (Tuần 4), không đưa vào git — chỉ **script sinh ra nó** mới commit.

## Lưu TBox dạng Turtle để diff được

Khi Save As trong Protégé, luôn chọn **Turtle Syntax** thay vì RDF/XML mặc định (đã nhắc ở [07-cai-dat-protege.md](../tuan-01/07-cai-dat-protege.md)) — Turtle đọc được bằng mắt, `git diff` hiển thị rõ ràng từng dòng thay đổi, trong khi RDF/XML sinh ra diff khó đọc do cấu trúc XML lồng nhau thay đổi linh tinh dù nội dung semantic chỉ đổi 1 chỗ nhỏ.

```bash
git add sanxuat.ttl
git commit -m "Thêm class WorkOrder, restriction CompleteSKU"
```

## Vấn đề: Protégé có thể sắp xếp lại thứ tự block khi save, gây diff "giả"

Ngay cả khi dùng Turtle, Protégé đôi khi sắp xếp lại thứ tự các block khi lưu, khiến `git diff` hiển thị hàng trăm dòng thay đổi dù về mặt ý nghĩa chỉ đổi 1 axiom nhỏ — đây là lúc cần **ROBOT**.

## ROBOT — công cụ dòng lệnh chuyên cho version control ontology

[ROBOT](http://robot.obolibrary.org/) là công cụ CLI phổ biến trong cộng đồng ontology (ban đầu từ domain y sinh — OBO Foundry — nhưng dùng chung được cho mọi ontology OWL):

```bash
# Chuẩn hóa thứ tự + format trước khi commit, để diff luôn sạch
robot convert --input sanxuat.ttl --output sanxuat.ttl --format ttl

# So sánh 2 version ontology ở mức NGỮ NGHĨA (không phải diff text thô)
robot diff --left v1.0/sanxuat.ttl --right v2.0/sanxuat.ttl --output changes.txt
```

`robot diff` so sánh **axiom thực sự thay đổi** (thêm/bớt class, property, restriction) — khác `git diff` chỉ so sánh text dòng-theo-dòng, nên không bị "nhiễu" bởi việc Protégé sắp xếp lại thứ tự block.

## Chiến lược versioning cho project

1. Mỗi lần thay đổi TBox có ý nghĩa (thêm class, đổi restriction) → `robot convert` chuẩn hóa format trước khi commit
2. Commit message mô tả rõ **thay đổi logic**, không chỉ "update ontology" (ví dụ: "Thêm defined class CompleteSKU, khai SKU/Order disjoint")
3. Khi thay đổi lớn (đổi cấu trúc nhiều class cùng lúc), cân nhắc tag version (`v1.0`, `v2.0`) theo convention IRI đã bàn ở [02-iri-namespace.md](../tuan-01/02-iri-namespace.md) — nếu namespace có version trong path, mỗi version lớn có thể có IRI riêng để không phá vỡ dữ liệu cũ tham chiếu bản trước

---
*Tiếp theo: [03 — Đánh giá chất lượng](03-danh-gia-chat-luong.md)*
