# Bài Tập Thực Hành Tuần 1 (Đầy Đủ)

Làm theo thứ tự Phần A → B → C. Mỗi phần build tiếp trên cùng 1 file ontology, đừng tạo file mới mỗi phần.

## Phần A — Mini-ontology cơ bản

1. Tạo 3 class: `Style`, `SKU`, `Order`
2. Tạo object property `hasSKU` (domain = `Style`, range = `SKU`)
3. Tạo object property `partOfOrder` (domain = `Style`, range = `Order`)
4. Tạo data property `hasSizeLabel` (domain = `SKU`, range = string), `hasColor` (domain = `SKU`, range = string)
5. Tạo vài instance: 1 `Order`, 1 `Style` thuộc Order đó, 2-3 `SKU` thuộc Style đó (size/màu khác nhau)
6. Chạy reasoner HermiT (menu Reasoner → Start reasoner) — không có cảnh báo đỏ là thành công

*(Lý thuyết liên quan: [01](01-rdf-triple.md), [03](03-class-hierarchy.md), [04](04-object-vs-data-property.md))*

## Phần B — Restriction, Cardinality & kiểm tra OWA

7. Tab Object Properties → chọn `belongsToStyle` (tạo mới nếu chưa có) → "Characteristics" → tick **Inverse Of** → chọn `hasSKU`
8. Tab Classes → chọn `SKU` → "SubClass Of" → thêm: `belongsToStyle exactly 1 Style`
9. Tab Classes → chọn `Style` → "SubClass Of" → thêm: `hasSKU min 1 SKU`
10. Tab Data Properties → chọn `hasSizeLabel` → tick **Functional**
11. Chạy lại reasoner (Reasoner → Start reasoner → Synchronize reasoner nếu cần)
12. **Thử cố tình phá nó:** chọn 1 instance SKU đã có `hasSizeLabel "M"`, gán thêm `hasSizeLabel "L"` → chạy reasoner → phải thấy cảnh báo đỏ "inconsistent ontology" → bấm "Explain" để xem nó chỉ ra property nào gây mâu thuẫn

*(Lý thuyết liên quan: [04](04-object-vs-data-property.md), [06](06-restriction-cardinality.md))*

Bước 12 là bài học quan trọng: thấy rõ **khi nào** OWL thực sự tự bắt lỗi (ràng buộc tuyệt đối như Functional kết hợp dữ liệu tự mâu thuẫn) và **khi nào** nó im lặng dù thiếu dữ liệu (OWA thuần túy — bước 9 không tự báo lỗi nếu bạn tạo Style mà quên gán SKU nào, xem [05](05-open-world-assumption.md)).

## Phần C — Disjoint, Defined Class & UNA (mở rộng)

13. Khai `SKU owl:disjointWith Order` → tạo (hoặc chọn) 1 individual, gán **cả hai** type `SKU` và `Order` → chạy reasoner → quan sát nó báo `inconsistent`, bấm "Explain" để thấy đúng disjoint axiom gây ra mâu thuẫn. *(Liên quan [03](03-class-hierarchy.md))*
14. Tạo class mới `CompleteSKU`, định nghĩa bằng **Equivalent To** (không phải SubClass Of): `SKU and (hasSizeLabel some xsd:string) and (hasColor some xsd:string)`. Gán đủ 2 property này cho 1 SKU instance, để 1 SKU khác thiếu `hasColor`. Chạy reasoner → mở tab Individuals → xem instance đủ property có xuất hiện thêm type `CompleteSKU` (màu vàng, do suy luận) hay không, instance thiếu property thì không. *(Liên quan [06](06-restriction-cardinality.md))*
15. Tạo 2 individual SKU **khác IRI** nhưng **cùng toàn bộ dữ liệu** (size, color giống hệt nhau) → chạy reasoner → xác nhận nó **không** tự coi 2 cái là "cùng một thứ". Sau đó vào tab Individuals, chọn cả 2 (hoặc cả nhóm SKU) → chuột phải → **"Different Individuals"** để khai rõ chúng khác nhau. *(Liên quan [05](05-open-world-assumption.md))*
16. Mở tab **DL Query**, gõ `hasSKU some SKU` vào ô Query → bấm Execute → xem nó liệt kê ngay những Style nào thỏa điều kiện này, không cần tạo class mới cho việc thử nghiệm. Thử thêm `hasSKU min 2 SKU` để xem danh sách thay đổi thế nào. *(Liên quan [07](07-cai-dat-protege.md))*

Làm xong cả 3 phần, báo lại kết quả — đặc biệt nội dung "Explain" ở bước 12 và 13, và việc instance có được xếp vào `CompleteSKU` đúng như kỳ vọng ở bước 14 hay không.

---
*Tiếp theo: [09 — Câu hỏi thường gặp](09-faq.md)*
