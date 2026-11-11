# Query SPARQL Qua HTTP Từ Python

```python
import requests

query = """
PREFIX nm: <http://nhamay.vn/ontology/sanxuat#>
SELECT ?sku ?size WHERE {
    ?sku nm:belongsToStyle nm:StyleA123 ;
         nm:hasSizeLabel ?size .
}
"""

r = requests.get(
    "http://localhost:3030/sanxuat/sparql",
    params={"query": query},
    headers={"Accept": "application/sparql-results+json"}
)
print(r.json())
```

## Khác biệt cốt lõi so với Tuần 3

| Tuần 3 (`rdflib` + file) | Tuần 4 (triple store + HTTP) |
|---|---|
| `g.parse("sanxuat.ttl")` — tự load file vào RAM | Gửi HTTP request tới dịch vụ đang chạy riêng |
| Mỗi script giữ bản copy graph riêng trong RAM | Một nguồn dữ liệu duy nhất, nhiều client cùng gọi |
| Mất dữ liệu nếu không tự serialize lại file | Dữ liệu persist bền vững trong triple store |

Cú pháp SPARQL **giữ nguyên** — chỉ đổi cách gửi câu query đi (gọi hàm Python trực tiếp → gửi HTTP request). Đây là lý do SPARQL học ở Tuần 3 áp dụng thẳng được sang Tuần 4 mà không cần học lại.

## Kết quả trả về dạng JSON

```json
{
  "results": {
    "bindings": [
      {"sku": {"value": "http://nhamay.vn/ontology/sanxuat#SKU001"}, "size": {"value": "M"}},
      {"sku": {"value": "http://nhamay.vn/ontology/sanxuat#SKU002"}, "size": {"value": "L"}}
    ]
  }
}
```

Cấu trúc JSON chuẩn SPARQL 1.1 Results Format — giống nhau trên mọi triple store (Fuseki, GraphDB, Oxigraph...), nên code Python đọc kết quả không cần sửa nếu sau này đổi triple store khác.

---
*Tiếp theo: [05 — API layer](05-api-layer.md)*
