# RDF Triple — Đơn Vị Nhỏ Nhất Của Tri Thức

## Bối cảnh: vì sao RDF tồn tại

RDF (Resource Description Framework) ra đời từ chuẩn W3C cho tầm nhìn **Semantic Web**: thay vì web chỉ là tập tài liệu để người đọc ("Web of Documents"), nó sẽ là tập **dữ liệu có cấu trúc, liên kết được với nhau để máy đọc/suy luận** ("Web of Data"). Một hệ quản trị CSDL quan hệ nhốt dữ liệu trong bảng riêng của từng ứng dụng; RDF cố tình thiết kế để dữ liệu từ hai nguồn hoàn toàn khác nhau (ví dụ ERP nội bộ và chuẩn GS1 công khai) có thể **ghép chung vào một graph** mà không cần transform phức tạp — miễn là cùng dùng IRI nhất quán (xem file [02-iri-namespace.md](02-iri-namespace.md)).

## Mô hình: một graph có hướng, có nhãn

Một tập hợp triple RDF về bản chất là một **directed labeled graph**:
- Subject và Object = các **node**
- Predicate = **cạnh có hướng, có nhãn** nối Subject → Object

```
(:SKU001) --[:belongsToStyle]--> (:StyleA123)
```

Khác với bảng SQL (hình chữ nhật cố định cột), graph không có "lược đồ cứng" — bạn có thể thêm một predicate mới cho một subject bất kỳ lúc nào mà không cần ALTER TABLE.

## Ba loại giá trị trong một triple

| Vị trí | Có thể là | Không thể là |
|---|---|---|
| **Subject** | IRI, hoặc blank node | Literal (không bao giờ) |
| **Predicate** | IRI | Literal, blank node |
| **Object** | IRI, Literal, hoặc blank node | — |

- **IRI**: định danh toàn cục cho một thực thể/khái niệm (`:SKU001`, `:belongsToStyle`).
- **Literal**: giá trị thô — chuỗi, số, ngày... luôn có thể kèm **datatype** (kiểu XSD) hoặc **language tag**:
  ```turtle
  :SKU001 :hasSizeLabel "M" .
  :SKU001 :cutDate "2026-11-20"^^xsd:date .
  :StyleA123 rdfs:label "Áo sơ mi dài tay"@vi .
  ```
- **Blank node**: một node "ẩn danh", dùng khi cần nhóm thông tin lại nhưng chưa cần (hoặc không có) một IRI riêng cho nó. Ví dụ địa chỉ giao hàng phức tạp của một Order:
  ```turtle
  :Order789 :shipTo [
      :street "123 Nguyễn Văn Cừ" ;
      :city   "TP.HCM"
  ] .
  ```
  Dấu `[...]` tạo một blank node ẩn danh có 2 thuộc tính. Dùng tiết kiệm — blank node khó tham chiếu lại từ chỗ khác trong graph, nên chỉ hợp cho dữ liệu "dùng một lần, không cần định danh riêng".

## Các serialization — cùng một dữ liệu, nhiều cách viết ra

RDF là một **mô hình dữ liệu** (data model), không phải một file format cụ thể. Cùng một tập triple có thể viết dưới nhiều cú pháp:

| Format | Đặc điểm | Khi nào dùng |
|---|---|---|
| **Turtle** (`.ttl`) | Dễ đọc bằng mắt, có prefix, phổ biến nhất khi viết tay | Ghi chú, ontology nhỏ, học tập — dùng xuyên suốt lộ trình này |
| **N-Triples** (`.nt`) | Mỗi dòng đúng 1 triple đầy đủ, không prefix, không rút gọn | Xử lý dòng-theo-dòng bằng script, dễ `grep`/`diff`, dễ ghép file |
| **RDF/XML** (`.rdf`) | Dựa trên XML, dài dòng, là format RDF *gốc* (cũ nhất) | Hệ thống legacy, một số tool Java cũ — ít dùng khi viết mới |
| **JSON-LD** (`.jsonld`) | JSON có thêm `@context` để map field → IRI | API/web app, thân thiện dev quen JSON — GS1 EPCIS (Tuần 2) dùng format này |

