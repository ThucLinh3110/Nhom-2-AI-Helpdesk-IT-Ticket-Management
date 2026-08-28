| STT | Thuật ngữ (Term) | Định nghĩa (Definition) |
| :--- | :--- | :--- |
| **1** | **Ticket (Vé báo lỗi)** | Bản ghi kỹ thuật số do nhân viên tạo để yêu cầu bộ phận IT xử lý sự cố. |
| **2** | **End-User (Khách hàng)** | Cán bộ, nhân viên nội bộ gặp sự cố kỹ thuật gửi yêu cầu hỗ trợ. |
| **3** | **IT Agent** | Chuyên viên hỗ trợ kỹ thuật trực tiếp tiếp nhận, xử lý và phản hồi các Ticket. |
| **4** | **IT Manager** | Cấp quản lý giám sát toàn hệ thống, theo dõi SLA, phân công vé thủ công và điều phối sự cố P1. |
| **5** | **SLA (Service Level Agreement)** | Cam kết mức độ dịch vụ. Là thời hạn tối đa quy định cho phản hồi đầu tiên và xử lý dứt điểm. |
| **6** | **Priority (Mức độ ưu tiên)** | Chỉ số phân cấp sự cố từ cao xuống thấp: P1 (Critical), P2 (High), P3 (Medium), P4 (Low). |
| **7** | **Warning / Overdue** | **Warning (Vàng):** Cảnh báo khi thời gian xử lý còn < 20% SLA.<br>**Overdue (Đỏ):** Cảnh báo khẩn khi Ticket đã vượt quá thời hạn SLA. |
| **8** | **Knowledge Base** | Cơ sở tri thức. Thư viện chứa bài viết, hướng dẫn để tra cứu và làm căn cứ cho AI gợi ý phản hồi. |
| **9** | **AI Classification** | Chức năng AI tự động đọc hiểu mô tả của Ticket để gán nhãn danh mục (Network, Hardware, v.v.). |
| **10** | **AI Guardrails** | Hàng rào an toàn AI. Bộ ràng buộc kỹ thuật ngăn AI trả lời ngoài phạm vi IT hoặc tự bịa thông tin. |
| **11** | **Intent (Ý định)** | Mục tiêu có cấu trúc suy ra từ mô tả của user (Ví dụ: `FIX_NETWORK`, `REQUEST_HARDWARE`). |
| **12** | **Clarification (Làm rõ)** | Trạng thái AI yêu cầu user/Agent cung cấp thêm thông tin khi mô tả lỗi quá ngắn (Category: Unknown). |
| **13** | **Grounded Response** | Câu trả lời gợi ý của AI được bám sát hoàn toàn vào dữ liệu thực tế từ Knowledge Base, không "ảo giác". |
| **14** | **Explicit Confirmation** | Hành động xác nhận rõ ràng bằng nút bấm (Ví dụ: IT Agent bấm nút "Đóng vé", AI không được tự đóng). |
| **15** | **Critical Action** | Hành động thay đổi dữ liệu quan trọng của hệ thống (Ví dụ: Xóa vé, thay đổi Priority). |

