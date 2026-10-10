# Thiết kế DB và quan hệ bảng (Prisma 8 contract: `apps/api/prisma/contract.prisma`)

Quy trình migrate mới: `contract emit` → `migration plan --name <ten>` → `db migrate --advance-ref db` → `migration status`. DB mới trống dùng `db init`.
Client code dùng `db.orm.public.<Model>` (file `apps/api/src/prisma/db.ts`, viết sau khi `contract emit` xong).

## Bảng chính
- `users` — tài khoản đăng nhập (email unique, role ADMIN/VIEWER).
- `departments` — phòng ban (name unique).
- `positions` — chức vụ (title unique).
- `employees` — nhân viên, FK tới departments + positions.

## Gán role mở rộng nhiều loại tài khoản
- `account_types` — loại tài khoản (`code` unique, vd `OPERA_CLOUD`).
- `account_roles` — role của từng loại (`@@unique([accountTypeId, code])`).
- `position_account_roles` — chức vụ -> role mặc định (`@@unique([positionId, accountRoleId])`).
- `employee_accounts` — role thực tế của nhân viên (`@@unique([employeeId, accountRoleId])`).

## Luồng thay đổi chức vụ
Đổi `employees.positionId` -> gợi ý lại role từ `position_account_roles`, Admin duyệt rồi ghi `employee_accounts`.
Chi tiết xem `docs/roles-flow.md`.
