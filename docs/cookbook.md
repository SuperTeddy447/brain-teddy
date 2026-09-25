# Cookbook — เดินงาน CDS ตั้งแต่เตรียม 60 นาทีถึงส่ง

เวลาในเอกสารเป็น **เวลาไทย (UTC+7) วันที่ 25 กันยายน 2026** ตามกำหนดการที่ได้รับ. C จดการเปลี่ยนแปลงจากผู้จัดทันที. ใช้ [brief](brief.md), [requirements](requirements.md) และ [decisions](decisions.md) เป็นแหล่งข้อเท็จจริงระหว่างแข่ง; `Pasted text.txt` เป็นบริบทเก่า ไม่ใช่กติกาที่ได้รับการยืนยัน. หากไม่เข้าใจ flow ให้เปิด [field guide](field-guide.md) หรือถาม Copilot ด้วย `/cds-explain-flow`

## แกนงานที่พาไปถึงเดโม

1. ทำ **P0** ใน [requirements](requirements.md): 3 API ที่ข้อมูลถูกต้อง, ตรวจสิทธิ์, persist/audit, UI ใช้จริง, tests, run และหลักฐาน Copilot
2. ทำ **P1 มากสุด 1–2 เรื่อง** หาก P0 เสถียร: Center เห็นรายการผ่าน History ตามสิทธิ์, ค้นคืนด้วย filter/page/reference, หรือ trace ID; หยุดเริ่ม P1 หลัง 14:30
3. อธิบาย **P2** เป็น [migration map](legacy-modernization.md): ระบบ ASP Classic/VB6/DB2 เดิมยังทำส่วนที่ไม่ย้าย; สิ่งที่ยังไม่มีต้องบอกว่า future/blocked/fixture

หลักฐานที่ใช้ตอบเกณฑ์คะแนนอยู่ใน [judging playbook](judging-playbook.md). ไม่มีแผนใดรับประกันชนะ; หากผู้จัดเปลี่ยนโจทย์ ให้ C ปรับ P0 ก่อนแจกงาน

## ก่อนเริ่ม: แจกสิทธิ์แก้ไฟล์

| คน | Owns | ขอ review จาก | ห้ามแก้ร่วมโดยไม่ตกลง |
|---|---|---|---|
| A — Backend | `backend/` และ backend tests | C เรื่อง auth/contract | `contracts/`, `frontend/` |
| B — Frontend | `frontend/` และ UI tests | A เรื่อง response/errors | `contracts/`, `backend/` |
| C — Integration lead | `contracts/`, `infra/`, `tests/`, shared `docs/` | A/B ก่อนเปลี่ยน contract | โค้ด business ใน `backend/` |

`contracts/`, `backend/`, `frontend/`, `infra/`, `tests/` คือ **ตำแหน่งเป้าหมายที่จะสร้างระหว่างแข่ง** ไม่ใช่ไฟล์ที่มีอยู่แล้ว. สื่อสาร contract change ก่อน merge/ส่งต่อ. ถ้าคน A/B ต้องแก้ shared docs ให้ C รับข้อมูลและเป็นผู้รวม

## 60 นาทีเตรียมก่อน sprint

ช่วงนี้เป็น checklist ที่บีบให้จบภายใน 60 นาที จะทำก่อนงานหรือระหว่าง briefing ตามเวลาจริงก็ได้

| นาที | คน A | คน B | คน C | Copilot / skill | ต้องผ่านก่อนเดินต่อ |
|---|---|---|---|---|---|
| 00–10 | เช็ก JDK/build/DB client | เช็ก Node/IDE/browser | เช็ก repo, Copilot, Podman, JMeter, สิทธิ์ SharePoint | [setup](copilot-setup.md); ยังไม่ให้ Agent เขียนโค้ด | ทุก blocker มี owner |
| 10–20 | อ่าน 3 API และถามกฎเงิน/limit | ร่าง 3 หน้าหลัก | จดเกณฑ์, deadline, AAD/DB/API standard, legacy ownership | `/cds-discovery`, `cds-contract`, `cds-legacy-modernization` | คำถามที่กระทบ correctness ถูกยื่นใน Q&A |
| 20–30 | เสนอ fields/state/error | ตรวจว่า UI ต้องใช้ fields ใด | รวม contract draft v0 | `/cds-kickoff`, `cds-contract` | A/B เห็น request/response ตัวอย่างเดียวกัน |
| 30–40 | ทดสอบ build เครื่องเปล่า | ทดลอง UI skill แบบจำกัดเวลา | เช็ก Compose provider และ JMeter CLI | `cds-ui-review`; UI UX Pro Max ถ้าอนุญาต | tooling รันได้หรือมี fallback |
| 40–50 | วาง transaction/persistence boundary | วาง flow/empty/error states | วาดสถาปัตยกรรมขั้นต่ำ | `cds-architecture-defense` | diagram ตรงกับสิ่งที่จะทำ |
| 50–60 | รับ backlog `backend/` | รับ backlog `frontend/` | ล็อก API contract, เจ้าของงาน, checkpoint | `/cds-kickoff` | `docs/decisions.md` ระบุ confirmed/pending |

