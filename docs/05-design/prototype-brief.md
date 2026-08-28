* **Prototype goal:** Kiểm chứng 4 flow có rủi ro cao.
* **Persona:** Lan - IT Agent bận rộn.

### Core Flows

#### FLOW A - Tạo ticket + AI phân loại
> *"Máy in tầng 3 không in được"*
* **Flow:** `draft-before-submit` $\rightarrow$ `AI-classifying` $\rightarrow$ Hiển thị category + priority đề xuất $\rightarrow$ `success`.

#### FLOW B - Phân loại không chắc chắn
> *"Máy tính bị lỗi"*
* **Flow:** `low-confidence` $\rightarrow$ Hệ thống yêu cầu phân loại thủ công.

#### FLOW C - Agent xử lý và trả lời khách
> *"AI Gợi ý"*
* **Flow:** AI trả về draft response $\rightarrow$ Agent chỉnh sửa $\rightarrow$ Gửi $\rightarrow$ Status cập nhật `In Progress`.

#### FLOW D - Employee xác nhận đóng ticket
> *"Verify & Close"*
* **Flow:** Ticket `Resolved` $\rightarrow$ Employee xem lại kết quả $\rightarrow$ Confirm $\rightarrow$ `success` (Done) hoặc `Reopen`.

---

### Required States

* `loading`
* `empty`
* `AI-classifying`
* `low-confidence`
* `no-agent-available`
* `network-error`
* `draft-before-submit`
* `success`
