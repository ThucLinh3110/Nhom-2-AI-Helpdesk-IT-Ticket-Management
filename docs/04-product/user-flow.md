# User Flow — Dự án AI Helpdesk & IT Ticket
### File: `docs/03-product/user-flow.mmd` (nội dung Mermaid bên dưới)
### Phụ trách chính thức: Linh (Product/BA) — bản này Ngọc soạn giúp dựa trên 5 Story đã có

Luồng đi: **Login → Tạo vé → AI phân loại → AI phản hồi/SLA → Đóng vé**, có nhánh lỗi/rẽ nhánh và nhánh Knowledge Base (tự phục vụ trước khi tạo vé).

```mermaid
flowchart TD
    A[Nhân viên đăng nhập] --> B{Đã biết cách xử lý chưa?}

    B -- Chưa biết / muốn tự tra cứu --> KB1[Vào Thư viện IT<br/>Story 5]
    KB1 --> KB2[Gõ từ khóa tìm kiếm]
    KB2 --> KB3{Có bài viết phù hợp?}
    KB3 -- Có --> KB4[Xem chi tiết bài viết]
    KB4 --> KB5{Đã giải quyết được?}
    KB5 -- Rồi --> END1[Kết thúc]
    KB5 -- Chưa --> C
    KB3 -- Không --> C[Tạo vé báo lỗi<br/>Story 1]

    B -- Biết vấn đề, muốn báo lỗi luôn --> C

    C --> C1{Điền đủ Tiêu đề + Mô tả?}
    C1 -- Thiếu thông tin --> C2[Báo lỗi, không cho gửi]
    C2 --> C
    C1 -- Đủ thông tin --> C3[Vé được tạo<br/>Trạng thái: Mới]

    C3 --> D[AI đọc nội dung vé<br/>Story 2]
    D --> D1{AI đủ tin cậy để phân loại?}
    D1 -- Có --> D2[Gắn tag tự động<br/>Lỗi Mạng/Phần cứng/Phần mềm]
    D1 -- Không --> D3[Gắn cờ: Cần xác nhận thủ công]
    D3 --> D4[Quản lý IT xác nhận tay]
    D4 --> D2

    D2 --> E[Hiện trên Dashboard<br/>đã chia cho IT Agent]

    E --> F[IT Agent mở vé]
    F --> F1[Bấm Generate AI Response<br/>Story 3]
    F1 --> F2{AI có đủ dữ liệu trả lời?}
    F2 -- Có --> F3[AI sinh câu trả lời nháp]
    F2 -- Không --> F4["AI trả lời: Không đủ dữ liệu"]
    F4 --> F5[IT Agent tự soạn câu trả lời]
    F3 --> F6[IT Agent đọc, sửa nếu cần, gửi]
    F5 --> F6

    E --> G[Hệ thống theo dõi SLA<br/>Story 4]
    G --> G1{Còn bao lâu đến hạn?}
    G1 -- Dưới 1 tiếng --> G2[Badge màu vàng - Warning]
    G1 -- Quá hạn --> G3[Badge màu đỏ - Overdue<br/>Báo quản lý]
    G1 -- Còn nhiều thời gian --> G4[Badge bình thường]

    F6 --> H[Nhân viên nhận phản hồi]
    H --> H1{Vấn đề đã được giải quyết?}
    H1 -- Chưa --> F
    H1 -- Rồi --> I[Đóng vé<br/>Trạng thái: Done]
    I --> END2[Kết thúc]
```

---

## Giải thích các nhánh chính (để trình bày)

### Happy path (đường chính)
Đăng nhập → Tạo vé → AI tự phân loại → Hiện trên Dashboard IT → IT Agent dùng AI gợi ý trả lời → Nhân viên nhận phản hồi → Đóng vé.

### Nhánh rẽ 1: Tự phục vụ trước (Knowledge Base — Story 5)
Nhân viên có thể tra cứu Thư viện IT trước khi tạo vé. Nếu tìm thấy bài viết và tự giải quyết được → kết thúc sớm, không cần tạo vé, giảm tải cho IT.

### Nhánh lỗi 1: Thiếu thông tin khi tạo vé (Story 1)
Nếu nhân viên bỏ trống Tiêu đề/Mô tả → hệ thống chặn lại, yêu cầu điền lại.

### Nhánh lỗi 2: AI không chắc chắn khi phân loại (Story 2)
Nếu AI thiếu tin cậy → không tự động gán, chuyển cho Quản lý IT xác nhận thủ công, tránh phân loại sai.

### Nhánh lỗi 3: AI không đủ dữ liệu để trả lời (Story 3)
AI không bịa câu trả lời — báo rõ "Không đủ dữ liệu", IT Agent tự soạn.

### Nhánh song song: Giám sát SLA (Story 4)
Chạy song song với toàn bộ luồng xử lý vé, liên tục kiểm tra thời gian còn lại để cảnh báo đúng lúc.

### Vòng lặp: Chưa giải quyết xong
Nếu nhân viên nhận phản hồi nhưng vấn đề chưa hết → quay lại cho IT Agent xử lý tiếp, chưa đóng vé.

---

## Cách dùng file này

- Copy phần code trong khối \`\`\`mermaid ở trên, dán vào file `user-flow.mmd` trong repo, hoặc dán trực tiếp vào Obsidian/GitHub (đều tự render thành sơ đồ).
- Đây là **bản nháp Ngọc soạn dựa trên 5 Story đã thống nhất** — Linh (người phụ trách chính thức PRD + user-flow) nên rà lại và điều chỉnh nếu có thay đổi nghiệp vụ.
