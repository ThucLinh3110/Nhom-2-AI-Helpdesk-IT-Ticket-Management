# AI Usage Log

## Hoàng Luy

| ID | Task | Input/context | AI output | Human verification/decision |
|:---|:---|:---|:---|:---|
| **A-01** | Soạn thảo Glossary hệ thống | Đề bài dự án + Yêu cầu từ BA (`requirements.md`) | Đưa ra danh sách từ vựng kỹ thuật chung. | Nhóm lọc bỏ bớt các từ quá quen thuộc, yêu cầu viết lại và chuẩn hóa các khái niệm đặc thù của dự án. |
| **A-02** | Xây dựng quy tắc Source Priority | Yêu cầu AI đề xuất cách phân xử khi tài liệu trong Vault có thông tin chênh lệch. | AI gợi ý: "Hệ thống sẽ tự động đối chiếu và chọn tài liệu có mốc thời gian cập nhật mới nhất để trả lời". | **Bác bỏ giải pháp:** Quy định này rủi ro vì AI không thể tự quyết định logic hệ thống. Điều chỉnh lại trật tự: **Business Rules > Requirement Inventory > PRD > Glossary**; trường hợp không có dữ liệu thì bắt buộc trả lời **KHÔNG ĐỦ DỮ LIỆU**. |
| **A-03** | Tạo bộ câu hỏi Vault Benchmark | Nạp `rules.md` và `requirements.md`, yêu cầu sinh 20 câu hỏi kiểm thử bao gồm ca bình thường và ca ngoại lệ. | AI sinh đủ 20 câu, nhưng với các câu hỏi nằm ngoài phạm vi kỹ thuật (hỏi giờ cơm trưa, tiền thưởng lễ), AI lại tự tra cứu kiến thức bên ngoài để trả lời. | **Chỉnh sửa ràng buộc:** Bổ sung quy định Hàng rào an toàn (Guardrails). Ràng buộc AI phải nhận biết câu hỏi Out-of-Scope, từ chối trả lời lịch sự và hướng dẫn liên hệ phòng Nhân sự/Hành chính (HR/Admin). |
| **A-04** | Kiểm thử câu hỏi Benchmark (Q20) | Đưa câu hỏi: *"Khách hàng có được tự ý đổi mức độ ưu tiên sau khi vé đã chốt không?"* kèm tài liệu `rules.md`. | Ban đầu AI trả lời: "Được, khách hàng có thể nâng mức độ ưu tiên lên Critical nếu cảm thấy sự cố của mình ngày càng khẩn cấp hơn". | **Bắt lỗi logic & Ảo giác (Hallucination):** Phản hồi này vi phạm quy tắc nghiệp vụ `BR-HD-03`. Nhóm bác bỏ kết quả, ép AI đọc lại điều khoản `BR-HD-03` trong `rules.md`. Sau khi tinh chỉnh context, AI đã trả lời chính xác (*Pass*): "Không, khách hàng không có quyền tự thay đổi Priority sau khi vé đã được AI phân loại hoặc IT Manager chốt". |

## Thục Linh

| ID | Task | Input/context | AI output | Human verification/decision |
|:---|:---|:---|:---|:---|
| **A-05** | Phân tích Yêu cầu (Requirement Reviewer) | Nạp ghi chú phỏng vấn (User Research) và yêu cầu rà soát khoảng trống tính năng. | AI đề xuất thêm tính năng "Tự động xóa các vé rác nếu mô tả quá ngắn". | **Bác bỏ:** Việc tự xóa vé là vi phạm quyền của người dùng. Quyết định đưa tính năng tự động xóa vào danh sách Out-of-Scope. |
| **A-06** | Chia User Story (Story Breaker) | Nạp file PRD và yêu cầu lên khung User Story cho hệ thống. | AI đề xuất gom chung luồng "Gán nhãn AI" và "Dashboard Quản lý" thành 1 Story rất lớn. | **Sửa đổi:** Ép AI tách luồng "AI gán nhãn tự động" thành một Story cá nhân độc lập (US-HD-02) để đảm bảo khối lượng công việc nhỏ gọn (≤ 3 points). |
| **A-07** | Viết Tiêu chí nghiệm thu (Acceptance Criteria) | Nạp thẻ US-HD-02, yêu cầu AI viết Acceptance Criteria theo chuẩn Given/When/Then. | AI sinh ra 2 AC nhưng chỉ bao phủ trường hợp thành công (Happy Path). | **Bổ sung:** Yêu cầu AI làm thêm các lỗi ngoại lệ (Error paths). Bổ sung AC2: Nếu mô tả vé quá mơ hồ, AI bắt buộc trả về nhãn "Unknown" thay vì đoán bừa. |

##Hoa

