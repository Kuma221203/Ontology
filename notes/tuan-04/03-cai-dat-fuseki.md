# Cài & Chạy Fuseki

## Cách nhanh nhất — Docker

```bash
docker run -p 3030:3030 -e ADMIN_PASSWORD=admin --name fuseki stain/jena-fuseki
```

Mở `http://localhost:3030` trên trình duyệt → đăng nhập admin → tạo dataset mới (ví dụ tên `sanxuat`).

## Nạp dữ liệu

**Cách 1 — qua giao diện web:** vào dataset `sanxuat` → tab Upload → chọn file `.ttl` đã có từ Tuần 1-3 (ontology + instance data) → Upload.

**Cách 2 — qua `curl` (tiện cho script tự động hóa sau này):**

```bash
curl -X POST -H "Content-Type: text/turtle" \
  --data-binary @sanxuat.ttl \
  http://localhost:3030/sanxuat/data
```

Sau khi nạp xong, bạn có một **SPARQL endpoint sống**: `http://localhost:3030/sanxuat/sparql`

## Không dùng Docker — cài trực tiếp

Nếu máy không có Docker: tải bản standalone từ [jena.apache.org/documentation/fuseki2](https://jena.apache.org/documentation/fuseki2/) (cần Java 11+ cài sẵn), giải nén, chạy `fuseki-server` — giao diện và cách dùng giống hệt bản Docker.

## Kiểm tra endpoint hoạt động

Vào tab **Query** trên giao diện web Fuseki, thử ngay một câu SPARQL đơn giản:

```sparql
SELECT * WHERE { ?s ?p ?o } LIMIT 10
```

Nếu trả về 10 dòng dữ liệu bất kỳ, endpoint đã hoạt động đúng.

---
*Tiếp theo: [04 — Query qua HTTP](04-query-qua-http.md)*
