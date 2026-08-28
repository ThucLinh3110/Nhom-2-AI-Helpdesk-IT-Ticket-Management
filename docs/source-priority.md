# Thứ Tự Ưu Tiên Nguồn Dữ Liệu (Source Priority)

Khi AI xử lý thông tin hoặc phát hiện mâu thuẫn giữa các tài liệu, thứ tự ưu tiên được áp dụng:
1. **Business Rules & Security Constraints** (`rules.md` - BR-HD-01..04, Bảo mật & Guardrails).
2. **Requirement Inventory** (`requirements.md` - REQ-HD-01..12, NFR-HD-01..05).
3. **Product Requirements Document** (`PRD.md` current version).
4. **Project Glossary** (`glossary.md`).
5. **Dữ liệu giả lập / Prototype Assumptions** (Chỉ dùng để minh họa, không tạo rule mới).

*Quy tắc*: Nếu phát hiện mâu thuẫn giữa hai nguồn ngang cấp hoặc không có trong Vault, AI bắt buộc trả lời **"KHÔNG ĐỦ DỮ LIỆU"** và đánh dấu UNKNOWN để con người xác nhận.
