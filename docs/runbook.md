# Runbook — ใครทำอะไร เรียก skill เมื่อไร

**สถานะ:** แผนจำลองสำหรับทีม 3 คน | วันที่ 25 กันยายน 2026 | เวลาในตารางเป็น **ประเทศไทย (UTC+7)** | ต้องตรวจ Q&A และเกณฑ์จริงก่อนใช้

## หนึ่งชั่วโมงเตรียมก่อนเริ่ม sprint

| นาที | Step และการตัดสินใจ | A: Java | B: React | C: Integration | Skill ที่ใช้ / ผ่านเมื่อ |
|---|---|---|---|---|---|
| 00–10 | เช็กเครื่อง/สิทธิ์ และวันส่ง | JDK/Maven/DB access | Node/IDE/Copilot | repo, Podman/JMeter, AAD owner | ยังไม่ต้องเรียก skill; บันทึก blocker |
| 10–20 | ดูโจทย์และ 3 API | ตั้งคำถาม limit/state | ร่าง form/history/receipt | เก็บโจทย์/มาตรฐาน API | `cds-contract` หลังอ่าน rules; unknowns มี owner |
| 20–30 | สถานะและ contract draft | เสนอ fields/money | ตรวจว่า UI ครบ fields | คุม draft และ scope | `cds-contract`; ทุกคนเห็นตัวอย่างเดียวกัน |
| 30–40 | ทดสอบ tooling แบบปลอดข้อมูลจริง | ตรวจ build/dependencies | ตรวจ UI UX Pro Max ถ้าอนุญาต | ตรวจ Podman Compose และ JMeter | `cds-ui-review` เมื่อ UI skill พร้อม; fallback มีอยู่ |
| 40–50 | วาดเส้นทางข้อมูลขั้นต่ำ | boundary ของ Java modules | เส้นทาง UI → API | AAD/DB boundaries | `cds-architecture-defense`; diagram ตรง implementation plan |
| 50–60 | ล็อกเจ้าของงานและ prompts | รับ `01-backend` | รับ `02-frontend` | รับ `00-kickoff`, `03-integration` | `docs/decisions.md` มี unresolved items ชัด |

ถ้าชั่วโมงนี้อยู่ในเวลาของ briefing ให้ใช้ตารางเป็น checklist และฟังประกาศจริงเป็นหลัก หลีกเลี่ยง install third-party package ก่อนทราบ policy

## Day 1 — การจำลอง sprint ตามกำหนดการที่ได้รับ

