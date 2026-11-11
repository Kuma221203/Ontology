# Câu Hỏi Thường Gặp — Tuần 1

**Hỏi:** Tôi tạo `Style` mới, không gán `hasSKU` nào, sao reasoner không báo lỗi dù tôi đã khai `hasSKU min 1 SKU`?
**Đáp:** Đúng như kỳ vọng theo Open World Assumption — xem [05](05-open-world-assumption.md). Việc validate "bắt buộc phải có" sẽ làm ở Tuần 6 bằng SHACL.

**Hỏi:** Vậy khai cardinality/restriction để làm gì nếu nó không validate?
**Đáp:** Để reasoner dùng trong **suy luận phân loại** (classification) — ví dụ một class `CompleteSKU` định nghĩa bằng `EquivalentClasses` kèm restriction có thể khiến reasoner **tự động** xếp instance vào class đó dựa trên dữ liệu đã nhập, như minh họa ở [06](06-restriction-cardinality.md) — hữu ích hơn validate đơn thuần vì nó *suy ra kiến thức mới*, không chỉ kiểm tra đúng/sai.

**Hỏi:** Vì sao gán 2 size cho 1 SKU (property Functional) lại bị báo lỗi, trong khi thiếu size lại không?
**Đáp:** "Functional" là tuyên bố logic tuyệt đối "không thể có 2 giá trị khác nhau cho cùng 1 subject" — nếu dữ liệu vi phạm điều này, đó là **mâu thuẫn nội tại thực sự**, reasoner luôn bắt được bất kể OWA. Nhưng "thiếu giá trị" không vi phạm gì cả về mặt logic — chỉ là "chưa biết", nên OWA cho qua.

**Hỏi:** SubClassOf và EquivalentClasses khác nhau thế nào, dùng sai thì sao?
**Đáp:** `SubClassOf` là điều kiện **cần** (một chiều): "là A thì chắc chắn có X". `EquivalentClasses` là điều kiện **cần và đủ** (hai chiều): "là A khi và chỉ khi có đủ X, Y, Z" — chỉ dạng này mới khiến reasoner **tự động phân loại** instance vào class dựa trên dữ liệu. Dùng `SubClassOf` mà kỳ vọng hiệu ứng tự phân loại là nhầm lẫn phổ biến nhất khi mới học — xem chi tiết [06](06-restriction-cardinality.md).

**Hỏi:** `owl:sameAs` dùng khi nào trong thực tế nhà máy?
**Đáp:** Khi hợp nhất dữ liệu từ hai hệ thống khác nhau (ví dụ ERP và MES) mà cùng một sản phẩm thực có hai ID khác nhau — nếu không khai `owl:sameAs`, một agent AI truy vấn trên ontology sẽ coi đó là hai sản phẩm riêng biệt và bỏ sót dữ liệu. Xem [05](05-open-world-assumption.md).

**Hỏi:** Transitive property có nguy hiểm gì không?
**Đáp:** Có — nếu khai Transitive cho một property mà dữ liệu thực tế không thực sự bắc cầu trong mọi trường hợp (hoặc vô tình tạo vòng lặp), reasoner sẽ suy luận lan rộng sai mà rất khó phát hiện, vì nó "tin tuyệt đối" vào tiên đề bạn khai. Chỉ dùng khi chắc chắn quan hệ đó luôn bắc cầu đúng về mặt nghiệp vụ. Xem [04](04-object-vs-data-property.md).

**Hỏi:** Khi nào nên dùng Object Property trỏ tới một class "giá trị" (ví dụ Color) thay vì Data Property chuỗi thô?
**Đáp:** Khi bạn cần gắn thêm thuộc tính hoặc quan hệ cho chính giá trị đó (ví dụ mã Pantone cho màu). Nếu chỉ cần lưu/hiển thị, data property chuỗi thô đơn giản hơn và nên dùng trước — chỉ nâng cấp khi thực sự cần. Xem [04](04-object-vs-data-property.md).

---
*Tiếp theo: [10 — Checklist hoàn thành](10-checklist.md)*
