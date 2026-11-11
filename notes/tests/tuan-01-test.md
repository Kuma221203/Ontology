# Test Tuần 1 — RDF/OWL Nền Tảng & Protégé

> Tự làm trước khi xem đáp án ở cuối file. Phần lý thuyết không cần viết dài, chỉ cần đúng ý. Phần thực hành làm trực tiếp trong Protégé, lưu thành file ontology mới.

---

## Phần 1 — Lý thuyết (10 câu)

1. Một triple RDF gồm 3 phần nào? Cho 1 ví dụ bằng domain nhà máy may (khác ví dụ đã học).

2. Vì sao IRI phải "toàn cục" thay vì chỉ cần duy nhất trong 1 hệ thống như primary key SQL? Nêu 1 tình huống cụ thể nó giải quyết được.

3. Phân biệt `SubClassOf` và `EquivalentClasses` bằng 1-2 câu. Loại nào khiến reasoner **tự động phân loại** instance vào class?

4. Property `hasSizeLabel` được khai là **Functional**. Giải thích: tại sao gán 2 giá trị khác nhau cho cùng 1 SKU lại khiến reasoner báo `inconsistent`, trong khi SKU **không hề có** giá trị `hasSizeLabel` nào lại không báo lỗi gì?

5. Giải thích Open World Assumption bằng ví dụ của riêng bạn (không dùng lại ví dụ Style/SKU đã học trong bài).

6. Unique Name Assumption là gì? Cho 1 tình huống thực tế trong nhà máy (có thể khác ví dụ ERP/MES đã học) mà việc thiếu `owl:sameAs` sẽ gây hậu quả sai lệch dữ liệu.

7. Property `partOfAssembly` được khai là **Transitive**. Nếu A partOfAssembly B, và B partOfAssembly C, reasoner suy ra điều gì? Nêu 1 rủi ro nếu khai Transitive cho một property mà thực tế không phải lúc nào cũng bắc cầu.

8. `Style owl:disjointWith Order` nghĩa là gì? Nếu không khai disjoint, điều gì (hoặc không điều gì) xảy ra khi một individual bị gán cả 2 type?

9. Cho class expression: `CompleteOrder EquivalentTo: Order and (hasStyle some Style) and (hasShipDate some xsd:date)`. Một `Order` instance có `hasStyle` nhưng **chưa có** `hasShipDate` — sau khi chạy reasoner, instance này có được xếp vào `CompleteOrder` không? Vì sao?

10. Vì sao triple store không lưu RDF dưới dạng text Turtle thô? Cơ chế nào giúp nó tiết kiệm dung lượng?

---

## Phần 2 — Thực hành (trong Protégé)

Tạo **một ontology mới** (không dùng lại file bài tập Tuần 1) cho domain: **quản lý BOM (Bill of Materials)** của nhà máy.

### Yêu cầu

1. Tạo 3 class: `BOM`, `Fabric`, `Trim`
2. Khai `Fabric owl:disjointWith Trim` (một nguyên liệu không thể vừa là vải vừa là phụ liệu)
3. Tạo object property `hasFabric` (domain `BOM`, range `Fabric`) và `hasTrim` (domain `BOM`, range `Trim`)
4. Tạo data property `hasFabricCode` (domain `Fabric`, range string), khai **Functional**
5. Tạo class `ReadyBOM` bằng **EquivalentClasses**: `BOM and (hasFabric some Fabric) and (hasTrim some Trim)`
6. Tạo 3 instance BOM:
   - `BOM_A`: có đủ cả Fabric lẫn Trim
   - `BOM_B`: chỉ có Fabric, chưa có Trim
   - `BOM_C`: có đủ Fabric + Trim, nhưng Fabric của nó bị gán **2 `hasFabricCode` khác nhau**
7. Chạy reasoner. Quan sát:
   - `BOM_A` có được xếp vào `ReadyBOM` không?
   - `BOM_B` có được xếp vào `ReadyBOM` không?
   - Ontology có báo `inconsistent` vì `BOM_C` không? Bấm "Explain" và chép lại thông điệp.