| เวลาไทย | Step / ทางผ่าน | A | B | C | Skill / prompt ที่ใช้ | หลักฐานออกจากช่วงนั้น |
|---|---|---|---|---|---|---|
| 08:30–09:45 | Check-in / ฟังประกาศโจทย์ | จด business questions | จด UI scope | จด submission/rules | ไม่มี prompt implementation | brief ที่แก้ตามผู้จัด |
| 09:45–10:00 | Q&A กติกา | ถาม DB2/limit | ถาม scope UI | ถาม AAD, prep skill, deadline | `cds-contract` สำหรับกรองคำถาม | assumption ที่ confirmed / pending |
| 10:00–10:30 | Contract gate | ตั้ง skeleton Java | เตรียม flow และ component library | OpenAPI, roles, acceptance criteria | `cds-contract`, `cds-architecture-defense`, `00-kickoff` | contract ที่ A/B ยอมรับ; blockers |
| 10:30–11:30 | Vertical slice แรก | POST withdrawal, GET history | withdrawal form + history | test fixtures, AAD/DB spike | `cds-contract`, `cds-ui-review`, `01/02/03` | request จาก UI เข้าถึง API ได้ |
| 11:30–12:00 | Integration checkpoint | แก้ contract mismatch | ต่อจริง/สถานะ UI | smoke test + contract diff | `cds-verification` เริ่มตรวจจริง | demo flow สั้นที่รันได้ หรือ blocker เฉพาะ |
| 12:00–13:00 | พักกลางวัน | พัก | พัก | พัก | ไม่จำเป็น | เก็บเวลา sprint หลังพัก |
| 13:00–14:30 | Receipt + rights | transaction, idempotency, audit | receipt detail + confirm/error | AAD authorization checks | `cds-contract`, `cds-ui-review`, `cds-verification` | ตัวอย่างที่ยืนยันได้ + ตอบการกดซ้ำ |
| 14:30–16:00 | Reliability gate | domain/concurrent tests | integration/UI states | JMeter CLI, startup, DB observations | `cds-verification` | test matrix และ performance report ตามจริง |
| 16:00–17:00 | Review/defense | แก้ defect สำคัญ | ตรวจ browser/keyboard | clean-start rehearsal, ADR, evidence | `cds-verification`, `cds-architecture-defense`, `cds-ui-review` | คำสั่งรันที่คนอื่นทำซ้ำได้; demo script |
| 17:00–17:30 | Feature freeze | แก้เฉพาะ blocker | ตรวจ demo | docs/submission checklist | `cds-verification` | reviewed revision, ยังไม่อ้างงานที่ไม่เสร็จ |
| 17:30–17:50 | Submit | ช่วยตรวจ repo | ช่วยตรวจ UI/ลิงก์ | ส่ง SharePoint และยืนยันไฟล์ | `docs/submission-checklist.md` | หลักฐานว่าอัปโหลดสำเร็จ |
| 17:50–18:00 | Buffer | พร้อม standby | พร้อม standby | ตรวจลิงก์/ไฟล์ | ไม่มีการเพิ่ม scope | ส่งเสร็จก่อน 18:00 |
| หลัง 18:00 | Demo / presentation ตามกติกา | ตอบ correctness | คลิกเดโม | Q&A, architecture | `cds-architecture-defense` | คำอธิบายตรงกับ source ที่ส่ง |

## หากงานไม่ทัน ให้ตัดตามนี้

1. รักษา **สาม API + contract + persisted flow** ก่อน โดยใช้ data fixture ที่แสดงที่มาชัด
2. รักษา **AAD หรือบันทึก blocker พร้อมหลักฐาน** และตรวจ role/branch ฝั่ง server
3. รักษา **tests ของเงิน, scope, duplicate receipt และ data consistency**
4. ทำ JMeter รอบเล็กที่ reproducible พร้อมข้อจำกัดของเครื่อง
5. ตัดฟีเจอร์ Center ที่ไม่อยู่ในสาม API, กราฟ/เอฟเฟกต์ UI, Redis/queue/read replicas และสไลด์ที่ไม่ช่วยอธิบายเดโม

การยืนยันรับเงินจากรายการ seed ที่มีสถานะ eligible ต้องระบุในเดโมว่าเป็น seeded fixture หาก approval/dispatch ยังไม่ได้พัฒนา ห้ามสื่อว่า flow นั้นครบจริง

## การใช้ IBM clinic (ถ้ามีตามกำหนดการ)

จดคำถามเฉพาะที่ติดจริง เช่น DB2 connection/transaction/schema หรือสัญญาของ integration แล้วให้ C นัดรอบไม่เกิน 15 นาที พร้อม error ที่คัดแล้วและไม่มี secrets หากแก้ได้ด้วยหลักฐานใน repo ก่อน ให้เวลา clinic กับ blocker ถัดไป

## เช็กสั้นก่อนจบแต่ละช่วง

- A/B/C แจ้ง: สิ่งที่รันสำเร็จ / สิ่งที่ไม่รัน / blocker / contract change / คนรับช่วงต่อ
- C บันทึก Copilot prompt และ human review ใน [ai-usage](ai-usage.md) เฉพาะงานจริง
- ตัวเลขจำนวนผู้ใช้ทั้งหมดเป็น population context ไม่ใช่ concurrent load; JMeter report ต้องระบุ environment และ measured values
