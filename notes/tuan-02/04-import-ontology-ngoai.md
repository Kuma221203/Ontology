# Import Ontology Ngoài Vào Protégé

## Các bước

1. Tải `epcis-ontology.ttl` và `cbv-ontology.ttl` từ [ref.gs1.org/standards/epcis/artefacts](https://ref.gs1.org/standards/epcis/artefacts)
2. Trong Protégé: tab **Active Ontology** → mục "Ontology imports" → **Import an ontology contained in a specific location** → chọn file `.ttl` vừa tải (có thể import từ URL trực tiếp nếu máy có mạng, hoặc từ file local đã tải)
3. Sau khi import, các class/property của EPCIS (ví dụ `epcis:ObjectEvent`, `epcis:TransformationEvent`, `epcis:bizStep`) xuất hiện trong tab Classes/Object Properties của bạn — mang prefix riêng (`epcis:`), **không** trộn lẫn namespace với ontology nội bộ (đúng nguyên tắc IRI/namespace đã học Tuần 1, mục "vì sao tránh collision")
4. Chạy lại reasoner sau khi import — đảm bảo ontology gộp vẫn consistent

## Nối ontology nội bộ với EPCIS

Hai cách phổ biến:

**Cách 1 — Kế thừa (subclass):** nếu khái niệm nội bộ thực sự là một dạng cụ thể của khái niệm EPCIS:
```turtle
nm:CuttingEvent rdfs:subClassOf epcis:TransformationEvent .
```
Nghĩa là mọi `CuttingEvent` cũng tự động là một `TransformationEvent` theo chuẩn GS1 — bất kỳ hệ thống nào hiểu EPCIS cũng đọc hiểu được sự kiện cắt của bạn, dù họ không biết class riêng `nm:CuttingEvent`.

**Cách 2 — Để riêng, không ép quan hệ:** nếu khái niệm nội bộ (ví dụ `nm:WorkOrder`) không khớp trực tiếp với khái niệm nào của EPCIS — cứ để 2 ontology tồn tại song song trong cùng 1 file, chỉ dùng vocabulary của bên nào phù hợp cho đúng phần đó, không gượng ép quan hệ subclass không tự nhiên.

**Khuyến nghị:** chỉ dùng Cách 1 khi quan hệ is-a thực sự đúng về mặt ngữ nghĩa (đã học Tuần 1: SubClassOf là tuyên bố logic thật, không phải gắn nhãn tùy tiện). Với domain này: các **event** sản xuất (cắt, may, KCS, đóng gói, xuất) nên kế thừa `epcis:TransformationEvent`/`epcis:ObjectEvent`; các **entity nghiệp vụ** (Style, SKU, Order, WorkOrder) để riêng ở namespace nội bộ.

## Ví dụ cụ thể: biểu diễn "SKU001 được cắt từ lô vải X"

```turtle
nm:CuttingEvent_20261120_01
    a                epcis:TransformationEvent ;
    epcis:bizStep    cbv:cutting ;
    epcis:inputEPC   nm:FabricLotX ;
    epcis:outputEPC  nm:SKU001 ;
    epcis:eventTime  "2026-11-20T08:30:00+07:00"^^xsd:dateTime .
```

Đọc là: một `TransformationEvent` (đã import từ EPCIS) với bước nghiệp vụ (`bizStep`, lấy từ CBV vocabulary) là "cutting", input là lô vải, output là SKU, xảy ra lúc 8h30 ngày 20/11/2026 — toàn bộ cấu trúc này **không tự đặt ra**, dùng đúng vocabulary chuẩn GS1 đã import.

---
*Tiếp theo: [05 — Bài tập thực hành](05-bai-tap-thuc-hanh.md)*
