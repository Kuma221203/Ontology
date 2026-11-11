# `rdflib` — Thao Tác RDF Graph Bằng Python

`rdflib` là thư viện Python chuẩn cho RDF — API xoay quanh đúng khái niệm Subject/Predicate/Object đã học Tuần 1 ([01-rdf-triple.md](../tuan-01/01-rdf-triple.md)).

## Load, thêm triple, lưu lại

```python
from rdflib import Graph, Namespace, Literal, RDF, XSD

g = Graph()
NM = Namespace("http://nhamay.vn/ontology/sanxuat#")
g.bind("nm", NM)

# Load ontology đã thiết kế ở Protégé
g.parse("sanxuat.ttl", format="turtle")

# Thêm triple mới bằng code (tương đương gõ tay trong Protégé)
g.add((NM.SKU001, RDF.type, NM.SKU))
g.add((NM.SKU001, NM.hasSizeLabel, Literal("M")))
g.add((NM.SKU001, NM.cutDate, Literal("2026-11-20", datatype=XSD.date)))

g.serialize(destination="sanxuat_updated.ttl", format="turtle")
```

**Điểm quan trọng:** `g.add((s, p, o))` chính là thêm **một triple** — code này literally là cách bạn "gõ" vào graph, chỉ khác là làm được hàng loạt qua vòng lặp thay vì click chuột trong Protégé.

## Populate hàng loạt từ dữ liệu thật

```python
import csv

with open("orders_export.csv") as f:
    for row in csv.DictReader(f):
        sku_uri = NM[row["sku_id"]]
        g.add((sku_uri, RDF.type, NM.SKU))
        g.add((sku_uri, NM.hasSizeLabel, Literal(row["size"])))
        g.add((sku_uri, NM.hasColor, Literal(row["color"])))
        g.add((sku_uri, NM.belongsToStyle, NM[row["style_id"]]))
```

Đây chính là cách 100.000 SKU "biến thành" triple — không ai nhập tay, script đọc file export từ ERP (CSV/JSON/DB query trực tiếp) rồi sinh triple theo đúng vocabulary đã thiết kế ở Tuần 1-2.

## Các thao tác thường dùng khác

```python
# Xóa 1 triple
g.remove((NM.SKU001, NM.hasColor, Literal("Xanh navy")))

# Kiểm tra 1 triple có tồn tại không
exists = (NM.SKU001, RDF.type, NM.SKU) in g

# Duyệt mọi triple của 1 subject
for p, o in g.predicate_objects(NM.SKU001):
    print(p, o)

# Đếm tổng số triple trong graph
print(len(g))
```

## Lỗi thường gặp

- **Quên `g.bind()`** — vẫn hoạt động nhưng khi serialize ra sẽ dùng IRI đầy đủ thay vì prefix ngắn, khó đọc.
- **Dùng `Literal()` không khai `datatype`** cho ngày/số — `rdflib` sẽ coi là `xsd:string` mặc định, có thể gây sai lệch khi query SPARQL với `FILTER` so sánh ngày/số (xem [04-sparql.md](04-sparql.md)).
- **Load file sai `format`** — Turtle phải khai `format="turtle"`, JSON-LD là `format="json-ld"`, nhầm format sẽ báo lỗi parse ngay.

---
*Tiếp theo: [03 — owlready2](03-owlready2.md)*
