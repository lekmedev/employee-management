# Các quyết định kiến trúc đã chốt và lý do

- Monorepo npm workspaces (`apps/api`, `apps/web`): 1 repo, 1 CLAUDE.md, Claude làm 1 module/session không lạc.
- PostgreSQL là source of truth, thay Excel: Prisma migration only, không sửa DB tay.
- Role mapping tách 3 tầng (account_types -> account_roles -> position_account_roles): thêm loại tài khoản mới không sửa schema.
- Controller mỏng, logic ở service + DTO validate: guard Admin/Viewer tập trung, test service không cần HTTP.
- Frontend gọi API qua `apps/web/src/api/` + TanStack Query: đổi backend không sửa component.
- Step 0 chỉ skeleton (health + schema + compose): verify `docker compose up -d` + migrate trước khi làm Auth.
