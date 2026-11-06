<!-- คัดลอกหัวข้อนี้ไปวางใน CLAUDE.md ของโปรเจกต์ แล้วกรอกให้ครบ — /card /decide /audit อ่านค่าจากที่นี่ -->

## Card workflow

| ค่า | ของโปรเจกต์นี้ |
|---|---|
| ระบบการ์ด | เช่น Jira `mycompany.atlassian.net` project `PROJ` (ผ่าน Atlassian MCP) / GitHub Issues `org/repo` (ผ่าน `gh`) |
| branch หลัก | `main` |
| build | `npm run build` |
| test (unit) | `npm test` |
| test (integration/e2e) | `npm run test:integration` — ใช้ DB สำหรับเทสต์เท่านั้น |
| typecheck / lint | `npm run typecheck` |
| โฟลเดอร์รายงานส่งมอบ | `docs/handover/` (แม่แบบ `docs/handover/TEMPLATE.md`) |

### 🔴 กฎที่ห้ามละเมิด
<!-- ข้อที่ผิดแล้ว "งานเสีย" ไม่ใช่แค่ต้องแก้ · แต่ละข้อบอกด้วยว่าตรวจยังไง -->
1. <กฎ> — ตรวจด้วย `<คำสั่ง grep/test>`
2. …

### path ที่เสี่ยงสูง (audit จะเสนอตรวจแบบ dual)
- `src/auth/` · `src/payments/` · `migrations/` · …

### เวอร์ชันที่ปักหมุด — ห้ามอัปเอง
| ของ | เวอร์ชัน | อัปแล้วพังยังไง |
|---|---|---|

### เอกสารไหนเป็นความจริงเรื่องอะไร
| เรื่อง | ไฟล์ |
|---|---|

### เรื่องที่ยังไม่ตัดสิน — ห้ามเดา
- <เรื่อง> — รอใคร
