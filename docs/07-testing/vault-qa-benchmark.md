## Bảng 20 câu hỏi Benchmark kiểm thử

| # | Phân loại | Prompt | Expected answer | Nguồn / Logic | KQ |
|:---|:---|:---|:---|:---|:---:|
| Q1 | Rule | Thời gian đếm ngược SLA bắt đầu được tính từ thời điểm nào? | Ngay khi vé được lưu thành công vào Database. | BR-HD-01 | Pass |
| Q2 | Rule | Thời gian SLA có phụ thuộc vào thời điểm AI phân tích xong vé hay không? | Không. SLA bắt đầu tính ngay khi vé được lưu vào Database, không phụ thuộc vào lúc AI phân tích xong. | BR-HD-01 | Pass |
| Q3 | Rule | Nếu AI chưa phân tích xong Ticket thì đồng hồ SLA có bắt đầu đếm không? | Có. Đồng hồ SLA bắt đầu đếm ngay sau khi Ticket được lưu thành công vào Database. | BR-HD-01 | Pass |
| Q4 | Rule | Ai là người có quyền xác nhận để chuyển Ticket sang trạng thái "Done"? | IT Agent là người bấm xác nhận để chuyển Ticket sang trạng thái "Done". | BR-HD-02 | Pass |
| Q5 | Rule | AI LLM có được tự ý chuyển Ticket sang trạng thái "Done" không? | Không. AI LLM tuyệt đối không được tự ý đóng Ticket. | BR-HD-02 | Pass |
| Q6 | Rule | Ticket chỉ được chuyển sang trạng thái "Done" khi nào? | Khi IT Agent bấm xác nhận hoàn thành Ticket. | BR-HD-02 | Pass |
| Q7 | Rule | Nếu AI xác định vấn đề của Ticket đã được giải quyết, AI có thể tự đóng Ticket không? | Không. AI không có quyền tự ý đóng Ticket; IT Agent phải bấm xác nhận. | BR-HD-02 | Pass |
| Q8 | Rule | Khách hàng có được tự thay đổi Priority sau khi vé đã được AI phân loại không? | Không. Khách hàng không có quyền tự thay đổi Priority sau khi vé đã được AI phân loại. | BR-HD-03 | Pass |
| Q9 | Rule | Sau khi IT Manager đã chốt Priority, khách hàng có được tự nâng mức Priority không? | Không. Khách hàng không có quyền tự thay đổi Priority sau khi IT Manager chốt. | BR-HD-03 | Pass |
| Q10 | Rule | Khách hàng có thể tự đổi Priority của Ticket từ Medium lên Critical sau khi AI đã phân loại không? | Không. Khách hàng không có quyền tự thay đổi Priority sau khi AI đã phân loại. | NFR-VC-02 | Pass |
| Q11 | Assumption | Giai đoạn MVP sử dụng bao nhiêu bài viết Knowledge Base giả lập để kiểm thử? | 20 bài viết cơ bản. | ASM-HD-01 | Pass |
| Q12 | Assumption | Dữ liệu Knowledge Base được sử dụng trong MVP là dữ liệu thật hay dữ liệu giả lập? | Dữ liệu giả lập (mock data). | ASM-HD-01 | Pass |
| Q13 | Assumption | 20 bài viết Knowledge Base giả lập được sử dụng để kiểm thử tính năng nào? | Tính năng "AI gợi ý phản hồi". | ASM-HD-01 | Pass |
| Q14 | Assumption | Trong giai đoạn đồ án, hệ thống có triển khai xác thực tài khoản qua tin nhắn SMS thật không? | Không. Giai đoạn đồ án tạm thời bỏ qua xác thực tài khoản qua SMS thật. | ASM-HD-02 | Pass |
| Q15 | Assumption | Giai đoạn MVP của hệ thống được triển khai trên môi trường nào? | Môi trường Web App. | ASM-HD-02 | Pass |
| Q16 | Out-of-Scope | Tính năng xác thực tài khoản qua tin nhắn SMS thật có nằm trong phạm vi của đồ án không? | Không. Tính năng xác thực qua SMS thật được tạm thời bỏ qua trong giai đoạn đồ án. | ASM-HD-02 | Pass |
| Q17 | Fallback | Nếu API của AI bị sập hoặc phản hồi chậm quá 5 giây thì hệ thống có phương án xử lý thay thế không? | Có. Hệ thống fallback để Manager tự phân công vé bằng tay. | Q-HD-01 | Pass |
| Q18 | Fallback | Khi API AI bị sập hoặc chậm quá 5 giây, ai sẽ thực hiện phân công Ticket thủ công? | Manager sẽ tự phân công Ticket bằng tay. | Q-HD-01 | Pass |
| Q19 | Fallback | Ngưỡng thời gian nào được sử dụng để kích hoạt cơ chế fallback của API AI trong MVP? | Khi API AI bị sập hoặc phản hồi chậm quá 5 giây. | Q-HD-01 | Pass |
| Q20 | Fallback | Nếu Ticket vừa được lưu vào Database nhưng API AI bị sập, SLA có bắt đầu tính không và ai sẽ xử lý phân công Ticket? | Có. SLA vẫn bắt đầu tính ngay khi Ticket được lưu vào Database; Manager sẽ tự phân công Ticket bằng tay. | BR-HD-01 + Q-HD-01 | Pass |
