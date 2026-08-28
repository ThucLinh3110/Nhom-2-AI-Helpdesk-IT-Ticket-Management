# Hướng dẫn Môi trường Phát triển (Development Environment Specification)
**Dự án:** Hệ thống Quản lý Lỗi IT tích hợp AI (AI-Assisted IT Helpdesk - Nhóm 2)  
**Tài liệu tham chiếu:** Hướng dẫn cài đặt các môi trường (MIS3032 - 2026), Giáo trình Thực hành Lập trình Ứng dụng Doanh nghiệp.

---

## 1. Môi trường Chuẩn Bắt buộc (Standard Stack)

Mọi thành viên trong nhóm và người đánh giá bắt buộc tuân thủ đúng các phiên bản công cụ sau để đảm bảo tính đồng nhất trên toàn hệ thống:

| Thành phần | Phiên bản / Tiêu chuẩn bắt buộc | Mục đích sử dụng |
| :--- | :--- | :--- |
| **Hệ điều hành** | Windows 11 64-bit (WSL 2) / macOS 14+ / Ubuntu 24.04 | Môi trường hệ thống |
| **Source Control** | Git + GitHub (kết nối SSH, branch mặc định `main`) | Quản lý mã nguồn, PR, CI/CD |
| **Code Editor** | Visual Studio Code (bản User Setup) | Môi trường soạn thảo chính |
| **Frontend Framework** | Node.js 24 LTS, React, TypeScript, Vite, Tailwind CSS | Giao diện Web App cho 3 vai trò |
| **Backend Framework** | Python 3.13.x, FastAPI | Xử lý API, Orchestrator, Auth |
| **Python Package Manager**| `uv` (Astral) | Quản lý Python version, virtualenv, dependencies |
| **Database** | PostgreSQL 18 qua Docker Container | Lưu trữ dữ liệu nghiệp vụ, SLA, logs |
| **Containerization** | Docker Desktop + Docker Compose (v2) | Chạy database và các dịch vụ bổ trợ |
| **Testing** | `pytest` (Backend Unit/API) & `Playwright` (Frontend E2E) | Tự động hóa kiểm thử |
| **Agile & Docs** | Taiga (Scrum board) & Obsidian / Markdown Vault | Quản lý Backlog và Project Vault |

*Lưu ý: Không cài PostgreSQL trực tiếp vào máy host (Windows/macOS) để tránh xung đột cổng và khác biệt phiên bản giữa các máy.*

---

## 2. Quy ước Thư mục & Cấu trúc Repository

Thư mục làm việc trên máy cá nhân không được chứa dấu tiếng Việt hoặc khoảng trắng (Ví dụ chuẩn: `D:\MIS3032\Nhom-2-AI-Helpdesk-IT-Ticket-Management` hoặc `~/MIS3032/Nhom-2-AI-Helpdesk-IT-Ticket-Management`).

Cấu trúc cây thư mục chuẩn của dự án:

```text
Nhom-2-AI-Helpdesk-IT-Ticket-Management/
├── README.md                   # Giới thiệu dự án, hướng dẫn chạy nhanh
├── .gitignore                  # Bỏ qua .env, node_modules, .venv, dist...
├── .env.example                # Cấu hình biến môi trường mẫu (không có secret)
├── compose.yaml                # Cấu hình Docker Compose cho PostgreSQL
│
├── frontend/                   # Ứng dụng Web Client
│   ├── src/                    # Mã nguồn React + TypeScript
│   ├── e2e/                    # Kịch bản kiểm thử Playwright
│   ├── package.json
│   └── package-lock.json
│
├── backend/                    # Máy chủ API & AI Orchestrator
│   ├── app/                    # Mã nguồn FastAPI
│   ├── tests/                  # Bộ kiểm thử pytest
│   ├── pyproject.toml          # Quản lý thư viện backend bằng uv
│   ├── uv.lock
│   └── .python-version         # Khóa phiên bản Python 3.13
│
├── vault/                      # Nguồn tri thức và tài liệu phân tích
│   ├── 01-Requirements/
│   ├── 02-Research/
│   ├── 03-Product/
│   ├── 04-User-Stories/
│   ├── 05-Design/
│   ├── 06-Engineering/
│   ├── 07-QA/
│   └── 08-Decisions/
│
├── docs/                       # Tài liệu kỹ thuật bàn giao
│   ├── PRD.md
│   ├── ARCHITECTURE.md
│   ├── API.md
│   ├── DATA_MODEL.md
│   ├── TRACEABILITY.md
│   ├── RUNBOOK.md
│   └── AI_USAGE_LOG.md
│
└── .github/
    └── workflows/              # GitHub Actions CI/CD
```

## 3. Hướng dẫn Thiết lập Dự án từng bước (Setup Runbook)

