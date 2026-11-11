# IRI & Namespace — Định Danh Toàn Cục Cho Mọi Thứ

## So sánh với primary key — vì sao phải "toàn cục"

Trong SQL, một `order_id` chỉ cần duy nhất **trong phạm vi một bảng** (hoặc một DB). Khi hai hệ thống khác nhau (ví dụ ERP và MES của nhà máy) cùng có khái niệm "SKU", chúng hoàn toàn có thể trùng mã số mà là hai thứ khác nhau — muốn ghép dữ liệu phải JOIN thủ công qua bảng mapping.

IRI (Internationalized Resource Identifier — bản mở rộng Unicode của URI/URL) được thiết kế để **duy nhất trên toàn cầu**, không chỉ trong một hệ thống:

```
http://nhamay.vn/ontology/sanxuat#Style
http://nhamay.vn/ontology/sanxuat#hasSKU
```

Nhờ vậy, khi bạn import ontology GS1 EPCIS (Tuần 2) vào cùng một file với ontology nội bộ, `gs1:TransformationEvent` và `nhamay:WorkOrder` nằm **chung một graph** mà không bao giờ đụng độ tên — vì namespace khác nhau. Đây là nền tảng cho việc "ghép nối tri thức" mà không cần ETL phức tạp.

## Cấu trúc một IRI

```
http://nhamay.vn/ontology/sanxuat#Style
└─scheme┘└────authority────┘└───path───┘└fragment┘
```

Hai kiểu namespace phổ biến, khác nhau ở ký tự cuối:

| Kiểu | Ví dụ | Ưu điểm | Nhược điểm |
|---|---|---|---|
| **Hash-namespace** (`#`) | `.../sanxuat#Style` | Toàn bộ ontology tải về trong 1 lần request (mọi thứ sau `#` chỉ là fragment của cùng 1 tài liệu) | Không dereference được *từng* resource riêng lẻ qua HTTP |
| **Slash-namespace** (`/`) | `.../sanxuat/Style` | Mỗi resource có thể trả về dữ liệu riêng khi GET (đúng tinh thần Linked Data) | Cần server thật xử lý mỗi path — phức tạp hơn để vận hành |

Với một ontology nội bộ nhỏ như project này, **hash-namespace** (`#`) là lựa chọn thực dụng — Protégé mặc định dùng kiểu này.

## IRI không bắt buộc phải "sống"

IRI không nhất thiết phải trả về nội dung gì khi bạn mở nó trên trình duyệt — nó chỉ cần **duy nhất và nhất quán**, giống một UUID. `http://nhamay.vn/ontology/sanxuat#Style` hoạt động tốt trong Protégé dù `nhamay.vn` không tồn tại server thật. Nhưng nếu sau này bạn muốn ontology trở thành **Linked Data thực thụ** — nơi một agent AI khác có thể "bấm vào" IRI đó và tự tải về định nghĩa — thì cần dựng server trả RDF khi GET đúng IRI đó. Không bắt buộc ở giai đoạn học này.

## Chọn namespace cho project

Ví dụ namespace: `http://nhamay.vn/ontology/sanxuat#`

Những điều nên cân nhắc:
- **Dùng domain thật của công ty** (nếu có) thay vì domain ngẫu nhiên — giúp IRI trông "chính chủ", tránh trùng với tổ chức khác.
- **Versioning**: đặt version vào path (`.../sanxuat/1.0#Style`) để khi ontology thay đổi cấu trúc lớn ở các bản sau, bạn có thể phát hành `2.0` song song mà không phá vỡ dữ liệu cũ còn tham chiếu `1.0`. Với project học tập quy mô nhỏ, có thể bỏ qua version ở version đầu, nhưng nên biết convention này tồn tại trước khi ontology lớn dần qua các tuần.

## Prefix — viết tắt, không phải namespace khác

Trong Turtle/Protégé, bạn không gõ IRI đầy đủ mỗi lần — khai `@prefix` một lần rồi dùng tên ngắn:

```turtle
@prefix :    <http://nhamay.vn/ontology/sanxuat#> .
@prefix gs1: <https://ref.gs1.org/epcis/> .

:SKU001 a :SKU .
```

`:` (prefix rỗng) thường được dùng làm namespace "chính" của ontology đang viết — mọi IRI không có tiền tố rõ ràng sẽ dùng prefix rỗng này.

## Ví dụ collision — vì sao namespace cứu bạn

Giả sử cả ontology nội bộ và GS1 đều có khái niệm tên ngắn là `Style`:

```turtle
@prefix :    <http://nhamay.vn/ontology/sanxuat#> .
@prefix gs1: <https://ref.gs1.org/voc/> .

:Style      a owl:Class .        # IRI thật: http://nhamay.vn/ontology/sanxuat#Style
gs1:Style   a owl:Class .        # IRI thật: https://ref.gs1.org/voc/Style — HOÀN TOÀN khác
```

Hai `Style` này là hai thực thể khác nhau trong graph — không hề xung đột, dù cùng hiển thị tên ngắn "Style" trên Protégé. Nếu bạn *muốn* chúng là cùng một khái niệm (ví dụ ontology của bạn tương đương với class của GS1), phải khai rõ `owl:equivalentClass` — không có gì tự động ngầm hiểu điều đó.

## PURL / w3id.org (biết trước, chưa cần dùng)

Cộng đồng ontology hay gặp vấn đề: domain công ty đổi, ontology cũ "chết" IRI. Giải pháp phổ biến là dùng dịch vụ redirect bền vững như **w3id.org** làm namespace thay vì domain công ty trực tiếp — ví dụ `https://w3id.org/nhamay/sanxuat#`. Không bắt buộc ở bước học này, chỉ cần biết để cân nhắc khi ontology lên production thật.

## Lỗi thường gặp

- **Đổi namespace giữa chừng dự án**: mọi triple cũ dùng namespace cũ sẽ "trôi dạt", không còn liên kết với ontology mới — tương đương đổi tên database production mà không migrate dữ liệu. Chốt namespace sớm, đổi thì phải có kế hoạch migrate.
- **IRI chứa khoảng trắng hoặc ký tự đặc biệt chưa encode** — gây lỗi parser khó debug. Luôn dùng chữ không dấu, không khoảng trắng cho local name (`:AoSoMiDaiTay`, không phải `:Áo sơ mi`).
- **Nhầm prefix rỗng giữa 2 file khác nhau** — nếu file A và B đều dùng `:` nhưng trỏ 2 namespace khác nhau, ghép 2 file dễ gây hiểu nhầm nghiêm trọng. Luôn ghi rõ `@prefix` đầu mỗi file, không giả định ngầm.

---
*Tiếp theo: [03 — Class & Class Hierarchy](03-class-hierarchy.md)*
