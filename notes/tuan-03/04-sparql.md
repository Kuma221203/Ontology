# SPARQL — Truy Vấn Graph Bằng Python

## Cú pháp cơ bản

```python
qres = g.query("""
    PREFIX nm: <http://nhamay.vn/ontology/sanxuat#>
    SELECT ?sku ?size
    WHERE {
        ?sku nm:belongsToStyle nm:StyleA123 .
        ?sku nm:hasSizeLabel ?size .
    }
""")

for row in qres:
    print(row.sku, row.size)
```

Cú pháp SPARQL gần giống SQL (`SELECT`/`WHERE`) nhưng pattern match trên **triple pattern** thay vì JOIN bảng — mỗi dòng trong `WHERE` là một mẫu triple, với biến (`?sku`, `?size`, bắt đầu bằng `?`) thay cho giá trị cụ thể. SPARQL tự tìm mọi cách gán giá trị cho các biến sao cho **toàn bộ** pattern khớp đồng thời với dữ liệu trong graph.

## FILTER — lọc theo điều kiện

```sparql
SELECT ?sku ?dueDate
WHERE {
    ?wo nm:forSKU ?sku ;
        nm:currentStage "sewing" ;
        nm:dueDate ?dueDate .
    FILTER (?dueDate < "2026-11-11"^^xsd:date)
}
```

Đây chính là cách trả lời **Competency Question #1** từ Tuần 2 ("SKU nào đang trễ tiến độ ở công đoạn may"). Lưu ý: so sánh ngày (`<`) chỉ hoạt động đúng nếu literal được khai `datatype` (`xsd:date`) — nhắc lại lỗi thường gặp ở [02-rdflib.md](02-rdflib.md).

## OPTIONAL — lấy dữ liệu có thể thiếu mà không loại bỏ dòng kết quả

```sparql
SELECT ?sku ?color
WHERE {
    ?sku a nm:SKU .
    OPTIONAL { ?sku nm:hasColor ?color }
}
```

Khác với pattern thường (nếu không khớp thì cả dòng bị loại), `OPTIONAL` giữ lại dòng kết quả dù `hasColor` chưa có giá trị — đây là biểu hiện trực tiếp của Open World Assumption trong truy vấn: "thiếu dữ liệu" không có nghĩa "loại bỏ khỏi kết quả", chỉ đơn giản trường đó trống.

## COUNT, GROUP BY — thống kê

```sparql
SELECT ?stage (COUNT(?defect) AS ?soLuong)
WHERE {
    ?qc nm:occursAtStage ?stage ;
        nm:hasDefectCode ?defect .
}
GROUP BY ?stage
ORDER BY DESC(?soLuong)
```

Trả lời **Competency Question #4** ("công đoạn nào gây nhiều lỗi KCS nhất trong tháng") — tương tự `GROUP BY`/`COUNT` trong SQL.

## Chạy từ `rdflib` vs chạy trên triple store thật

Ở Tuần 3, bạn chạy SPARQL qua `g.query(...)` — graph nằm trong RAM, load từ file. Từ Tuần 4, graph sẽ nằm trong triple store thật (Fuseki), và bạn gửi cùng câu SPARQL đó qua **HTTP request** thay vì gọi hàm Python trực tiếp — cú pháp SPARQL không đổi, chỉ đổi cách gửi đi.

---
*Tiếp theo: [05 — Bài tập thực hành](05-bai-tap-thuc-hanh.md)*
