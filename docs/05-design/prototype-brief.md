# Output #10 - Prototype Brief

**Prototype goal:** kiểm chứng 4 flow có rủi ro cao trong quy trình xử lý ticket.

**Persona & Phân quyền:**
* **Lan (Employee):** Người gửi yêu cầu, theo dõi và bấm xác nhận đóng vé (Verify & Close).
* **Nam (IT Agent):** Người tiếp nhận xử lý, dùng AI Gợi ý (Generate AI Response) để trả lời.
* **Đức (IT Manager):** Người giám sát Dashboard, phân công vé (Assign) và duyệt các vé AI phân loại không chắc chắn (Need Triage).

**High-Risk Flows (Điều chỉnh đa góc nhìn):**
* **FLOW A - Tạo ticket + AI phân loại (Employee):**
  Employee gửi "Máy in tầng 3 không in được" → AI gợi ý nhãn → Employee gửi vé.
* **FLOW B - Phân loại không chắc chắn / Low Confidence (Employee & IT Manager):**
  Employee gõ mô tả ngắn (< 4 từ) → Ticket rơi vào trạng thái low-confidence → Chuyển về tab "Need Triage" trên Dashboard của IT Manager để duyệt tay.
* **FLOW C - Agent/Manager xử lý và trả lời khách (Agent & IT Manager):**
  Bấm "AI Gợi ý" → Nút gửi Disabled → Chỉnh sửa văn bản nháp → Gửi → Cập nhật In Progress.
* **FLOW D - Xác nhận đóng vé (Employee):**
  Ticket Resolved → Employee xem kết quả → Bấm "Verify & Close" (Done) hoặc Reopen.
