# Competency Questions — Công Cụ Scope Quan Trọng Nhất

## Định nghĩa

Một **Competency Question (CQ)** là một câu hỏi bằng ngôn ngữ tự nhiên mà ontology **phải trả lời được** sau khi hoàn thành. Danh sách CQ chính là "bản đặc tả yêu cầu" của ontology — giống user story trong phát triển phần mềm, hoặc acceptance criteria trong kiểm thử.

## Vì sao CQ quan trọng hơn "liệt kê khái niệm"

Nếu bắt đầu bằng cách hỏi "ontology này cần những class gì?", bạn dễ liệt kê tràn lan theo trực giác. Nếu bắt đầu bằng "người dùng thật sẽ hỏi gì?", mọi class/property sinh ra đều **có lý do tồn tại rõ ràng** — truy ngược được về đúng 1 câu hỏi nghiệp vụ cụ thể.

## Quy trình viết CQ

1. Hỏi người dùng nghiệp vụ thật (quản lý sản xuất, QC, kế hoạch...) — hoặc tự đặt mình vào vai đó nếu chưa phỏng vấn được ai
2. Viết câu hỏi bằng tiếng Việt tự nhiên **trước**, chưa cần nghĩ tới class/property OWL
3. Từ mỗi câu hỏi, suy ngược ra **class và property cần có** để trả lời được nó
4. Nếu một class/property không phục vụ bất kỳ CQ nào → cân nhắc bỏ (nguyên tắc chống "class explosion" đã học Tuần 1)

## Ví dụ CQ cho domain nhà máy may

| # | Competency Question | Class/Property cần có |
|---|---|---|
| 1 | SKU nào đang trễ tiến độ ở công đoạn may? | `WorkOrder`, `hasStage`, `dueDate`, `currentStage` |
| 2 | Lot vải nào đã dùng cho đơn hàng X? | `Fabric`, `hasLotNumber`, quan hệ suy từ BOM → SKU → Order |
| 3 | Style nào chưa đủ điều kiện sản xuất (thiếu BOM)? | `CompleteSKU` (Tuần 1), `hasBOM` |
| 4 | Công đoạn nào gây nhiều lỗi KCS nhất trong tháng? | `QCEvent`, `hasDefectCode`, `occursAtStage`, `eventDate` |
| 5 | Đơn hàng nào sắp tới hạn giao mà còn SKU chưa hoàn thành cắt? | `Order`, `dueDate`, `CuttingEvent` |

## Mức độ chi tiết của CQ — tránh 2 sai lầm phổ biến

| Sai lầm | Ví dụ | Vấn đề |
|---|---|---|
| **Quá mơ hồ** | "Cho tôi biết thông tin về SKU" | Không rõ cần property gì — không guide được thiết kế |
| **Quá cụ thể (gắn chặt vào UI/report)** | "Hiển thị bảng Excel có 5 cột: mã SKU, size, màu, ngày cắt, trạng thái" | Nhầm CQ với thiết kế report — ontology không cần biết về "cột Excel", chỉ cần biết dữ liệu có liên kết logic đúng |

CQ tốt nằm ở giữa: đủ cụ thể để suy ra class/property, nhưng không gắn chết vào một hình thức hiển thị cụ thể.

## Thực hành viết tiếp

Viết tiếp danh sách trên tới khi có **8-10 câu**, ưu tiên câu hỏi mà *người quản lý sản xuất thật sự sẽ hỏi* — không chỉ hỏi bản thân, hỏi người làm sản xuất thực tế nếu có cơ hội. Gợi ý thêm vài hướng chưa có trong bảng trên để tự mở rộng:
- Câu hỏi về **nguyên liệu tồn kho** (Fabric/Trim còn đủ cho đơn hàng sắp tới không?)
- Câu hỏi về **năng suất** (chuyền nào hoàn thành work order nhanh nhất tuần này?)
- Câu hỏi về **truy vết** (SKU bị khách khiếu nại dùng lô vải nào, ai may, công đoạn nào?)

---
*Tiếp theo: [03 — Quyết định reuse](03-quyet-dinh-reuse.md)*
