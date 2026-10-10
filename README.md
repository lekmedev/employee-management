# Employee Management — Web quản lý nhân viên

Thay thế file Excel, DB là source of truth. Tự động gán Opera Cloud Roles theo chức vụ, mở rộng được loại tài khoản khác.

## Stack
- Backend: NestJS + Prisma 8 RC + PostgreSQL (`apps/api`)
- Frontend: React + TS + Vite + shadcn/ui + Tailwind (`apps/web`)
- Hạ tầng: Docker Compose

## Chạy dev
```bash
docker compose up -d        # postgres
npm run dev --workspace api # API :3000
npm run dev --workspace web # Web :5173
```

## Tài liệu
- `CLAUDE.md` — quy ước bắt buộc cho AI agent
- `docs/schema.md`, `docs/roles-flow.md`, `docs/decisions.md`
