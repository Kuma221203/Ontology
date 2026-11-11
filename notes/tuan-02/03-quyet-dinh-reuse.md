# Quyết Định Reuse — Dựa Trên Research Đã Có

Xem đầy đủ, có trích dẫn nguồn, tại `research/ontology-manufacturing-standards.md` (đã làm ở giai đoạn research sớm trong project). File này tóm tắt lại thành quyết định hành động cho Tuần 2.

## Bảng quyết định

| Thành phần | Quyết định | Vì sao |
|---|---|---|
| **GS1 EPCIS + CBV** | **Tái dùng trực tiếp** — import file `epcis-ontology.ttl`, `cbv-ontology.ttl` | File OWL/RDF chính thức, hiện hành (gs1.org), khớp đúng traceability event (cắt→may→KCS→đóng gói→xuất) |
| **ISA-95** | Tham khảo **khung khái niệm** (Personnel/Equipment/Material/Process Segment) khi thiết kế, không import | Không có bản OWL chính thức nào tồn tại để import — chỉ có chuẩn text/XSD |
| **GS1 Web Vocabulary** | Tái dùng hẹp, chỉ phần hướng khách hàng (GTIN, size/color variant) | Scope catalog/e-commerce, không phải sản xuất nội bộ — chỉ dùng nếu ontology "lộ ra" phía khách hàng |
| **MASON, OntoCAPE** | Không dùng, chỉ tham khảo ý tưởng chia Entity/Operation/Resource | MASON dừng bảo trì từ 2006; OntoCAPE sai domain (hóa chất liên tục, không phải may mặc rời rạc) |
| **Apparel/textile ontology khác** (CPI, VetiVoc, SAREF4WEAR) | **Không dùng** | Scope retail-catalog hoặc wearable-IoT, không khớp sản xuất nhà máy |
| **Core: Order/Style/SKU/BOM/WorkOrder/QC** | **Tự thiết kế từ đầu** | Greenfield thật sự — không tìm thấy ontology sản xuất may mặc nào sẵn có trong research |

## Hệ quả thiết kế cụ thể

- Namespace nội bộ (`nhamay:...`, đã chọn ở Tuần 1) chỉ chứa phần **core tự thiết kế**.
- Phần **traceability event** (cắt, may, KCS, đóng gói, xuất hàng) sẽ dùng trực tiếp class/property của `epcis:` (ví dụ `epcis:TransformationEvent`, `epcis:bizStep`) — không tự đặt tên riêng cho khái niệm này, tránh trùng lặp vô ích với chuẩn đã có.
- Khi cần, dùng `rdfs:subClassOf` hoặc để riêng 2 namespace cùng tồn tại trong 1 ontology — không bắt buộc phải "hợp nhất" ISA-95 vì nó không có artifact để hợp nhất, chỉ dùng tên gọi/khái niệm của nó làm cảm hứng đặt tên property (`hasEquipment`, `hasPersonnel` nếu cần sau này).

## Vì sao quyết định này quan trọng trước khi code (Tuần 3)

Nếu không chốt bảng này trước, rất dễ **tự định nghĩa lại** một khái niệm mà GS1 đã có sẵn (ví dụ tự tạo `nhamay:ShippingEvent` thay vì dùng `epcis:ObjectEvent` có sẵn) — gây lãng phí công sức và khó tích hợp với đối tác/khách hàng dùng chuẩn GS1 sau này (nhiều brand/khách hàng lớn trong ngành may mặc đã yêu cầu nhà cung cấp tuân thủ EPCIS cho truy vết).

---
*Tiếp theo: [04 — Import ontology ngoài](04-import-ontology-ngoai.md)*
