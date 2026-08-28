**Mục tiêu:** Kiểm chứng 4 luồng rủi ro cao với 3 người dùng (đóng vai Khách hàng và IT Agent).

### 1. Kịch bản Kiểm thử (Test Script)

| Task (Nhiệm vụ) | Success criterion (Tiêu chí thành công) |
| :--- | :--- |
| **T1 (Employee): Báo lỗi & Xem phân loại** | Người dùng hoàn thành form, nhận ra vé đã được tạo và thấy được nhãn AI (Hardware). |
| **T2 (IT Manager): Duyệt vé Low-confidence** | Truy cập tab "Need Triage" trên Dashboard, chọn danh mục và phân công Agent thủ công. |
| **T3 (IT Agent): Trả lời AI & Theo dõi SLA** | Nhận diện được vé sắp vi phạm SLA (nhờ màu cảnh báo/icon đồng hồ), bấm "AI Gợi ý", biết chỉnh sửa nội dung nháp trước khi bấm gửi. |
| **T4 (Employee): Đóng hoặc mở lại vé** | Mở lại vé đã xử lý xong, kiểm tra câu trả lời của Agent và bấm nút "Xác nhận đóng vé (Done)" hoặc "Mở lại (Reopen)". |

### 2. Kết quả & Quyết định (Findings & Decisions)

| Finding (Phát hiện vấn đề) | Evidence (Bằng chứng) | Decision (Quyết định sửa đổi UI/Logic) |
| :--- | :--- | :--- |
| **Gửi nhầm AI Draft** | 2/3 IT Agent bấm nút "Gửi" luôn ngay khi AI vừa tạo xong Draft mà không đọc lại. | Đổi trạng thái nút "Gửi" thành Disabled. Bắt buộc Agent phải click vào ô text Draft và gõ/xóa ít nhất 1 ký tự thì mới cho gửi. |
| **Bỏ qua cờ cảnh báo SLA** | 1/3 IT Agent không nhận ra vé sắp hết hạn vì màu vàng của Badge Warning quá nhạt. | Đổi mã màu color.warning đậm hơn, thêm icon đồng hồ cát nhấp nháy bên cạnh vé còn < 1 tiếng. |
| **Cờ Low-confidence bị trôi** | Manager mất thời gian tìm vé bị AI phân loại sai (Category: Unknown) trong danh sách chung. | Tạo riêng một tab "Cần duyệt tay (Need Triage)" trên Dashboard để gom toàn bộ vé Low-confidence vào một chỗ. |
| **Khó tìm nút đóng vé** | 1/3 Employee mất thời gian tìm nút xác nhận sau khi Agent báo hoàn thành. | Đưa nút "Verify & Close" lên đầu khung ChatThread với màu color.ok nổi bật. |