### Bước 1: Khởi động Database với Docker Compose
Đảm bảo Docker Desktop đang chạy (trạng thái Engine running). Tại thư mục gốc của dự án, chạy:
```bash
# Khởi động PostgreSQL container chạy ngầm
docker compose up -d

# Kiểm tra container đã hoạt động bình thường
docker compose ps

# (Tùy chọn) Kiểm tra kết nối trực tiếp vào database
docker compose exec db psql -U app -d app
# Gõ \q để thoát khỏi giao diện psql
```

File `compose.yaml` chuẩn của hệ thống:
```yaml
services:
  db:
    image: postgres:18-alpine
    environment:
      POSTGRES_DB: app
      POSTGRES_USER: app
      POSTGRES_PASSWORD: app-local-password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U app -d app"]
      interval: 5s
      timeout: 5s
      retries: 10
volumes:
  postgres_data:
```

### Bước 2: Thiết lập Backend (FastAPI + Python 3.13)
Sử dụng công cụ `uv` để tự động hóa môi trường ảo và cài đặt thư viện:
```bash
# Di chuyển vào thư mục backend
cd backend

# Đảm bảo Python 3.13 đã được pin
uv python pin 3.13

# Đồng bộ / cài đặt các gói phụ thuộc (FastAPI, pytest, SQLModel/SQLAlchemy...)
uv sync

# Khởi chạy server Backend ở chế độ phát triển (Hot-reload)
uv run fastapi dev app/main.py
```
Kiểm tra Backend:
- Địa chỉ API: http://127.0.0.1:8000/ (Phản hồi: `{"message": "..."}`)
- Tài liệu tương tác Swagger UI: http://127.0.0.1:8000/docs

### Bước 3: Thiết lập Frontend (React + TypeScript + Vite)
Sử dụng Node.js 24 LTS và npm:
```bash
# Di chuyển vào thư mục frontend
cd frontend

# Cài đặt toàn bộ dependencies
npm install

# Khởi chạy máy chủ giao diện
npm run dev
```
Kiểm tra Frontend: Truy cập trình duyệt tại http://localhost:5173.

### Bước 4: Cấu hình Biến môi trường (.env)
Sao chép file `.env.example` thành `.env` tại thư mục tương ứng:
```bash
cp .env.example .env
```
Cập nhật các giá trị biến môi trường phát triển cục bộ:
```env
# Database connection
DATABASE_URL=postgresql://app:app-local-password@localhost:5432/app

# JWT Security
JWT_SECRET=local-development-secret-key-change-in-production

# External AI API Configuration (Gemini / OpenAI)
AI_API_KEY=your_actual_api_key_here
AI_MODEL=gemini-1.5-flash
AI_TIMEOUT_SECONDS=3.0
```
Quy tắc an toàn: Tuyệt đối không commit file `.env` chứa API Key thật lên GitHub.

## 4. Danh sách Extension VS Code khuyến nghị
Nhấn tổ hợp phím `Ctrl + Shift + X` (hoặc `Cmd + Shift + X` trên macOS) để cài đặt:
- **Python & Pylance**: Hỗ trợ intellisense, type check cho Python và FastAPI.
- **ESLint & Prettier**: Kiểm tra cú pháp và tự động định dạng mã nguồn TypeScript/React.
- **Docker**: Quản lý container, volumes và database logs trực tiếp trên VS Code.
- **Playwright Test**: Chạy và debug các bài kiểm thử giao diện E2E.
- **GitLens**: Theo dõi lịch sử commit, tác giả và nhánh theo từng dòng mã nguồn.
- **Markdown All in One**: Soạn thảo tài liệu và Project Vault.

## 5. Kịch bản Kiểm tra Môi trường (Environment Check Verification)
Trước khi bắt đầu thực hiện code hoặc lên báo cáo, sinh viên chạy kiểm tra bộ 5 bài kiểm thử baseline sau:
```bash
# 1. Kiểm tra build Frontend
cd frontend && npm run build

# 2. Chạy bộ kiểm thử tự động Backend
cd ../backend && uv run pytest -q

# 3. Kiểm tra trạng thái cơ sở dữ liệu
docker compose ps

# 4. Chạy kiểm thử tự động Playwright (Frontend)
cd ../frontend && npx playwright test

# 5. Kiểm tra trạng thái Git
git status
```

Bảng Tiêu chí Đạt chuẩn (PASS Checklist):
```text
ENVIRONMENT CHECK
[PASS] Git & GitHub SSH Connection
[PASS] Node 24 LTS & npm
[PASS] uv & Python 3.13
[PASS] Docker & Docker Compose Engine
[PASS] PostgreSQL 18 Container Running
[PASS] React / Vite Production Build
[PASS] FastAPI Swagger Documentation (/docs)
[PASS] Pytest Suite (All Tests Passed)
[PASS] Playwright E2E Baseline
```