**กฎตัดสินใจเร็ว:** ถ้า AAD/DB2 ไม่พร้อม ให้บันทึก blocker พร้อม owner และเวลา; ใช้ fallback เฉพาะเมื่อผู้จัดอนุญาต. ห้ามเรียก mocked auth ว่า AAD สำเร็จ. หาก Q&A ยังไม่ตอบเรื่อง prepared material/third-party skill ให้ใช้เฉพาะสิ่งที่อนุญาตแล้ว

## Day 1: ตารางเดินงานทั้งวัน

| เวลา | A — Backend | B — Frontend | C — Integration/Lead | Prompt + skill | Exit gate และหลักฐาน |
|---|---|---|---|---|---|
| 08:30–09:45 | ฟังโจทย์/จดกฎ business | จด scope หน้า UI | จดกติกา, legacy boundary, สิ่งที่ส่ง, cutoff | `/cds-discovery`, `cds-contract`, `cds-legacy-modernization` | ปรับ `brief.md`, `requirements.md`, แยก confirmed/pending |
| 09:45–10:00 | ถาม DB2/limit/state | ถาม branch/center UI | ถาม AAD, API standard, source of truth, AI/skill policy, scoring, deadline | `cds-contract` | คำตอบลง `decisions.md`; P0/P1 ชัด |
| 10:00–10:30 | ตั้ง Java skeleton หลัง contract | ตั้ง React skeleton หลัง contract | เขียน OpenAPI v1, architecture, task boundaries | `/cds-kickoff`; `cds-contract`, `cds-architecture-defense` | A/B review contract; unresolved blockers มี owner |
| 10:30–11:30 | POST withdrawal + GET history + persistence | withdrawal form + history บน contract | AAD/DB spike, fixture และ smoke path | `/cds-backend-slice`, `/cds-frontend-slice`, `/cds-integration`; `cds-contract`, `cds-ui-review` | UI เรียก API จริงอย่างน้อย 1 รายการ |
| 11:30–12:00 | แก้ mismatch กับ contract | ต่อ UI กับ API จริง | รัน smoke, เปิด bug list | `/cds-verify`; `cds-verification` | withdrawal + history ผ่าน หรือระบุ blocker ชัด |
| 12:00–13:00 | พัก; ส่งสถานะให้ C ก่อนพัก | พัก | อัปเดต backlog/usage ตามจริง | ไม่ต้องเรียก prompt | รู้สิ่งที่เริ่มหลังพัก |
| 13:00–14:30 | receipt transaction, idempotency, audit, scope | receipt detail/review/confirm/error | ตรวจ AAD API authorization และ state fixtures; อนุมัติ P1 สูงสุด 1–2 เรื่องถ้า P0 ผ่าน | `/cds-receipt-hardening`, `/cds-frontend-slice`, `/cds-integration`; `cds-contract`, `cds-ui-review` | success และ duplicate/cross-branch rejection แสดงได้ |
| 14:30–16:00 | ทดสอบ money, rollback, concurrency | ทดสอบ UI states/keyboard | smoke 3 API, JMeter รอบเล็ก, metrics | `/cds-verify`; `cds-verification` | `test-matrix.md` และ `performance.md` มีผลจริง |
| 16:00–17:00 | แก้ defect ที่ขวางเดโม; ตรวจ data claim | browser rehearsal, narrow layout; ตรวจ UI claim | 16:00–16:15 migration map; ต่อด้วย clean start, diagram/ADR, demo script | `/cds-modernization`, `/cds-rehearse`; `cds-legacy-modernization`, `cds-verification`, `cds-architecture-defense`, `cds-ui-review` | เพื่อนร่วมทีมรันตาม README ได้; demo สำเร็จ 1 รอบ; map ตรงระบบ |
| 17:00–17:30 | Feature freeze; แก้ blocker เท่านั้น | ตรวจ label/error/links | review source revision และเอกสารส่ง | `/cds-submit`; `cds-verification` | checklist เกือบครบ; ไม่มี claim เกินสิ่งที่ทดสอบ |
| 17:30–17:50 | ช่วยตรวจไฟล์สุดท้าย | ช่วยตรวจ UI/เดโม | ส่ง SharePoint และเปิดไฟล์ที่ส่งตรวจ | `/cds-submit` | อัปโหลดสำเร็จ; เก็บหลักฐานเวลา/ไฟล์ |
| 17:50–18:00 | standby | standby | buffer แก้ submission issue เท่านั้น | ไม่เริ่ม feature ใหม่ | ส่งครบก่อน 18:00 |
| หลัง 18:00 | ตอบ business/transaction | คลิก demo | นำเสนอ architecture, AI evidence, limits | `/cds-rehearse` ใช้ script ที่ซ้อมแล้ว | พูดตรงกับ source ที่ส่ง |

