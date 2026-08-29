**Ma trận truy vết (Traceability Matrix)**

| ID Yêu cầu (Bài 1) | User Story (Bài 2 - Taiga) | Giao diện (Figma / UI) | API / Kỹ thuật (Backend) | Bảng Dữ liệu (Database) |
| :--- | :--- | :--- | :--- | :--- |
| **REQ-HD-01**, **REQ-HD-02** | **US-HD-01.1:** Khách hàng tạo vé báo lỗi đính kèm ảnh | Màn hình `Tạo vé`, `Nhấn gửi` | `POST /api/tickets`<br>`POST /api/tickets/:id/attachments` | Bảng `Ticket`, `Attachment` |
| **REQ-HD-08** | **US-HD-01.2:** Khách hàng và IT trao đổi qua khung chat | Màn hình `Chi tiết vé` | `POST /api/tickets/:id/comments` | Bảng `TicketComment` |
| **REQ-HD-03**, **REQ-HD-04** | **US-HD-02.1:** AI tự động gán nhãn Category và Priority cho vé mới | Hệ thống chạy ngầm / Dashboard | `POST /api/ai/classify` | Bảng `Ticket`, `AITriageLog` |
| **REQ-HD-05**, **NFR-HD-05** | **US-HD-02.2:** Xử lý fallback khi AI có độ tin cậy thấp | Tab "Cần duyệt tay (Need Triage)" trên `Dashboard` | `POST /api/tickets/:id/assign` | Bảng `Ticket`, `AuditEvent` |
| **REQ-HD-09** | **US-HD-03.1:** Hệ thống dùng RAG lấy bài viết KB để AI sinh Draft | Màn hình `Sử dụng AI` | `POST /api/ai/suggest-reply` | Bảng `AIReplySuggestionLog`, `KnowledgeArticle` |
| **REQ-HD-10** | **US-HD-03.2:** IT Agent duyệt, chỉnh sửa và gửi phản hồi AI | Trạng thái Disabled của nút "Gửi" ở `Chi tiết vé` | `POST /api/tickets/:id/comments` | Bảng `TicketComment` |
| **REQ-HD-11** | **US-HD-04.1:** Hệ thống đếm ngược và gắn badge cảnh báo SLA | Màn hình `Dsach vé` (Icon đồng hồ cát nhấp nháy, badge đổi màu) | `GET /api/tickets/sla` | Bảng `TicketSLA`, `SLAPolicy` |
| **REQ-HD-05** | **US-HD-04.2:** Dashboard thống kê vé trễ hạn cho IT Manager | Màn hình `Dashboard` | `GET /api/tickets/sla-breached` | Bảng `TicketSLA`, `Notification` |
| **REQ-HD-09** | **US-HD-05.1:** Agent tìm kiếm bài viết hướng dẫn | Màn hình `Thư viện IT` | `GET /api/articles` | Bảng `KnowledgeArticle` |
| **REQ-HD-12** | **US-HD-05.2:** Admin Thêm/Sửa/Xóa bài viết Knowledge Base | Màn hình Quản trị KB | `POST /api/articles` | Bảng `KnowledgeArticle` |
