# Luồng gán Opera Cloud Roles theo chức vụ

1. Admin tạo/sửa `positions`, gán role mặc định qua `position_account_roles`.
2. Khi thêm nhân viên hoặc đổi chức vụ: hệ thống gợi ý role từ mapping của position.
3. Admin duyệt danh sách gợi ý -> ghi vào `employee_accounts` (kèm `grantedBy`, `grantedAt`).
4. Loại tài khoản mới (ngoài Opera Cloud): thêm 1 dòng `account_types`, thêm roles, map lại position — không sửa code.

## Quy tắc
- Viewer chỉ xem, không duyệt/gán (guard ở backend).
- Mọi thay đổi role đều để lại bản ghi `employee_accounts`, không xóa ngầm.
