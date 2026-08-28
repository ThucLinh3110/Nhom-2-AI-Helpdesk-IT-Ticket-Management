# Sơ đồ Kiến trúc Tổng thể

```mermaid
graph TD
    %% Khối Client
    subgraph Client ["Client"]
        Input["Speech Input / TTS + text fallback"]
        WebApp["Web App React/Next.js"]
        Input --> WebApp
    end

    %% Khối Application (Backend)
    subgraph Application ["Application"]
        APIAuth["API + Auth"]
        Orchestrator["Assistant Orchestrator (structured tool calls)"]
        DomainServices["Ticket / Knowledge Base / SLA Domain Services"]

        WebApp -->|Requests| APIAuth
        APIAuth --> Orchestrator
        APIAuth --> DomainServices
        
        Orchestrator -->|Validated Calls| DomainServices
    end

    %% Khối Data & External
    subgraph DataExternal ["Data & External"]
        DB[("PostgreSQL")]
        LLM["LLM Provider"]
        AuditLog[("Logs / Audit")]
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

## ADR – Quyết định kiến trúc

### ADR-001: Không cho LLM truy cập Database trực tiếp

**Context:**
Hệ thống sử dụng LLM để hỗ trợ phân loại Ticket, đề xuất phản hồi và thực hiện một số tác vụ thông qua structured tool call.

**Decision:**
Không cho LLM truy cập Database trực tiếp. LLM chỉ tạo structured tool call, sau đó Application Layer kiểm tra tool, quyền người dùng, tham số và business rules trước khi gọi Domain Service.

**Rationale:**
Cách tiếp cận này giúp kiểm soát quyền truy cập, đảm bảo các quy tắc nghiệp vụ được thực thi tại Domain Service và duy trì Ticket, Knowledge Base, SLA là nguồn dữ liệu nghiệp vụ chính.

**Trade-off:**
Thiết kế này giúp hệ thống an toàn, dễ kiểm soát và tránh để LLM tự ý thay đổi dữ liệu, nhưng làm tăng thêm một bước xử lý thông qua Orchestrator và Validation Layer. Đây là đánh đổi phù hợp với phạm vi hệ thống vì ưu tiên tính kiểm soát và nhất quán của dữ liệu nghiệp vụ.