## ลำดับ prompt แบบคัดลอกได้

Prompt files อยู่ใน `.github/prompts/`. ใน VS Code Copilot Chat ให้เลือก Agent ตามคน แล้วพิมพ์ `/ชื่อ` ตามตาราง; ถ้า slash menu ไม่ขึ้น เปิดไฟล์แล้ววางเนื้อหาใน chat. แต่ละ prompt ระบุ input, งาน, ขอบเขตไฟล์ และหลักฐานที่ต้องส่งกลับ

| ขั้น | C | A | B |
|---|---|---|---|
| Brief/Q&A | `/cds-discovery` | ตรวจคำถามกับ C | ตรวจ UI scope กับ C |
| Contract | `/cds-kickoff` | review contract | review contract |
| Slice 1 | `/cds-integration` | `/cds-backend-slice` | `/cds-frontend-slice` |
| Slice 2 | `/cds-integration` | `/cds-receipt-hardening` | `/cds-frontend-slice` พร้อมขอบเขต receipt |
| Verification | `/cds-verify` | แก้ defect ที่ C แจ้ง | แก้ defect ที่ C แจ้ง |
| Modernization/defense | `/cds-modernization` | ตรวจ data/transaction claim | ตรวจ UI claim |
| Rehearsal | `/cds-rehearse` | ร่วมซ้อม | ร่วมซ้อม |
| Submit | `/cds-submit` | ตรวจ revision | ตรวจ UI |

ตัวอย่างเสริมตอนใช้ prompt: `Use cds-contract. Confirmed rule: [คำตอบผู้จัด]. Pending: [ข้อที่ยังไม่รู้]. Work only in your owned paths. Return files changed, tests run and blockers.` ใช้ **คำตอบผู้จัดจริง** แทนวงเล็บ. ถ้าต้องเริ่ม chat ใหม่ ให้แนบ `docs/brief.md`, `docs/decisions.md` และ contract รุ่นล่าสุด

### การตัดสินใจระหว่างแข่ง

| ถ้าเกิด | ให้ทำ | อย่าพูดเกินหลักฐาน |
|---|---|---|
| AAD/DB2 access ติด | C ระบุ owner/เวลา/ผลที่ลอง; A/B ทำส่วนไม่พึ่ง access และ fallback ที่อนุญาต | mock login/DB fallback ไม่ใช่ integration จริง |
| ขอเบิกได้แต่ยังรับไม่ได้ | ใช้ eligible fixture ที่ระบุชัด แล้วสาธิต Receipt API แยก | อย่าบอกว่ารายการใหม่ผ่าน approval/dispatch จริง |
| Center view ไม่อยู่ใน contract | ถามผู้จัด; ถ้าสิทธิ์อนุญาต ใช้ History เดิมกับ scope ที่ตกลง | อย่าสร้าง endpoint ที่ 4 เพราะอยากให้เดโมดูครบ |
| เวลาหลัง 14:30 เหลือน้อย | หยุด P1 แล้วทดสอบ 3 API, clean start, AI evidence | อย่าขยาย UI/สถาปัตยกรรม |
| JMeter ตัวเลขไม่ดี | รายงาน workload/เครื่อง/คอขวด; แก้ query/index/pool เฉพาะที่เห็น | อย่าเปลี่ยน workload เพื่อทำตัวเลขให้สวย |

## ถ้างานไม่ทัน ให้ตัดตามลำดับ

1. รักษา 3 API + persisted flow + OpenAPI ที่ตรงกัน
2. รักษา auth/scope ฝั่ง server หรือบอก blocker AAD จริง; ห้ามทำข้อมูลข้ามสาขาหลุด
3. รักษา money, receipt duplicate และ transaction tests
4. รักษา clean start, JMeter รอบเล็กและหลักฐานที่ตรวจซ้ำได้
5. ลด dashboard/กราฟ/อนิเมชัน, Center functions ที่ไม่อยู่ใน 3 API, Redis/queue/replica, สไลด์ที่ไม่ช่วยเดโม; เก็บ migration map เป็นเอกสารสั้นตามจริง

**Fallback demo:** ใช้ seeded eligible receipt พร้อมป้ายชัดว่า approval/dispatch ไม่ได้พัฒนา. ถ้า AAD หรือ DB2 เข้าไม่ได้ ให้แสดงหลักฐาน blocker และสิ่งที่รันกับ fallback โดยไม่เรียกว่า production integration

## เช็กก่อนส่ง

ให้ C เปิด [submission checklist](submission-checklist.md) และตรวจไฟล์ที่อัปโหลดจริง. Demo 5–7 นาที: context 30s → login/branch 30s → withdrawal/history 90s → eligible receipt 60s → duplicate/cross-branch rejection 45s → AI corrections/tests 60s → trade-off/limits 45s. ปรับตามเวลาผู้จัด
