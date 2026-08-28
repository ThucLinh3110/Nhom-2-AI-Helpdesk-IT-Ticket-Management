# Đặc tả API (API Contracts)

Tài liệu này định nghĩa các endpoint giao tiếp giữa Frontend và Backend, thiết kế theo chuẩn RESTful.

## Bảng API Endpoints

| Method | Path | Input | Output | Auth |
| :--- | :--- | :--- | :--- | :--- |
| **POST** | `/api/tickets` | `{title, description, attachments}` | `Ticket` | USER |
| **GET** | `/api/tickets` | `?status,category,priority` | `Ticket[]` | USER / AGENT / MANAGER |
| **GET** | `/api/tickets/:id` | `-` | `Ticket + Messages` | OWNER / AGENT / MANAGER |
| **PATCH** | `/api/tickets/:id/status` | `{status}` | `Ticket` | AGENT / MANAGER |
| **PATCH** | `/api/tickets/:id/assign` | `{agentId}` | `Ticket` | MANAGER |
| **POST** | `/api/tickets/:id/messages` | `{content, isInternal}` | `Message` | OWNER / AGENT |
| **POST** | `/api/assistant/analyze` | `{ticketId, description}` | `{category, priority}` | SYSTEM / AGENT |
| **POST** | `/api/assistant/draft` | `{ticketId, kbQuery}` | `DraftResponse` | AGENT |
| **GET** | `/api/dashboard/stats` | `?timeRange` | `DashboardStats` | MANAGER |
| **GET** | `/api/kb` | `?q,category` | `KBArticle[]` | PUBLIC / AUTH |
| **POST** | `/api/kb` | `{title, content, category}` | `KBArticle` | MANAGER / ADMIN |
| **PATCH**| `/api/kb/:id` | `{title, content}` | `KBArticle` | MANAGER / ADMIN |

## Ghi chú:
- **Auth Roles**:
  - `USER`: Khách hàng / Nhân viên báo lỗi.
  - `AGENT`: Nhân viên IT xử lý kỹ thuật.
  - `MANAGER`: Quản lý IT, xem thống kê và phân công.
  - `OWNER`: Người tạo ra vé (User cụ thể).
  - `SYSTEM`: Hệ thống tự gọi (ví dụ lúc tạo vé tự động gọi AI).
- Các API liên quan đến `assistant` sẽ ngầm gọi đến LLM Provider (ví dụ: Gemini) và được Backend kiểm soát chặt chẽ.
