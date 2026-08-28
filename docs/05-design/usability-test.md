**Mục tiêu:** Kiểm chứng 3 luồng rủi ro cao với 3 người dùng (đóng vai Khách hàng và IT Agent).

---

### 1. Kịch bản Kiểm thử (Test Script)

| Task (Nhiệm vụ) | Success Criterion (Tiêu chí thành công) |
| :--- | :--- |
| **T1 (Khách hàng):** Báo lỗi "Không in được tài liệu". | Người dùng hoàn thành form, nhận ra vé đã được tạo và thấy được nhãn AI (Hardware). |
| **T2 (IT Agent):** Dùng AI gợi ý trả lời. | Biết cách bấm nút "AI Gợi ý" và hiểu cần sửa nội dung nháp trước khi gửi. |
| **T3 (IT Agent):** Xác định vé sắp lố giờ. | Nhận diện được ngay vé nào sắp vi phạm SLA trên giao diện danh sách. |

---

### 2. Kết quả & Quyết định (Findings & Decisions)

| Finding (Phát hiện vấn đề) | Evidence (Bằng chứng) | Decision (Quyết định sửa đổi UI/Logic) |
| :--- | :--- | :--- |
| **Gửi nhầm AI Draft** | 2/3 IT Agent bấm nút "Gửi" luôn ngay khi AI vừa tạo xong Draft mà không đọc lại. | Đổi trạng thái nút "Gửi" thành **Disabled**. Bắt buộc Agent phải click vào ô text Draft và gõ/xóa ít nhất 1 ký tự thì mới cho gửi. |
| **Bỏ qua cờ cảnh báo SLA** | 1/3 IT Agent không nhận ra vé sắp hết hạn vì màu vàng của Badge Warning quá nhạt. | Đổi mã màu `color.warning` đậm hơn, thêm icon đồng hồ cát nhấp nháy bên cạnh vé còn < 1 tiếng. |
| **Cờ Low-confidence bị trôi** | Manager mất thời gian tìm vé bị AI phân loại sai (Category: Unknown) trong danh sách chung. | Tạo riêng một tab **"Cần duyệt tay (Need Triage)"** trên Dashboard để gom toàn bộ vé Low-confidence vào một chỗ. |
