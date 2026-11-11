# Open World Assumption — Vì Sao Tư Duy SQL Sẽ "Lừa" Bạn Lúc Đầu

Đây là phần quan trọng nhất của Tuần 1 — phần lớn sự bối rối khi dân backend/AI Engineer mới học OWL đều bắt nguồn từ đây.

## Định nghĩa formal

| | Closed World Assumption (SQL quen thuộc) | Open World Assumption (OWL) |
|---|---|---|
| Nguyên tắc | Điều gì **không chứng minh được là đúng** thì coi là **sai** | Điều gì không chứng minh được là đúng **cũng không** chứng minh được là sai — coi là **chưa biết** |
| Cơ chế phủ định | *Negation as failure*: `NOT EXISTS (...)` trong SQL/Prolog — "không tìm thấy" = "không tồn tại" | *Classical negation*: phải khai tường minh (`owl:complementOf`, `disjointWith`) mới có phủ định logic thật |
| Hệ quả | Database luôn trả lời chắc chắn true/false cho mọi câu hỏi | Reasoner có thể trả lời "không xác định được" |

## Ví dụ cụ thể: domain/range từ file trước

Nếu `Style` có restriction `hasSKU min 1 SKU`, và bạn tạo một `Style` instance mới **chưa gán SKU nào** — reasoner **không báo lỗi**. Vì theo OWA, việc bạn chưa khai SKU nào không có nghĩa là Style đó *thực sự* không có SKU — có thể bạn chỉ *chưa nhập xong dữ liệu*. Reasoner luôn để ngỏ khả năng "còn thông tin chưa biết".

## Unique Name Assumption (UNA) — một "bẫy" khác, chưa nhắc tới

Ngoài OWA, OWL còn có một đặc điểm khác hay bị bỏ sót: **OWL mặc định KHÔNG giả định hai IRI khác nhau là hai thực thể khác nhau.**

Giả sử bạn có:
```turtle
:SKU001   a :SKU .
:SanPham1 a :SKU .
```

Nếu không khai gì thêm, reasoner **không tự động coi `:SKU001` và `:SanPham1` là hai sản phẩm khác nhau** — nó coi là "chưa biết, có thể đây là cùng một thực thể được gọi bằng 2 tên khác nhau". Đây gọi là **No Unique Name Assumption**.

Hai cách khai rõ ràng:
```turtle
:SKU001 owl:differentFrom :SanPham1 .     # khẳng định chắc chắn LÀ HAI THỨ khác nhau
:SKU001 owl:sameAs        :SanPham1 .     # khẳng định chắc chắn LÀ CÙNG một thực thể
```

### Vì sao điều này quan trọng với nhà máy của bạn

Khi tích hợp dữ liệu từ **ERP** và **MES** (hai hệ thống khác nhau), cùng một sản phẩm vật lý có thể có hai ID khác nhau ở hai hệ thống:

```turtle
erp:SP00123   a :SKU .
mes:PRODUCT77 a :SKU .
```

Nếu đây thực ra là **cùng một SKU**, bạn **phải** khai `owl:sameAs` để hợp nhất — nếu không, một agent AI truy vấn "SKU này có bao nhiêu lỗi QC" sẽ chỉ thấy dữ liệu từ một trong hai nguồn, bỏ sót dữ liệu ở nguồn còn lại, dù cả hai đều nói về cùng một sản phẩm thực tế. Đây là lý do UNA không phải chi tiết hàn lâm — nó ảnh hưởng trực tiếp tới độ chính xác của pipeline RAG/agent ở Tuần 5.

## OWA + no-UNA kết hợp — tình huống dễ gây bug

Giả sử bạn viết một agent AI dựa trên ontology để trả lời "liệt kê tất cả SKU của Style A123, không trùng lặp". Nếu trong dữ liệu vô tình có 2 IRI khác nhau trỏ tới cùng 1 sản phẩm thực (do nhập liệu 2 lần từ 2 nguồn, quên `owl:sameAs` hoặc quên `owl:differentFrom`), reasoner **không tự loại trùng cũng không tự gộp** — kết quả phụ thuộc hoàn toàn vào việc bạn có khai rõ quan hệ sameAs/differentFrom hay không. Agent/code phía trên ontology cần biết điều này để không "tin tưởng mù quáng" rằng số lượng SKU trả về luôn chính xác tuyệt đối nếu dữ liệu nguồn chưa sạch.

## unsatisfiable class — khi một class "không thể tồn tại"

Nếu các tiên đề bạn khai mâu thuẫn nhau tới mức **không instance nào có thể thỏa mãn** một class, reasoner sẽ xếp class đó **tương đương `owl:Nothing`**. Ví dụ: nếu bạn lỡ khai `CompleteSKU ⊑ SKU and Order` (một lỗi gõ nhầm), mà `SKU` và `Order` đã bị khai `disjointWith` nhau (file 03) — thì `CompleteSKU` trở thành class rỗng vĩnh viễn, vì không gì có thể vừa là SKU vừa là Order. Đây là dấu hiệu lỗi thiết kế, Protégé thường tô class đó **màu đỏ** trong panel sau khi chạy reasoner.

## Thực hành mở rộng

1. Tạo 2 individual SKU **khác IRI** nhưng **cùng toàn bộ dữ liệu** (cùng size, cùng color) → chạy reasoner → quan sát: nó **không** tự coi 2 cái là "cùng một thứ" hay "khác nhau" — đơn giản là không nói gì về quan hệ giữa chúng.
2. Vào tab Individuals, chọn cả nhóm individual trong 1 class → chuột phải → **"Different Individuals"** (Protégé có sẵn tính năng này để khai hàng loạt `owl:differentFrom` giữa mọi cặp trong nhóm đã chọn) → đây là thao tác rất hay dùng trong thực tế, vì khai từng cặp bằng tay sẽ tốn công khi có nhiều instance.
3. Thử ngược lại: khai `owl:sameAs` giữa 2 SKU instance khác IRI đó → chạy reasoner → quan sát Protégé "gộp" hiển thị chúng lại như cùng 1 thực thể.

## Liên hệ Tuần 6

Đây chính là lý do **SHACL** (Tuần 6) tồn tại song song với OWL: SHACL hoạt động theo **Closed World Assumption**, gần với tư duy constraint trong SQL mà bạn vốn quen — "field này bắt buộc phải có giá trị, nếu không có là lỗi, chấm hết". OWL dùng để **suy luận mở**, SHACL dùng để **validate đóng** trước khi đẩy dữ liệu vào hệ thống production. Hai công cụ bổ sung cho nhau, không thay thế nhau.

---
*Tiếp theo: [06 — Restriction, Cardinality & Defined Classes](06-restriction-cardinality.md)*
