# Restriction, Cardinality & Defined Classes — Sức Mạnh Suy Luận Thật Sự Của OWL

## Bảng restriction cơ bản (nhắc lại)

| Restriction | Cú pháp Protégé | Ý nghĩa |
|---|---|---|
| Existential | `hasSKU some SKU` | Style này có **ít nhất 1** SKU nào đó |
| Universal | `hasSKU only SKU` | Mọi giá trị của `hasSKU` (nếu có) **phải** là SKU |
| Cardinality (exact) | `belongsToStyle exactly 1 Style` | SKU này thuộc **đúng 1** Style |
| Min cardinality | `hasSKU min 1 SKU` | Style phải có **tối thiểu 1** SKU |
| Max cardinality | `hasSKU max 5 SKU` | Style có **tối đa 5** SKU |

## Qualified Cardinality Restriction — đếm kèm điều kiện loại

Cardinality thường chỉ đếm số lượng, không quan tâm loại object. **Qualified** cardinality restriction đếm số lượng **thuộc một class cụ thể**:

```
hasSKU exactly 3 ActiveSKU
```

Khác với `hasSKU exactly 3 SKU` (đúng 3 SKU bất kỳ), câu trên nói: đúng 3 trong số các SKU đó phải thuộc class `ActiveSKU` cụ thể — có thể còn các SKU khác (đã ngừng sản xuất) không tính vào con số 3 này.

## hasValue restriction — ràng buộc một giá trị cụ thể

```
hasStatus hasValue "Active"
```

Khác `some`/`only` (ràng buộc theo *loại*), `hasValue` ràng buộc theo **một giá trị cụ thể, chính xác**. Dùng khi bạn cần diễn đạt "phải có đúng giá trị này", ví dụ một class `ApprovedOrder` yêu cầu `hasApprovalStatus hasValue "Approved"`.

## Kết hợp nhiều restriction bằng toán tử logic

```
(hasSKU some SKU) and (hasBOM some BOM) and (hasApprovalStatus hasValue "Approved")
```

Một class expression có thể gộp nhiều restriction bằng `and`/`or`/`not` — đây là nền tảng để xây **defined class**, phần quan trọng nhất của file này.

## SubClassOf (điều kiện cần) vs EquivalentClasses (điều kiện cần VÀ đủ)

Đã nhắc ở file 03: `SubClassOf` chỉ nói "nếu là A thì chắc chắn có tính chất X" — một chiều. `EquivalentClasses` nói cả hai chiều: "là A **khi và chỉ khi** có đủ tính chất X, Y, Z" — đây gọi là **defined class**.

Ví dụ cụ thể cho domain nhà máy:

```turtle
:CompleteSKU owl:equivalentClass [
    owl:intersectionOf (
        :SKU
        [ a owl:Restriction ; owl:onProperty :hasSizeLabel   ; owl:someValuesFrom xsd:string ]
        [ a owl:Restriction ; owl:onProperty :hasColor       ; owl:someValuesFrom xsd:string ]
        [ a owl:Restriction ; owl:onProperty :hasBOM         ; owl:someValuesFrom :BOM ]
    )
] .
```

Viết gọn theo cú pháp Manchester (cách Protégé hiển thị trong ô "Class description"):

```
CompleteSKU
EquivalentTo: SKU and (hasSizeLabel some xsd:string) and (hasColor some xsd:string) and (hasBOM some BOM)
```

**Điều xảy ra khi chạy reasoner:** nếu một instance SKU đã được gán đủ cả 3 property này (size, color, BOM), reasoner sẽ **tự động phân loại (classify)** instance đó vào class `CompleteSKU` — **bạn không cần tự gõ `rdf:type CompleteSKU` thủ công**. Đây là điểm khác biệt rõ ràng nhất giữa OWL và lập trình if-else thông thường: thay vì viết code kiểm tra "nếu SKU có đủ 3 field thì đánh dấu complete", bạn **khai báo một lần** điều kiện, và reasoner tự suy ra kết luận cho **mọi** instance, kể cả instance được thêm vào sau này.

## Thực hành — quan sát sức mạnh suy luận này trực tiếp

1. Tạo class `CompleteSKU` với định nghĩa `EquivalentTo` như trên (trong Protégé: chọn class → panel "Description" → "Equivalent To" thay vì "SubClass Of" → bấm `+` để thêm biểu thức).
2. Gán đủ `hasSizeLabel`, `hasColor`, `hasBOM` cho **một** SKU instance.
3. Để **một** SKU instance khác **thiếu** `hasBOM`.
4. Chạy reasoner (Reasoner → Start reasoner → Synchronize).
5. Mở tab Individuals, xem mục "Types" của từng instance: instance đủ 3 property sẽ hiện thêm `CompleteSKU` màu vàng (suy luận ra, không phải bạn tự gõ) — instance thiếu `hasBOM` thì **không** có `CompleteSKU` trong danh sách type.

Đây là bài thực hành quan trọng nhất của cả Tuần 1 — nếu làm được bước này và hiểu *tại sao* nó hoạt động, bạn đã nắm được giá trị cốt lõi mà OWL mang lại so với một schema hay taxonomy thông thường.

## General Class Inclusion (GCI) — biết tên, chưa cần đào sâu

Khi `SubClassOf` xảy ra giữa hai **class expression phức tạp** (thay vì một class đơn ở bên trái), nó được gọi là GCI. Ví dụ: `(Style and HighPriority) ⊑ (hasSKU min 2 SKU)`. Chỉ cần biết thuật ngữ này tồn tại — chưa cần tự viết GCI phức tạp ở Tuần 1, nó sẽ tự nhiên xuất hiện khi ontology lớn dần ở các tuần sau.

## Lỗi thường gặp

- **Nhầm SubClassOf với EquivalentClasses**: dùng `SubClassOf` nhưng kỳ vọng reasoner tự "suy ngược" (từ thuộc tính ra class) — chỉ `EquivalentClasses` mới có hiệu ứng phân loại tự động này.
- **Cú pháp qualified cardinality sai**: trong Protégé, quên chọn đúng class trong ô "class/datatype" khi nhập `exactly N ClassName` — nếu để trống, nó mặc định hiểu là cardinality không qualified (đếm mọi loại).
- **Tạo defined class nhưng dữ liệu hiếm khi đủ điều kiện**: khiến class đó gần như luôn trống trên thực tế dữ liệu — không phải lỗi kỹ thuật, nhưng là dấu hiệu nên xem lại điều kiện có quá chặt so với thực tế nghiệp vụ hay không.

---
*Tiếp theo: [07 — Cài đặt & làm quen Protégé](07-cai-dat-protege.md)*
