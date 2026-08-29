**Requirement Inventory**

| ID | Loại | Yêu cầu | Nguồn | Priority |
| :--- | :--- | :--- | :--- | :--- |
| **REQ-HD-01** | FR | Khách hàng có thể tạo vé báo cáo sự cố bằng văn bản và đính kèm hình ảnh. | User Research | Must |
| **REQ-HD-02** | FR | Hệ thống hiển thị lịch sử các vé đã tạo và trạng thái (New, In Progress, Done) cho người tạo. | User Research | Must |
| **REQ-HD-03** | FR | AI tự động phân tích mô tả của vé mới để gán nhãn danh mục (Network, Hardware, Software). | Persona: Manager | Must |
| **REQ-HD-04** | FR | AI tự động phân tích và đề xuất mức độ ưu tiên (Low, Medium, High, Critical) dựa trên nội dung. | Persona: Manager | Must |
| **REQ-HD-05** | FR | Hệ thống có Dashboard để IT Manager thống kê số lượng vé theo danh mục và trạng thái. | Persona: Manager | Must |
| **REQ-HD-06** | FR | IT Manager có thể phân công vé thủ công cho một IT Agent cụ thể. | Persona: Manager | Must |
| **REQ-HD-07** | FR | IT Agent nhận được danh sách các vé lỗi đang được phân công cho mình. | Persona: Agent | Must |
| **REQ-HD-08** | FR | Có khung chat trong chi tiết vé để IT Agent và khách hàng trao đổi thông tin. | Persona: Agent | Must |
| **REQ-HD-09** | FR | IT Agent có thể sử dụng nút "AI Gợi ý" để lấy câu trả lời nháp từ Knowledge Base. | Persona: Agent | Must |
| **REQ-HD-10** | FR | IT Agent bắt buộc xem và có quyền chỉnh sửa câu trả lời của AI trước khi gửi cho khách hàng. | Constraint | Must |
| **REQ-HD-11** | FR | Hệ thống tự động tính toán và hiển thị đồng hồ đếm ngược SLA cho từng vé. | User Research | Must |
| **REQ-HD-12** | FR | Quản trị viên (Admin) có thể thêm, sửa, xóa các bài viết trong Knowledge Base (Thư viện tài liệu). | Persona: Manager | Should |
| **NFR-HD-01** | NFR | Thời gian AI trả về kết quả gán nhãn và phân loại không vượt quá 3 giây. | Constraint | Must |
| **NFR-HD-02** | NFR | Giao diện phải Responsive, có thể sử dụng tốt trên cả màn hình PC và Mobile. | Constraint | Should |
| **NFR-HD-03** | NFR | Các endpoint API gọi AI phải được phân quyền và bảo mật, không rò rỉ API Key ở frontend. | Constraint | Must |
| **NFR-HD-04** | NFR | Hệ thống phải lưu log mỗi khi trạng thái vé hoặc người phụ trách bị thay đổi. | NFR tiêu chuẩn | Must |
| **NFR-HD-05** | NFR | Nếu API của AI LLM bị lỗi hoặc timeout, hệ thống phải cho phép IT Manager tự phân loại vé bằng tay. | NFR tiêu chuẩn | Must |
