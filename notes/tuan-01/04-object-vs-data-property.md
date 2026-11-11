# Object Property vs Data Property — Và Các Đặc Tính Logic Của Property

## Phân biệt cơ bản

| Loại | Nối gì với gì | Ví dụ |
|---|---|---|
| **Object Property** | Instance ↔ Instance khác trong ontology | `:belongsToStyle` nối `SKU001` → `StyleA123` |
| **Data Property** | Instance ↔ giá trị thô (số, chuỗi, ngày) | `:hasSizeLabel` nối `SKU001` → `"M"` |

Quy tắc phân biệt nhanh: bên phải là **một thực thể khác trong ontology** → object property; bên phải là **con số/chuỗi/ngày** → data property.

## Domain/Range là suy luận, không phải validate

Mỗi property có **Domain** (ai có quyền "có" property này) và **Range** (giá trị bên kia phải thuộc loại gì). Ví dụ `hasSKU` domain = `Style`, range = `SKU`.

**Điều cần nhớ:** nếu bạn lỡ gán `hasSKU` cho một individual chưa khai type (hoặc khai type khác `Style`), reasoner **không báo lỗi ngay** — nó **suy luận thêm** rằng individual đó cũng phải là `Style` (vì domain nói vậy). Chỉ khi điều suy luận đó **mâu thuẫn** với một tiên đề khác (ví dụ individual đó đã bị khai `disjointWith Style`) thì mới thành `inconsistent`. Đây là hệ quả trực tiếp của Open World Assumption — xem sâu hơn ở file [05](05-open-world-assumption.md).

## Catalogue đầy đủ các đặc tính logic của property

| Đặc tính | Ý nghĩa | Ví dụ nhà máy |
|---|---|---|
| **Functional** | Mỗi subject chỉ có **đúng 1** giá trị | `hasSizeLabel` — một SKU chỉ có 1 size |
| **Inverse Functional** | Mỗi *giá trị* chỉ thuộc về **đúng 1** subject (chiều ngược) | `hasBarcode` — mỗi mã vạch chỉ gắn với đúng 1 SKU |
| **Symmetric** | Nếu A→B thì tự suy B→A | `relatedStyleTo` — hai style gợi ý "mua kèm" nhau, quan hệ 2 chiều |
| **Asymmetric** | Nếu A→B thì **chắc chắn không** có B→A | `partOfOrder` — SKU thuộc Order thì Order không thể "thuộc" SKU |
| **Transitive** | Nếu A→B và B→C thì tự suy A→C | `partOfAssembly` — bộ phận A thuộc cụm B, cụm B thuộc sản phẩm C → A thuộc C |
| **Reflexive** | Mọi thứ tự quan hệ với chính nó | Hiếm dùng trong domain này |
| **Irreflexive** | Không thứ gì quan hệ với chính nó | `dependsOnStep` — một công đoạn không thể phụ thuộc chính nó |

**Cảnh báo về Transitive:** đây là đặc tính dễ gây suy luận "bùng nổ" ngoài ý muốn nhất. Nếu bạn khai `partOfAssembly` là transitive nhưng dữ liệu vô tình tạo thành vòng lặp (A thuộc B, B thuộc A), reasoner sẽ rơi vào suy luận vô hạn hoặc báo lỗi khó debug. Chỉ khai Transitive khi chắc chắn quan hệ đó **thực sự** bắc cầu trong mọi trường hợp.

## Sub-property — property cũng có thể "kế thừa"

```turtle
:hasActiveSKU rdfs:subPropertyOf :hasSKU .
```

Nếu `StyleA123 :hasActiveSKU :SKU001`, reasoner tự suy ra `StyleA123 :hasSKU :SKU001` luôn (vì mọi active SKU cũng là một SKU thường). Hữu ích khi bạn cần property "chi tiết hơn" nhưng vẫn muốn truy vấn chung bằng property cha.

## Property chain — suy luận gián tiếp qua nhiều bước

Một tính năng mạnh ít người biết khi mới học: bạn có thể định nghĩa một property được suy ra từ **chuỗi nhiều property khác**:

```
hasSKU o hasDefect → hasKnownDefect
```

Đọc là: nếu `Style hasSKU SKU` và `SKU hasDefect Defect`, thì tự động suy ra `Style hasKnownDefect Defect` — không cần bạn gán tay quan hệ gián tiếp này. Property chain hữu ích khi bạn muốn truy vấn trực tiếp "Style nào có lỗi gì" mà không cần JOIN 2 bước trong SPARQL. Chưa cần dùng ở Tuần 1, nhưng nên biết để nhận ra khi cần ở Tuần 3-4.

## Khi nào nên biến Data Property thành Object Property trỏ tới Class

Ví dụ: `hasColor` hiện là data property, giá trị string `"Xanh navy"`. Nếu sau này nghiệp vụ cần thêm thông tin về màu (mã Pantone, nhóm màu để báo cáo), chuỗi thô không đủ biểu diễn. Giải pháp: biến `Color` thành một **class** riêng, có các instance (`:MauXanhNavy`), rồi dùng object property `hasColorValue` trỏ tới instance đó:

```turtle
# Trước (data property, string thô)
:SKU001 :hasColor "Xanh navy" .

# Sau (object property, trỏ tới instance có thêm thuộc tính)
:SKU001        :hasColorValue :MauXanhNavy .
:MauXanhNavy   a              :Color ;
               rdfs:label     "Xanh navy" ;
               :pantoneCode   "19-4052 TPX" .
```

| Data property (string thô) | Object property (trỏ class) |
|---|---|
| Đơn giản, nhanh, đủ cho hiển thị | Phức tạp hơn, nhưng mở rộng được (gắn thêm thuộc tính cho giá trị) |
| Không truy vấn được "các SKU cùng nhóm màu" dễ dàng | Truy vấn nhóm/liên kết dễ dàng qua object property |

**Nguyên tắc:** bắt đầu với data property (đơn giản, như đã làm ở bài tập Tuần 1). Chỉ nâng cấp lên object property khi thực sự cần thêm thuộc tính hoặc quan hệ cho chính giá trị đó — tránh over-engineer ngay từ đầu.

## Lỗi thường gặp

- **Khai Transitive cho property không thực sự bắc cầu trong mọi trường hợp** — gây suy luận sai lan rộng khắp ontology, rất khó phát hiện vì reasoner "tin" tuyệt đối vào tiên đề bạn khai.
- **Quên khai Domain/Range** — khiến property "lỏng lẻo", reasoner không có gì để suy luận thêm, mất đi một phần lợi ích của ontology so với vocabulary thuần.
- **Dùng Functional cho property thực ra có thể nhiều giá trị** — ví dụ nhầm `hasDefect` là Functional trong khi một SKU hoàn toàn có thể có nhiều lỗi cùng lúc, dẫn tới reasoner báo inconsistent sai chỗ khi bạn gán 2 defect.

---
*Tiếp theo: [05 — Open World Assumption](05-open-world-assumption.md)*
