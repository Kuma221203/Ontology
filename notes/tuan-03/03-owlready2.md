# `owlready2` — Thao Tác OWL + Chạy Reasoner Từ Python

Khác `rdflib` (thao tác RDF graph ở mức triple thô), `owlready2` cho phép thao tác ontology ở mức **class/individual như object Python**, và quan trọng nhất: **chạy reasoner HermiT/Pellet ngay trong code**, không cần mở Protégé.

## Load ontology, thao tác như object Python

```python
from owlready2 import get_ontology, sync_reasoner_hermit

onto = get_ontology("file://./sanxuat.owl").load()

Style = onto.Style
SKU = onto.SKU

# Tạo instance như tạo object Python
style_a = Style("StyleA123")
sku001 = SKU("SKU001")
sku001.belongsToStyle = [style_a]
sku001.hasSizeLabel = ["M"]
```

Lưu ý cú pháp: property trong `owlready2` là **list** (vì OWL không mặc định Functional trừ khi bạn khai — nhớ lại Tuần 1), kể cả khi thực tế chỉ có 1 giá trị.

## Chạy reasoner từ code

```python
with onto:
    sync_reasoner_hermit()

# Kiểm tra suy luận: SKU001 có được xếp vào CompleteSKU không?
print(onto.CompleteSKU in sku001.is_a)
```

Đoạn `with onto: sync_reasoner_hermit()` tương đương bấm **Reasoner → Start reasoner → Synchronize** trong Protégé — chỉ khác là chạy tự động trong pipeline, không cần người ngồi bấm nút.

## So sánh `rdflib` vs `owlready2`

| | `rdflib` | `owlready2` |
|---|---|---|
| Mức trừu tượng | Triple thô (Subject/Predicate/Object) | Class/Individual như object Python |
| Chạy reasoner | Không — chỉ thao tác graph | **Có** — gọi HermiT/Pellet trực tiếp |
| SPARQL | Hỗ trợ tốt, đầy đủ | Hỗ trợ hạn chế hơn |
| Dùng khi | Populate hàng loạt, SPARQL, serialize | Cần kiểm tra suy luận/consistency từ code |

Thực tế thường dùng **cả hai song song**: `owlready2` để thiết kế/kiểm tra logic (đặc biệt lúc thêm class/restriction mới cần xác nhận không gây inconsistent), `rdflib` để populate hàng loạt và chạy SPARQL truy vấn.

## Lưu ý về hiệu năng reasoner

Như đã bàn ở phần "chi phí lưu trữ/TBox vs ABox" trước đó: chạy `sync_reasoner_hermit()` trên ontology có **hàng trăm nghìn individual** sẽ chậm, có thể mất nhiều phút hoặc hơn. Trong pipeline thực tế (Tuần 6 sẽ nói kỹ hơn), thường chỉ chạy reasoner đầy đủ trên **TBox** hoặc một tập dữ liệu mẫu nhỏ để xác nhận logic đúng, không chạy lại trên toàn bộ ABox mỗi lần có thay đổi nhỏ.

---
*Tiếp theo: [04 — SPARQL](04-sparql.md)*
