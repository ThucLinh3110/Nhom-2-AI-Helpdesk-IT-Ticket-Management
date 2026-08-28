# Kiến trúc Hệ thống (Architecture)

**Ý tưởng cốt lõi**: Xây dựng một hệ thống IT Helpdesk tích hợp AI, cho phép người dùng báo lỗi (qua văn bản hoặc giọng nói), theo dõi tiến độ xử lý vé (ticket). Trợ lý thông minh (AI) chỉ đóng vai trò diễn giải yêu cầu, phân loại và gọi các chức năng có kiểm soát (structured tool calls); toàn bộ dữ liệu về trạng thái vé, danh mục lỗi, thời gian SLA và Knowledge Base luôn lấy từ và thuộc thẩm quyền quản lý của backend.

## Sơ đồ Kiến trúc Tổng thể

```mermaid
graph TD
    %% Khối Client
    subgraph Client ["Client"]
        Input[Speech Input / TTS + text fallback]
        WebApp[Web App React/Next.js]
        Input --> WebApp
    end

    %% Khối Application (Backend)
    subgraph Application ["Application"]
        APIAuth[API + Auth]
        Orchestrator[Assistant Orchestrator (structured tool calls)]
        DomainServices[Ticket / Knowledge Base / SLA Domain Services]

        WebApp -->|Requests| APIAuth
        APIAuth --> Orchestrator
        APIAuth --> DomainServices
        
        Orchestrator -->|Validated Calls| DomainServices
    end

    %% Khối Data & External
    subgraph DataExternal ["Data & External"]
        DB[(PostgreSQL)]
        LLM[LLM Provider]
        AuditLog[(Logs / Audit)]
    end
    
    DomainServices --> DB
    Orchestrator <--> LLM
    Orchestrator --> AuditLog
    DomainServices --> AuditLog
```

## Các nguyên tắc thiết kế quan trọng

- **LLM không truy cập DB trực tiếp**: LLM chỉ nhận ngữ cảnh từ người dùng/hệ thống và đề xuất các hành động thông qua `structured tool call` (Ví dụ: `assign_category`, `draft_agent_reply`).
- **Validation chặt chẽ**: Tầng Application sẽ trực tiếp validate (kiểm tra tính hợp lệ) tên tool, quyền người dùng (role), các tham số (arguments) và quy tắc nghiệp vụ (business rules) trước khi thực sự gọi các domain service.
- **Source of Truth**: Ticket/Knowledge Base/SLA Domain service luôn là nguồn sự thật (source-of-truth) duy nhất cho toàn bộ dữ liệu nghiệp vụ. AI chỉ là công cụ hỗ trợ đề xuất.
- **Audit & Logging**: Audit log có nhiệm vụ lưu lại các tool call metadata, kết quả, latency, các sự kiện tạo/thay đổi trạng thái vé (đáp ứng NFR-HD-04); hệ thống tuyệt đối không lưu secret, raw audio hay API keys vào log.
