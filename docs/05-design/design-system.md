## 1. Design Tokens

### 1.1. Màu nền & Cấu trúc (Background & Layout Colors)

| Token | Value | Use |
| :--- | :--- | :--- |
| `color.page` | `#EEF0F4` | Nền toàn trang (content area) |
| `color.surface` | `#FFFFFF` | Nền card, panel, modal |
| `color.border` | `#E1E4EA` | Viền card, input, bảng |
| `color.border-soft` | `#EDEFF3` | Viền dòng trong bảng/list (nhạt hơn border chính) |
| `color.sidebar` (ink) | `#0E2A4A` | Nền thanh sidebar — xanh navy đậm |
| `color.sidebar-hover` (ink-2) | `#163B63` | Hover trên sidebar |
| `color.on-sidebar` | `#E9EFF7` | Chữ chính trên sidebar |
| `color.on-sidebar-dim` | `#8FA5C0` | Chữ phụ trên sidebar (label, role) |

### 1.2. Màu chữ (Typography Colors)

| Token | Value | Use |
| :--- | :--- | :--- |
| `text.primary` | `#10172A` | Tiêu đề, nội dung chính |
| `text.secondary` | `#5B6478` | Mô tả phụ, label |
| `text.tertiary` | `#97A0B3` | Placeholder, timestamp, mã ticket |

### 1.3. Màu thương hiệu & Bán ngữ nghĩa (Brand & Semantics)

| Token | Value | Use |
| :--- | :--- | :--- |
| `color.brand` | `#3A4CC1` | Nút CTA chính, link, trạng thái active trên sidebar |
| `color.brand-dark` | `#2B3899` | Hover của nút primary |
| `color.brand-tint` | `#EBEDFB` | Nền nhạt cho vùng liên quan brand (box gợi ý AI phân loại) |
| `color.ok` / `ok-tint` | `#147C5C` / `#E3F5EE` | Ticket Done, SLA On track, thông báo thành công |
| `color.warn` / `warn-tint` | `#A85B00` / `#FCEED3` | SLA At risk/Warning, priority Medium, ticket In Progress |
| `color.bad` / `bad-tint` | `#B23A32` / `#FBE7E4` | SLA Overdue, priority Critical, lỗi |
| `color.info` / `info-tint` | `#2E6FA8` / `#E5F0F9` | Ticket status Open |
| `color.ai` / `ai-tint` | `#7B3FE4` / `#F2ECFD` | Mọi thứ liên quan AI: badge đề xuất, nút "Generate AI Response" |

---

## 2. Typography

| Token | Value | Use |
| :--- | :--- | :--- |
| `font.ui` | `Inter (400/500/600/700)` | Toàn bộ giao diện |
| `font.mono` | `JetBrains Mono (400–700)` | Mã ticket (`TCK-101`), đồng hồ đếm ngược SLA — tạo cảm giác dữ liệu chính xác |
| `type.h1` (topbar title) | `16px / 600 / -0.01em` | Tiêu đề trang trên topbar |
| `type.h2` (login hero) | `28px / 700 / -0.02em` | Tiêu đề lớn màn đăng nhập |
| `type.h3` (login form) | `19px / 600` | Tiêu đề form |
| `type.section-title` | `13.5px / 700` | Tiêu đề section trong nội dung (vd "Số vé theo danh mục") |
| `type.body` | `14px / 400` | Nội dung chính |
| `type.label` | `12.5px / 600` | Label field, eyebrow topbar |
| `type.caption` | `11.5px / 400` | Metadata, mã ticket dạng mono |

---

## 3. Bo góc & Bóng đổ (Radius & Shadows)

| Token | Value | Use |
| :--- | :--- | :--- |
| `radius.sm` | `8px` | Button, input, chip nhỏ |
| `radius.md` | `14px` | Card, panel, modal |
| `radius.lg` | `20px` | (khai báo sẵn, chưa dùng — dự phòng cho modal lớn) |
| `shadow.card` | `0 1px 2px rgba(16,23,42,.04), 0 8px 24px -12px rgba(16,23,42,.10)` | Card, dropdown, ô tìm kiếm nổi |

---

## 4. Component Inventory

| Component | Variants/States | Ghi chú |
| :--- | :--- | :--- |
| **Sidebar / SideNav** | `active`/`hover`, ẩn theo role | Menu item hiện/ẩn theo role (Dashboard chỉ IT Manager, Tạo ticket chỉ Employee) |
| **Topbar** | eyebrow + title động theo view | Cập nhật linh hoạt theo trang đang xem |
| **LoginForm** | `default`/`error` | Demo — chưa check email/mật khẩu thật |
| **LoginHeroPanel** | — | Dùng token `--hero` riêng, không đổi theo màu sidebar |
| **MetricCard** | 3 biến thể màu (`info`/`warn`/`bad`) | Cập nhật số liệu real-time mỗi giây không cần reload |
| **BarChart** (số vé theo danh mục) | — | Custom CSS bar, không dùng thư viện chart |
| **AgentSearchDropdown** | `closed`/`open`/`searching`/`selected` | Gõ để lọc tên nhân viên, chọn để lọc bảng ticket theo Agent |
| **TicketTable** | rỗng/có dữ liệu, cột Agent tuỳ ngữ cảnh | Dùng chung cho Danh sách vé và Dashboard |
| **StatusBadge** | `open`/`in-progress`/`done` | Hiển thị trạng thái ticket |
| **PriorityBadge** | `low`/`medium`/`high`/`critical` | Mức độ ưu tiên công việc |
| **SLABadge** (đếm ngược) | `ontrack`/`warning(pulse)`/`overdue(pulse)` | Cập nhật riêng lẻ mỗi giây qua `data-ticket`, không re-render toàn trang |
| **AIBadge** | — | Dùng token `--ai`, tách biệt hoàn toàn với `--brand` |
| **TicketForm** | `draft`/`loading`/`suggested`/`network-error` | Có dropdown loại lỗi + nút đính kèm ảnh |
| **GenerateAIResponseButton** | `idle`/`loading`/`drafted` | Gradient tím `--ai`, không còn bị mất nội dung khi đếm giây (đã fix) |
| **ChatThread** | — | Avatar màu theo người gửi |
| **AssignPanel** | — | Dùng `<select>` gốc trình duyệt, chưa phải component Figma riêng |
| **KnowledgeBaseCard** | — | Grid 2 cột, icon theo danh mục |
| **ConfirmDialog** | — | Đang tạm dùng `alert()`/`confirm()` |
| **Toast** | — | Thông báo lỗi/thành công đang dùng `alert()` hoặc chuyển view |

---

## 5. UX Copy Standards

| Context | Microcopy / Text |
| :--- | :--- |
| **AI đang phân loại** | *AI đang phân tích và phân loại ticket…* |
| **AI đề xuất** | *AI đề xuất — cần xác nhận* |
| **Low confidence** | *AI không đủ tin cậy để phân loại* |
| **Network error** | *Không thể kết nối máy chủ. AI phân loại tạm thời không khả dụng. Bạn có thể tự chọn danh mục thủ công.* |
| **Agent search placeholder** | *Tìm và chọn nhân viên để lọc vé...* |
| **Agent filter active** | *Đang lọc: {Tên nhân viên}* |
| **Empty ticket list** | *Chưa có ticket nào* |
| **Empty KB search** | *Không tìm thấy bài viết phù hợp* |
| **Success** | *Ticket đã được gửi thành công* |
| **Login hint** | *Demo: email/mật khẩu bất kỳ đều đăng nhập được — chọn vai trò để xem đúng giao diện tương ứng. Đây chưa phải xác thực thật.* |
