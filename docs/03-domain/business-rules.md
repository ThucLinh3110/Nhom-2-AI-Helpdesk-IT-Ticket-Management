| ID | Rule/Assumption/Question |
| :--- | :--- |
| **BR-HD-01** | Thời gian SLA bắt đầu đếm ngược ngay khi vé được lưu thành công vào Database, không phụ thuộc vào lúc AI phân tích xong. |
| **BR-HD-02** | Vé chỉ được chuyển sang trạng thái "Done" (Hoàn thành) khi IT Agent bấm xác nhận, AI LLM tuyệt đối không được tự ý đóng vé. |
| **BR-HD-03** | Khách hàng không có quyền tự thay đổi mức độ ưu tiên (Priority) sau khi vé đã được AI phân loại hoặc IT Manager chốt. |
| **ASM-HD-01** | MVP chỉ sử dụng dữ liệu Knowledge Base giả lập (mock data) gồm 20 bài viết cơ bản để test tính năng "AI gợi ý phản hồi". |
| **ASM-HD-02** | Giai đoạn đồ án chỉ chạy trên môi trường Web App, tạm thời bỏ qua bước xác thực tài khoản qua tin nhắn SMS thật. |
| **Q-HD-01** | Trong trường hợp API của AI bị sập/chậm quá 5 giây, hệ thống có tự động fallback (chuyển về) cho Manager tự phân công vé bằng tay không? → *Chốt cho MVP: Có.* |
