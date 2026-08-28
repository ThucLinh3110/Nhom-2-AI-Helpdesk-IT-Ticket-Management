 ## Cách test
 - **API**: dùng Postman, gọi từng endpoint, kiểm tra đúng status code + đúng dữ liệu trả về.
 - **UI**: click tay trên Figma Prototype, đối chiếu với Acceptance Criteria.
 - **AI Output**: kiểm tra JSON trả về đúng cấu trúc; nội dung câu trả lời AI đọc thủ công.

 ## Quy trình
 1. Đọc AC của Story trên Taiga.
 2. Test API bằng Postman theo case đúng + case lỗi.
 3. Test UI bằng Figma Prototype.
 4. Ghi kết quả Pass/Fail vào `traceability.xlsx`.
 5. Fail → báo chủ luồng, sửa, test lại.

 ## Ưu tiên test
 1. Story 1 (Tạo ticket) — nền tảng cho luồng khác.
 2. Story 2 (AI phân loại) — output sai sẽ hỏng Dashboard.
 3. Story 3, Story 4.
 4. Story 5 (Knowledge Base) — độc lập, ít phụ thuộc.

 ## Test Automation (phần bổ sung)
 - Gom API vào 1 Postman Collection, viết Tests script (JS) cho mỗi request để tự động chấm Pass/Fail.
 - Dùng Collection Runner để chạy lại toàn bộ khi có code mới, không cần bấm tay từng cái.
 - Chỉ tự động hóa API — UI vẫn test tay vì mới là Figma prototype, chưa có code thật để automation.

 ```javascript
 // Ví dụ test cho GET /api/knowledge
 pm.test("Status 200", () => pm.response.to.have.status(200));
 pm.test("Có field results", () => {
 	const json = pm.response.json();
     pm.expect(json).to.have.property("results");
 });
 ```

 ## Bảng Layer / Coverage
 | Layer | Coverage cho dự án |
 |---|---|
 | Unit | validator form Tạo ticket; parser JSON output AI phân loại; hàm so sánh SLA (created_at vs current_time); hàm rank kết quả tìm kiếm Knowledge Base |
 | Integration | `POST /api/tickets` + DB; AI service (mock) + gán nhãn vào DB; `GET /api/tickets/sla-status` + DB; `GET /api/knowledge` + DB |
 | E2E | Tạo ticket → AI phân loại → hiện trên Dashboard → SLA cảnh báo → đóng ticket; tìm kiếm Knowledge Base → xem chi tiết → tạo ticket nếu chưa đủ |
 | Non-functional | latency cơ bản API tìm kiếm (<1s); không lộ secret/API key; audit log có mặt cho hành động tạo ticket/AI phân loại/publish bài viết |

 ## 10 Test Case mẫu

 | ID | Case | Trace | Expected | Mode |
 |---|---|---|---|---|
 | TC-01 | Tạo ticket thiếu tiêu đề | REQ-01, Story 1 AC | Báo lỗi, không cho submit | Automated |
 | TC-02 | Tạo ticket đầy đủ thông tin | REQ-01, Story 1 AC | Tạo ticket mới, trạng thái "Mới" | Automated |
 | TC-03 | AI phân loại vé mới | REQ-02, Story 2 AC | Tự gắn đúng tag phân loại | Automated |
 | TC-04 | AI không chắc chắn khi phân loại | REQ-02, Story 2 AC | Gắn cờ "Cần xác nhận thủ công" | Automated |
 | TC-05 | IT Agent bấm Generate AI Response | REQ-03, Story 3 AC | Trả lời gợi ý trong vài giây | Automated |
 | TC-06 | AI không đủ dữ liệu để trả lời | REQ-03, Story 3 AC | Trả lời rõ "Không đủ dữ liệu" | Automated |
 | TC-07 | Vé còn dưới 1 tiếng đến hạn SLA | REQ-04, Story 4 AC | Badge chuyển màu vàng | Automated |
 | TC-08 | Vé quá hạn SLA | REQ-04, Story 4 AC | Badge đỏ + báo quản lý | Automated |
 | TC-09 | Tìm kiếm Knowledge Base khớp bài viết | REQ-05, Story 5 AC | Trả về bài viết liên quan | Automated |
 | TC-10 | Tìm kiếm Knowledge Base không khớp | REQ-05, Story 5 AC | Hiện "Không tìm thấy" + gợi ý tạo ticket | Manual/E2E |
