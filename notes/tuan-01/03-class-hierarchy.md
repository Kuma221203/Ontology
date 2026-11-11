# Class & Class Hierarchy — Mô Hình Hóa "Loại" Trong Thế Giới Mở

## Class vs Individual

- **Class** = một *tập hợp* (khái niệm) — `Style`, `SKU`, `Order`.
- **Individual** = một *thực thể cụ thể* thuộc tập đó — `:SKU001` là một individual thuộc class `SKU`.
- `owl:Thing` = class gốc, cha của mọi class (mọi thứ trong ontology đều là `owl:Thing`).
- `owl:Nothing` = class rỗng — không instance nào có thể thuộc nó. Nếu reasoner suy ra một class của bạn **tương đương** `owl:Nothing`, nghĩa là bạn đã định nghĩa một class **không thể có instance nào tồn tại** — dấu hiệu chắc chắn có mâu thuẫn trong thiết kế (unsatisfiable class), xem thêm ở file [05](05-open-world-assumption.md).

## SubClassOf là điều kiện CẦN, không phải định nghĩa đầy đủ

Đây là điểm dễ hiểu lầm nhất khi mới học OWL:

```
ShirtStyle ⊑ Style
```

Câu này chỉ nói: *"Nếu một thứ là ShirtStyle thì chắc chắn nó cũng là Style"* (điều kiện cần). Nó **không** nói "mọi Style có đủ tính chất X, Y, Z thì là ShirtStyle" — SubClassOf không phải định nghĩa hai chiều. Muốn định nghĩa đầy đủ (điều kiện cần **và đủ**, để reasoner tự động xếp loại), phải dùng `EquivalentClasses` — khái niệm quan trọng sẽ đào sâu ở file [06-restriction-cardinality.md](06-restriction-cardinality.md), vì nó thường đi kèm restriction.

## Multiple inheritance — khác OOP bạn quen

```
RushOrderStyle ⊑ Style
RushOrderStyle ⊑ HighPriorityItem
```

Một class OWL có thể có **nhiều superclass cùng lúc** — không giống nhiều ngôn ngữ OOP (Java/C#) chỉ cho phép kế thừa đơn (single inheritance) từ class thường. Lý do: OWL class là **tập hợp theo lý thuyết tập hợp** (set theory), không phải "implementation kế thừa code" — `RushOrderStyle` đơn giản là **giao** của tập `Style` và tập `HighPriorityItem`, hoàn toàn hợp lý về mặt toán học.

## Disjoint classes — khai rõ "không thể vừa là A vừa là B"

Mặc định, OWL **không** tự động coi hai class khác nhau là loại trừ nhau. Nếu bạn lỡ gán một individual vừa `rdf:type :SKU` vừa `rdf:type :Order`, **reasoner sẽ không báo lỗi gì cả** — vì không có gì ngăn cấm điều đó theo logic bạn đã khai. Phải khai rõ:

```turtle
:SKU owl:disjointWith :Order .
```

Giờ nếu một individual bị gán cả hai type, reasoner sẽ báo **inconsistent** — đây là một ví dụ cụ thể, dễ demo, cho thấy *chỉ khi bạn khai ràng buộc rõ ràng* thì OWL mới "bắt lỗi" được, đúng với tinh thần Open World Assumption đã nhắc ở Giai đoạn 0.

**Thực hành gợi ý:** khai `SKU`, `Order`, `Style` disjoint nhau theo cặp — đây gần như luôn nên làm cho mọi class "cấp đỉnh" không có quan hệ is-a với nhau, để dữ liệu rác (gán nhầm type) bị phát hiện sớm.

## Class expression — xây class "ảo" bằng logic

OWL cho phép tạo **anonymous class** (không cần đặt tên, không cần khai riêng) bằng toán tử logic:

| Toán tử | Cú pháp Protégé | Ý nghĩa |
|---|---|---|
| Giao (AND) | `Style and Discontinued` | Vừa là Style, vừa là Discontinued |
| Hợp (OR) | `ShirtStyle or PantsStyle` | Là ShirtStyle hoặc PantsStyle (hoặc cả hai) |
| Phủ định (NOT) | `Style and not Discontinued` | Là Style nhưng không phải Discontinued |

Những class expression này dùng trực tiếp trong restriction (file 06) hoặc trong tab **DL Query** của Protégé để thử nghiệm nhanh mà không cần tạo class thật.

## owl:equivalentClass giữa hai ontology

Khi sau này bạn ghép ontology nội bộ với một ontology khác (ví dụ ontology của một brand/khách hàng), có thể cần khai:

```turtle
:Style owl:equivalentClass brand:ProductStyle .
```

Reasoner sẽ coi hai class này **hoàn toàn tương đương** — mọi instance của bên này tự động cũng là instance của bên kia. Chưa cần dùng ở Tuần 1, nhưng nên biết cơ chế này tồn tại cho việc "map ontology" sau này.

## Ví dụ hierarchy đầy đủ cho domain may mặc

```
Product
├── Style
│   ├── ShirtStyle
│   ├── PantsStyle
│   └── DressStyle
├── Fabric
└── Trim
```

**Câu hỏi thiết kế quan trọng:** khi nào nên tạo subclass riêng (`ShirtStyle`) và khi nào chỉ cần 1 class `Style` + một data property `garmentType` với giá trị enum ("shirt", "pants", "dress")?

| Tạo subclass riêng khi... | Dùng property + giá trị khi... |
|---|---|
| Mỗi loại cần restriction/axiom riêng (ví dụ chỉ `ShirtStyle` mới có `hasCollarType`) | Chỉ cần phân loại để lọc/hiển thị, không có luật logic riêng theo loại |
| Số loại cố định, ít thay đổi | Số loại có thể mở rộng liên tục theo thời gian (dễ thêm giá trị enum hơn thêm class + cập nhật mọi restriction liên quan) |
| Cần reasoner tự suy luận class membership dựa trên thuộc tính khác | Chỉ cần lưu trữ, không cần suy luận |

Nguyên tắc tránh **"class explosion"**: nếu bạn thấy mình sắp tạo hàng chục subclass chỉ khác nhau một thuộc tính, đó là dấu hiệu nên dùng data property thay vì sinh thêm class.

## Lỗi thường gặp

- **Class explosion**: tạo quá nhiều lớp con không thực sự cần axiom riêng — khiến ontology khó bảo trì, nên cân nhắc property + enum thay thế.
- **Quên khai disjoint** giữa các class cấp đỉnh không liên quan — khiến dữ liệu gán nhầm type không bao giờ bị phát hiện bởi reasoner.
- **Nhầm SubClassOf với định nghĩa đầy đủ** — mong đợi reasoner tự "suy ngược" (từ thuộc tính ra class) chỉ với SubClassOf, trong khi phải dùng EquivalentClasses mới có hiệu ứng đó (xem file 06).

---
*Tiếp theo: [04 — Object Property vs Data Property](04-object-vs-data-property.md)*
