# Test Strategy (Chiến lược kiểm thử)
### Người phụ trách: Ngọc (QA/Release)

## 1. Phạm vi kiểm thử
Dự án: Hệ thống Quản lý lỗi IT có AI hỗ trợ. Kiểm thử bao phủ 5 luồng chính:
Tạo & Theo dõi Ticket, AI Phân loại & Phân công, Xử lý & Phản hồi AI, Cảnh báo & SLA, Knowledge Base.

## 2. Cách test theo từng lớp

| Lớp | Công cụ | Cách làm |
|---|---|---|
| **API (Backend)** | Postman | Gọi trực tiếp từng endpoint trong file API.md, kiểm tra: đúng status code, đúng cấu trúc JSON trả về, xử lý đúng khi input sai/thiếu |
| **UI (Figma Prototype)** | Click tay trên Figma Prototype (đã nối bởi Ngân) | Đi qua từng luồng như người dùng thật, đối chiếu với Acceptance Criteria (Given-When-Then) |
| **AI Output (Story 2, 3)** | Postman + đọc thủ công | Gửi nhiều loại nội dung vé khác nhau, kiểm tra AI trả JSON đúng cấu trúc, không bịa thông tin ngoài Vault |
| **Usability (trải nghiệm)** | usability-test.md của Ngân | Tổng hợp lỗi 3 người dùng ngoài nhóm phát hiện được |

## 3. Quy trình test 1 Story (áp dụng cho cả 5 luồng)

1. Đọc Acceptance Criteria (Given-When-Then) của Story trên Taiga.
2. Nếu AC chưa rõ ràng/không test được → gửi lại chủ luồng yêu cầu viết lại (xem sheet "Definition of Ready" trong traceability.xlsx).
3. Test API bằng Postman theo từng case trong AC (case đúng + case lỗi/biên).
4. Test UI bằng cách click qua Figma Prototype, đối chiếu đúng luồng mô tả.
5. Ghi kết quả Pass/Fail vào file `traceability.xlsx` (cột "Pass/Fail").
6. Nếu Fail → note lý do, báo lại chủ luồng để sửa, test lại (regression) trước deadline.

## 4. Ưu tiên test (do thời gian gấp tới 29/08)

1. **Ưu tiên cao:** Story 1 (Tạo ticket) — vì mọi luồng khác phụ thuộc vào việc có ticket trước.
2. **Ưu tiên cao:** Story 2 (AI phân loại) — vì output JSON sai sẽ làm hỏng luồng Dashboard.
3. **Ưu tiên trung bình:** Story 3 (AI phản hồi), Story 4 (SLA).
4. **Ưu tiên trung bình:** Story 5 (Knowledge Base) — luồng độc lập, ít phụ thuộc luồng khác.

## 5. Tiêu chí để 1 Story được coi là "Test xong" (Exit Criteria)

- [ ] Toàn bộ case trong Acceptance Criteria đã test và đánh dấu Pass/Fail.
- [ ] Không còn lỗi mức nghiêm trọng (Critical/Blocker) chưa xử lý.
- [ ] Đã cập nhật kết quả vào `traceability.xlsx`.
- [ ] Story trên Taiga đã chuyển sang cột "Done".

## 6. Công cụ sử dụng

| Công cụ | Mục đích |
|---|---|
| Postman | Test API endpoints (thủ công + chạy Collection tự động) |
| Figma (chế độ Present/Prototype) | Test luồng UI |
| Taiga | Theo dõi trạng thái Story, gắn kết quả test |
| traceability.xlsx | Ghi nhận kết quả Pass/Fail, tổng hợp toàn dự án |

---

## 7. Chiến lược Test Automation (Kiểm thử tự động)

> Mục tiêu: không phải build hệ thống automation phức tạp (không đủ thời gian trong 1 tuần), mà là **tự động hóa phần lặp lại nhiều nhất** — test API — để mỗi khi Hoa/Linh/Luy/Ngân sửa code, Ngọc chạy lại 1 lệnh là biết ngay cái gì vỡ, thay vì phải test tay lại từ đầu.

### 7.1. Phạm vi tự động hóa

| Loại test | Có tự động hóa không? | Lý do |
|---|---|---|
| API (backend) | **Có** | Ổn định, dễ viết assertion, chạy lại nhanh, ROI cao nhất trong thời gian ngắn |
| UI trên Figma Prototype | **Không** | Figma là bản thiết kế, chưa phải app thật nên không automation được — vẫn test tay (click qua Prototype) |
| AI Output (Story 2, 3) | **Bán tự động** | Tự động gọi API và kiểm tra *cấu trúc* JSON trả về (đúng field, đúng kiểu dữ liệu); nội dung AI sinh ra (câu trả lời) vẫn cần người đọc lại vì không đoán trước chính xác 100% |

### 7.2. Công cụ & cách làm

- **Postman Collection + Collection Runner**: Ngọc gom toàn bộ API trong `API.md` của Hoa thành 1 Postman Collection. Mỗi request có sẵn **Tests script** (tab "Tests" trong Postman, viết bằng JS) để tự check:
  ```javascript
  // Ví dụ test cho POST /api/tickets
  pm.test("Status code là 201", () => pm.response.to.have.status(201));
  pm.test("Trả về có ticket id", () => {
      const json = pm.response.json();
      pm.expect(json).to.have.property("id");
      pm.expect(json.status).to.eql("Mới");
  });
  ```
  ```javascript
  // Ví dụ test cho GET /api/knowledge?keyword=...
  pm.test("Status 200 và có field results", () => {
      pm.response.to.have.status(200);
      const json = pm.response.json();
      pm.expect(json).to.have.property("results");
      pm.expect(json.results).to.be.an("array");
  });
  ```
- **Collection Runner**: khi cần test lại toàn bộ (sau khi ai đó sửa code), Ngọc bấm "Run Collection" → Postman tự chạy hết các request theo thứ tự, tự chấm Pass/Fail, xuất báo cáo — không phải bấm tay từng cái.
- **Biến môi trường (Environment)**: tạo 1 Postman Environment chứa `base_url`, `token` để đổi giữa local/deploy mà không sửa từng request.

### 7.3. Quy trình chạy Automation

1. Ngọc gom API từ `API.md` (Hoa) → tạo Postman Collection tương ứng (1 folder / 1 Story).
2. Viết Tests script cho từng request, bám theo Acceptance Criteria (Given-When-Then) của Story đó.
3. Trước deadline / trước mỗi lần demo: chạy Collection Runner 1 lần → kết quả Pass/Fail tự động.
4. Copy kết quả (số request Pass/Fail) dán vào cột "Pass/Fail" trong `traceability.xlsx`.
5. Fail → báo ngay chủ luồng, không đợi đến lúc thuyết trình mới phát hiện.

### 7.4. Giới hạn (nói rõ khi báo cáo, tránh bị hỏi vặn)

- Automation ở đây dừng ở mức **API-level testing bằng Postman**, không phải automation UI end-to-end (VD: Selenium/Cypress) — vì UI mới là Figma Prototype, chưa phải code thật nên không có DOM để automation UI.
- Không tự động hóa việc đánh giá "câu trả lời AI có hay/đúng không" — phần này vẫn cần người đọc, vì nội dung AI sinh ra không cố định.