Cùng một triple `:SKU001 :hasSizeLabel "M"` trong 2 format:

```turtle
# Turtle
@prefix : <http://nhamay.vn/ontology/sanxuat#> .
:SKU001 :hasSizeLabel "M" .
```

```json
// JSON-LD
{
  "@context": {"hasSizeLabel": "http://nhamay.vn/ontology/sanxuat#hasSizeLabel"},
  "@id": "http://nhamay.vn/ontology/sanxuat#SKU001",
  "hasSizeLabel": "M"
}
```

Protégé lưu file mặc định ở RDF/XML, nhưng bạn có thể đổi sang Turtle lúc Save As — nên đổi, vì Turtle dễ review bằng mắt và dễ diff trong git (Tuần 6 sẽ version control ontology bằng git).

## Vì sao SPARQL nhanh dù không có "bảng"

Một triple store thường lưu trữ cùng lúc 3 (hoặc 6) chỉ mục trên cùng bộ triple: SPO (Subject-Predicate-Object), POS (Predicate-Object-Subject), OSP (Object-Subject-Predicate)... Nhờ vậy truy vấn "tìm mọi Subject có Predicate X và Object Y" hay "tìm mọi Predicate của Subject X" đều tra index trực tiếp thay vì quét toàn bộ — đây là lý do SPARQL (Tuần 3-4) chạy được trên hàng triệu triple mà không cần "thiết kế bảng" như SQL. Chi tiết triển khai không cần nhớ kỹ ở Tuần 1, chỉ cần hiểu **vì sao graph không có schema cứng vẫn truy vấn nhanh được**.

## Ví dụ đầy đủ cho domain nhà máy

```turtle
@prefix :    <http://nhamay.vn/ontology/sanxuat#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

:Order789    a            :Order ;
             :orderDate   "2026-11-01"^^xsd:date ;
             :shipTo      [ :city "TP.HCM" ] .

:StyleA123   a            :Style ;
             rdfs:label   "Áo sơ mi dài tay"@vi ;
             :partOfOrder :Order789 .

:SKU001      a            :SKU ;
             :belongsToStyle :StyleA123 ;
             :hasSizeLabel   "M" ;
             :hasColor       "Xanh navy" ;
             :cutDate        "2026-11-20"^^xsd:date .
```

`a` là viết tắt của `rdf:type` — khai instance thuộc class nào (sẽ nói kỹ ở file [03-class-hierarchy.md](03-class-hierarchy.md)).

## Lỗi thường gặp

- **Nhầm chiều predicate**: `:SKU001 :belongsToStyle :StyleA123` ≠ `:StyleA123 :belongsToStyle :SKU001` — Subject-Predicate-Object không đối xứng, viết sai chiều là sai nghĩa hoàn toàn (muốn tự động sinh chiều ngược lại, dùng `owl:inverseOf` — xem file [04](04-object-vs-data-property.md)).
- **Quên dấu `.` kết câu** trong Turtle — mỗi triple phải kết bằng `.` (trừ khi dùng `;` để nối cùng Subject hoặc `,` để nối cùng Subject-Predicate).
- **Quên khai `@prefix`** trước khi dùng — Protégé/parser sẽ báo lỗi "unknown prefix".
- **Để Object là literal nhưng Predicate đã khai là Object Property** (range là một Class) — gây lỗi hoặc bị phớt lờ, vì Object Property chỉ chấp nhận Object là IRI khác, không phải literal.

## Liên hệ các tuần sau

- **Tuần 2**: GS1 EPCIS cũng chỉ là một tập triple theo chuẩn khác — hiểu triple ở đây giúp đọc hiểu ngay file `.ttl` của GS1.
- **Tuần 3**: thao tác trực tiếp những triple này bằng `rdflib` trong Python — API của `rdflib` xoay quanh đúng khái niệm Subject/Predicate/Object vừa học.

---
*Tiếp theo: [02 — IRI & Namespace](02-iri-namespace.md)*
