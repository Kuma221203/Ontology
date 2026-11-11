# SHACL — Validate Theo Closed World

## Nhắc lại vì sao cần SHACL

Xuyên suốt Tuần 1, bạn đã học đi học lại một điểm: **OWL/reasoner không tự validate "bắt buộc phải có"** vì hoạt động theo Open World Assumption — thiếu dữ liệu chỉ là "chưa biết", không phải "sai" (xem lại [05-open-world-assumption.md](../tuan-01/05-open-world-assumption.md)). Điều này đúng về mặt logic, nhưng **không phù hợp** khi bạn cần trả lời câu hỏi rất thực tế: *"dữ liệu này có đủ để đẩy vào production không?"*

**SHACL (Shapes Constraint Language)** sinh ra để trả lời đúng câu hỏi đó — nó hoạt động theo **Closed World Assumption**, gần với tư duy `NOT NULL`/constraint trong SQL mà bạn vốn quen.

## So sánh trực tiếp OWL Restriction vs SHACL Shape

| | OWL Restriction (Tuần 1) | SHACL Shape |
|---|---|---|
| Giả định | Open World — thiếu dữ liệu = chưa biết | Closed World — thiếu dữ liệu = **vi phạm** |
| Mục đích | Suy luận (inference) — phân loại instance, phát hiện mâu thuẫn logic | Validate (kiểm tra) — báo lỗi rõ ràng, đúng/sai |
| Dùng khi | Thiết kế TBox, suy luận defined class (`CompleteSKU`) | Kiểm tra dữ liệu ABox trước khi ghi vào hệ thống production |
| Kết quả | Instance được xếp vào class, hoặc ontology `inconsistent` | Danh sách lỗi cụ thể: thiếu trường nào, ở instance nào |

## Viết một SHACL Shape

```turtle
@prefix sh:  <http://www.w3.org/ns/shacl#> .
@prefix nm:  <http://nhamay.vn/ontology/sanxuat#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

nm:WorkOrderShape
    a sh:NodeShape ;
    sh:targetClass nm:WorkOrder ;
    sh:property [
        sh:path nm:dueDate ;
        sh:datatype xsd:date ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
        sh:message "WorkOrder phải có đúng 1 dueDate." ;
    ] ;
    sh:property [
        sh:path nm:assignedLine ;
        sh:minCount 1 ;
        sh:message "WorkOrder phải được gán cho một chuyền sản xuất." ;
    ] .
```

So với OWL restriction `hasSKU min 1 SKU` (chỉ dùng để suy luận, không báo lỗi khi thiếu), `sh:minCount 1` ở đây **thực sự báo lỗi** nếu một `WorkOrder` không có `dueDate` — đúng kỳ vọng tư duy database bạn quen.

## Chạy validate bằng Python

```python
from pyshacl import validate

conforms, results_graph, results_text = validate(
    data_graph="sanxuat_data.ttl",
    shacl_graph="sanxuat_shapes.ttl",
    inference="rdfs"
)
print(conforms)       # True/False
print(results_text)   # Danh sách lỗi cụ thể nếu có
```

## Khi nào dùng OWL, khi nào dùng SHACL — không phải chọn 1 trong 2

| Việc cần làm | Công cụ |
|---|---|
| "Nếu SKU có đủ size+color+BOM thì nó LÀ CompleteSKU" (suy luận ra kiến thức mới) | OWL (`EquivalentClasses`, Tuần 1) |
| "Mọi WorkOrder BẮT BUỘC phải có dueDate trước khi lưu vào hệ thống" (validate trước khi ghi) | SHACL |
| Phát hiện 2 class loại trừ nhau bị gán nhầm (`disjointWith`) | OWL |
| Phát hiện thiếu trường bắt buộc khi LLM extract dữ liệu sai (Tuần 5) | SHACL |

Hai công cụ **bổ sung cho nhau**, không thay thế nhau — pipeline thực tế thường chạy SHACL **ngay sau** bước LLM extraction (Tuần 5) để chặn dữ liệu thiếu trước khi ghi vào triple store, trong khi OWL vẫn giữ vai trò suy luận trong toàn bộ vòng đời.

---
*Tiếp theo: [02 — Versioning với ROBOT](02-versioning-robot.md)*
