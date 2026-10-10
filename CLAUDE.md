# CLAUDE.md

## Dự án
Web quản lý nhân viên và tự động gán Opera Cloud Roles theo chức vụ,
thay thế file Excel hiện tại. Database là nguồn dữ liệu chính (source of truth).
Thiết kế phải mở rộng được cho loại tài khoản khác ngoài Opera Cloud.

Module: employees, positions, roles, Excel export, dashboard.
Đăng nhập, phân quyền: Admin (toàn quyền) / Viewer (chỉ xem).

## Stack
- Backend: NestJS + Prisma 8 RC + PostgreSQL (`@prisma/orm-postgres`)
- Frontend: React + TypeScript + Vite + shadcn/ui + Tailwind
  (React Router, TanStack Query, React Hook Form + Zod)
- Hạ tầng: Docker Compose

## Cấu trúc thư mục
- `apps/api/src/modules/<module>/`   controller, service, dto, test của từng module
- `apps/api/prisma/`                 contract.prisma, prisma.config.ts, migrations, seed
- `apps/api/src/prisma/`            db.ts (Prisma 8 client: `db.orm.public.<Model>`)
- `apps/web/src/pages/`              mỗi trang một thư mục
- `apps/web/src/components/`         component dùng chung
- `apps/web/src/api/`                hàm gọi API, tách riêng khỏi UI
- `docs/`                            tài liệu chi tiết (đọc khi cần, không đọc hết)

## Lệnh thường dùng
- Chạy dev: `docker compose up -d` rồi `npm run dev`
- Migration (Prisma 8, RC theo registry mới):
  `npx prisma contract emit` (sau mỗi lần sửa contract) →
  `npx prisma migration plan --name <ten>` →
  `npx prisma db migrate --advance-ref db` (dev) →
  `npx prisma migration status` (verify)
- DB mới trống: `npx prisma db init` (tạo bảng + sign lần đầu)
- Test API: `npm run test --workspace api`
- Lint/type check: `npm run lint && npm run typecheck`

## Quy ước (bắt buộc)
- TypeScript chặt, không dùng `any`.
- Mọi thay đổi DB đi qua Prisma migration, không sửa DB bằng tay.
- Backend: controller mỏng, logic nằm ở service; validate đầu vào bằng DTO.
- Frontend: không gọi fetch trực tiếp trong component, dùng hook TanStack Query + `src/api/`.
- Mỗi file dưới ~300 dòng; vượt thì tách.
- Giao diện tiếng Việt; tên biến, hàm, commit bằng tiếng Anh.
- Quyền Admin/Viewer kiểm tra ở backend (guard), UI chỉ ẩn/hiện thêm.
- Không thêm thư viện mới khi chưa hỏi.

## Cách làm việc
- Làm một module, một bước mỗi session: schema -> API -> UI.
- Task lớn: lập kế hoạch ngắn và chờ duyệt trước khi code.
- Chỉ đọc file liên quan task, không quét toàn bộ repo.
- Xong task: chạy test/typecheck, commit, cập nhật mục Tiến độ bên dưới.

## Tài liệu chi tiết
- `docs/schema.md`      thiết kế DB và quan hệ bảng
- `docs/roles-flow.md`  luồng gán Opera Cloud Roles theo chức vụ
- `docs/decisions.md`   các quyết định kiến trúc đã chốt và lý do

## Tiến độ
Cập nhật mục này cuối mỗi session.

- [x] Step 0: repo setup, CLAUDE.md, Prisma 8 RC contract (skeleton monorepo, 2026-10-10)
- Đang làm: Step 0 verify — `contract emit` + `db init` + commit + push `lekmedev/employee-management` (public). Prisma 8 RC theo registry mới (`prisma@8.0.0-rc.22`, `@prisma/orm-postgres@8.0.0-rc.17`), quy trình migrate viết lại trong CLAUDE.md + docs/schema.md.
- [ ] Auth + phân quyền Admin/Viewer
- [ ] Module positions
- [ ] Module employees
- [ ] Module roles (gán role theo chức vụ)
- [ ] Excel export
- [ ] Dashboard

Đang làm: Step 0 — skeleton monorepo vừa dựng (NestJS skeleton + Prisma schema + Vite web + Docker Compose postgres), chưa có module nào
Việc kế tiếp: 1) `docker compose up -d` + `npx prisma migrate dev` để verify schema; 2) Auth + phân quyền Admin/Viewer
Lưu ý/vấn đề tồn đọng: frontend demo cũ (Vite + mock localStorage, Tailwind v4, DataTable 330 dòng) đã bỏ, làm lại đúng stack shadcn/ui khi tới module employees