| ID | Task | Input/Context | AI Output | Human Verification/Decision |
| :--- | :--- | :--- | :--- | :--- |
| **A-09** | Vẽ sơ đồ kiến trúc hệ thống (`architecture.md`) | Yêu cầu dự án + các chức năng AI, Ticket, SLA, Knowledge Base và Notification | AI đề xuất kiến trúc gồm LLM, AI Orchestrator, các Service, API + Auth, PostgreSQL, File Storage và Audit/AI Logs. | **Kiểm tra và điều chỉnh:** Xác nhận luồng LLM → AI Orchestrator → Service và nguyên tắc Human-in-the-loop. |
| **A-10** | Vẽ sơ đồ DB / Data Model (`data-model.md`) | Yêu cầu nghiệp vụ + kiến trúc hệ thống + các chức năng chính | AI đề xuất các entity, fields, relationships và business rules cho hệ thống. | **Review và chuẩn hóa:** Kiểm tra entity, field, relationship và các business rules theo yêu cầu dự án. |
| **A-11** | Liệt kê API (`API.md` – Swagger/Postman) | Data Model + Architecture + yêu cầu authentication/authorization | AI đề xuất các REST API kèm Method, Path, Input, Output và Auth. | **Kiểm tra và hoàn thiện:** Đối chiếu API với Data Model, business flow và quyền của từng role. |

---

##  Ngọc

| ID | Task | Input/Context | AI Output | Human Verification/Decision |
| :--- | :--- | :--- | :--- | :--- |
| **A-12** | Lập bảng Traceability Matrix (Traceability) | Bảng phân công + hướng dẫn thực thi của nhóm trưởng (cột: Mã REQ, Link Taiga, Link Figma, API, Pass/Fail); 5 Story của nhóm. | Đề xuất bảng 8 cột cho 5 Story, tự điền sẵn dòng Story 5 (do Ngọc phụ trách), các dòng Story 1-4 để trống dạng placeholder. | Chấp nhận cấu trúc cột; kiểm tra placeholder được đánh dấu rõ (màu vàng) để không nhầm là dữ liệu thật. Yêu cầu bổ sung sheet Definition of Ready đi kèm. |
| **A-13** | Duyệt Acceptance Criteria (Definition of Ready) | 5 thẻ Story dự kiến của nhóm (Tạo ticket, AI phân loại, AI phản hồi, SLA, Knowledge Base). | AI đề xuất bộ AC mẫu theo Given-When-Then cho từng Story, dùng để đối chiếu khi duyệt thẻ thật trên Taiga. | **Bổ sung:** Chỉ dùng làm bản đối chiếu tạm thời; AC thật phải do chủ luồng viết và Ngọc duyệt lại trên Taiga, không copy thẳng bản AI làm vào backlog chính thức. |
| **A-14** | Viết chiến lược Test Automation (Test Strategy) | Yêu cầu nhóm trưởng: 'Viết chiến lược Test Automation'; 5 API endpoint dự kiến của các Story. | AI đề xuất chỉ automation phần API (Postman Collection + Tests script + Collection Runner), giữ UI test tay vì mới ở giai đoạn Figma prototype. | Đồng ý giới hạn phạm vi automation; tự thêm ghi chú giới hạn (không automation UI, không tự động chấm chất lượng câu trả lời AI) để không bị hỏi vặn khi báo cáo. |
| **A-15** | Lập 10 Test Case cho Story 5 và toàn dự án | AC của 5 Story; mẫu bảng Test Case tham khảo từ giáo trình (ID \| Case \| Trace \| Expected \| Mode). | AI sinh 10 test case TC-01..TC-10 phủ đủ 5 Story, có cột Trace nối về đúng REQ/AC tương ứng. | Đối chiếu từng test case có Trace hợp lệ, không có case 'mồ côi' không rõ nguồn. Giữ TC-10 (Knowledge Base không khớp) ở chế độ Manual/E2E vì cần quan sát trực quan UI. |

---

##  Ngân

| ID | Task | Input/Context | AI Output | Human Verification/Decision |
| :--- | :--- | :--- | :--- | :--- |
| **A-16** | Đưa ra 4 flow rủi ro cao | Dựa vào user story | Xây dựng 4 flow: <br>- A: Tạo ticket + AI phân loại<br>- B: Low-confidence<br>- C: Agent xử lý và AI Reply<br>- D: Verify & Close | Kiểm tra lại flow và yêu cầu giữ đúng các state bắt buộc: `loading`, `empty`, `AI-classifying`, `low-confidence`, `no-agent-available`, `network-error`, `draft-before-submit`, `success`. |
| **A-17** | Xây dựng Prototype Brief | Tạo một prototype web responsive cho Voice Commerce.<br>- Dùng requirement và business rules.<br>- Tập trung 4 flows trong Prototype Brief.<br>- Hiển thị rõ transcript, assistant message, product cards, cart summary và confirmation. | Prototype đã hiển thị có thể đổi role ("Xem như") để duyệt qua cả 4 flow: Employee, Agent, IT Manager. | Giữ lại các luồng cơ bản, bổ sung thêm các luồng và chỉnh sửa bố cục hợp lý. |
| **A-18** | Xây dựng Design System | Prototype Brief | AI đưa ra bảng quy định chuẩn: Design Tokens, Component Inventory, UX Copy dựa vào prototype. | Chấp nhận quy định màu, kiểm tra lại Figma thực tế, đặc biệt Button size và ảnh component, trước khi merge. AI không tự quyết định giá trị Figma chưa được xác nhận. |