8. Dùng tab **DL Query**, gõ `hasFabric some Fabric` — liệt kê kết quả trả về.

### Nộp lại

Báo cho mình: kết quả quan sát ở bước 7 (3 câu hỏi) và nội dung Explain — mình sẽ đối chiếu với đáp án bên dưới.

---

## Đáp án (chỉ mở sau khi tự làm xong)

<details>
<summary>Bấm để xem đáp án Phần 1</summary>

1. Subject – Predicate – Object. Ví dụ: `:BOM_A :hasFabric :VaiCotton01 .`
2. Vì nhiều hệ thống (ERP, MES, GS1...) cần tham chiếu chung một thực thể mà không đụng độ tên — IRI toàn cục cho phép ghép dữ liệu từ nhiều nguồn vào chung 1 graph mà không cần bảng mapping thủ công.
3. `SubClassOf` = điều kiện cần (một chiều). `EquivalentClasses` = điều kiện cần và đủ (hai chiều) — chỉ `EquivalentClasses` khiến reasoner tự phân loại instance dựa trên dữ liệu.
4. Vì Functional là tuyên bố logic tuyệt đối "chỉ có đúng 1 giá trị" — 2 giá trị khác nhau vi phạm trực tiếp tiên đề này (mâu thuẫn chắc chắn). Thiếu giá trị không vi phạm gì — theo OWA, "chưa biết" không phải "sai".
5. (Tự đánh giá — chỉ cần đúng tinh thần: thiếu dữ liệu ≠ sai, reasoner chỉ dùng tiên đề để suy luận thêm, không coi thiếu là lỗi.)
6. (Tự đánh giá — cần nêu đúng: OWL không tự coi 2 IRI khác nhau là 2 thực thể khác nhau; thiếu `owl:sameAs` khi hợp nhất dữ liệu 2 nguồn → hệ thống/agent coi là 2 thứ riêng biệt dù là cùng 1 thực tế.)
7. Reasoner tự suy ra A partOfAssembly C. Rủi ro: nếu dữ liệu vô tình tạo vòng lặp hoặc property không thực sự bắc cầu trong mọi trường hợp, suy luận sai sẽ lan rộng khó phát hiện.
8. Disjoint nghĩa là 2 class loại trừ nhau tuyệt đối — một instance không thể vừa là Style vừa là Order. Nếu không khai, gán nhầm cả 2 type sẽ **không bị phát hiện gì cả** (OWA không tự suy ra mâu thuẫn nếu không có tiên đề loại trừ rõ ràng).
9. Không. Vì EquivalentClasses yêu cầu **đủ cả 2 điều kiện** (`hasStyle` và `hasShipDate`) — thiếu một trong hai, instance không thỏa mãn vế phải, reasoner không xếp nó vào `CompleteOrder`.
10. Vì text thô lặp lại string tốn dung lượng và chậm khi query. Cơ chế: dictionary encoding (mỗi IRI/literal duy nhất chỉ lưu 1 lần, gán ID số nguyên) + lưu triple dưới dạng bộ 3 số nguyên trong các index (SPO/POS/OSP).

</details>

<details>
<summary>Bấm để xem đáp án Phần 2</summary>

- `BOM_A` → **có** trong `ReadyBOM` (đủ cả hasFabric và hasTrim).
- `BOM_B` → **không** trong `ReadyBOM` (thiếu hasTrim — EquivalentClasses cần đủ cả 2 điều kiện).
- `BOM_C` → ontology **inconsistent**, vì Fabric của nó vi phạm `hasFabricCode` Functional (2 giá trị khác nhau cho cùng 1 Functional property). Explain sẽ chỉ thẳng vào axiom Functional của `hasFabricCode` và 2 triple gán giá trị khác nhau là nguồn gốc mâu thuẫn.
- DL Query `hasFabric some Fabric` → trả về mọi BOM đã gán ít nhất 1 Fabric (gồm BOM_A, BOM_B, BOM_C nếu cả 3 đều có hasFabric).

</details>
