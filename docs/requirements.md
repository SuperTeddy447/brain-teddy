# ข้อกำหนดและเกณฑ์ผ่านสำหรับวันแข่ง

สถานะ: **working plan ก่อนฟังโจทย์จริง**. C แก้ตารางนี้ทันทีหลัง briefing; คำประกาศและ API standard ของผู้จัดมีสิทธิ์เหนือแผนนี้. P0 = ต้องรักษา, P1 = เพิ่มเมื่อ P0 ผ่าน, P2 = อธิบายแผนต่อยอด. `Txx` อ้างถึง [test matrix](test-matrix.md). ห้ามติ๊กผ่านจากโค้ดหรือคำตอบ AI อย่างเดียว

## P0 — ผลงานที่ต้องทำงานจริง

| ID | ผลลัพธ์ | เกณฑ์ผ่านที่ตรวจได้ | Owner | เวลาเป้าหมาย |
|---|---|---|---|---|
| R01 | Contract 3 business APIs | OpenAPI ตรง implementation; A/B ตกลง amount, state, error, scope, retry | C, A/B review | 10:30 |
| R02 | Withdrawal request | คำขอที่ถูกต้องถูกเก็บหนึ่งครั้ง; invalid amount/กฎที่ยืนยันแล้วไม่เปลี่ยนข้อมูล; มี reference | A | 12:00 |
| R03 | History inquiry | สาขาเห็นเฉพาะสิทธิ์ตน; filter/page/order แน่นอน; reload แล้วยังเห็นข้อมูล | A/B | 12:00 |
| R04 | Receipt confirmation | รายการ eligible ยืนยันได้หนึ่ง transition; wrong state/duplicate/cross-branch ไม่สร้างผลเงินซ้ำ | A/B | 14:30 |
| R05 | Identity and authorization | ถ้ามี AAD จริง: SPA login ตาม config จริง, API ตรวจ token และ scope; ไม่มีสิทธิ์ต้องถูกปฏิเสธ. ถ้าขาดสิทธิ์: blocker พร้อม owner และ fallback ที่ผู้จัดอนุญาต | C/A | 14:30 |
| R06 | Persistence and audit | ใช้ DB ที่อนุมัติ; mutation กับ audit สอดคล้องใน transaction; restart แล้วข้อมูลยังอยู่. ถ้า DB2 เข้าไม่ได้ ระบุ fallback ชัด | A/C | 14:30 |
| R07 | React task flow | สาขาทำ create/history/receipt ได้; ยอด สาขา เลขอ้างอิง และผลลัพธ์อ่านชัด; error/unknown outcome ไม่ชวนกดซ้ำมั่ว | B | 14:30 |
| R08 | Reliability/security evidence | Smoke 3 APIs; T01–T11 และ T20–T21 ที่ทำได้มีผลจริง; ทดสอบ rollback/concurrency และ OWASP review ตามเวลา/access | C/A | 16:00 |
| R09 | Run and load evidence | เพื่อนร่วมทีม clean start ได้; JMeter อย่างน้อยหนึ่งรอบ correctness และรอบเล็กหาก environment พร้อม; ระบุ p95/error/ข้อจำกัดตามจริง | C | 17:00 |
| R10 | Submission and AI evidence | README วิธีรัน, diagram ตรงระบบ, Copilot prompt → output → human correction → test, checklist และไฟล์ที่ส่งเปิดได้ | C | 17:50 |

**กฎของเงินและ state:** ใช้เฉพาะ limit, denomination, cutoff, calendar, receipt eligibility และ partial/mismatch policy ที่ผู้จัดยืนยัน. สิ่งที่ยังไม่รู้ต้องมี `Pending + owner` ใน [decisions](decisions.md). การมี endpoint ที่ตอบ 200 ยังไม่ถือว่าผ่านหากข้อมูล/สิทธิ์/ผลธุรกรรมผิด

## P1 — ตัวเพิ่มความน่าเชื่อถือภายในเวลาเดิม

ทำเมื่อ R01–R07 ผ่านและยังมีเวลาสำหรับ R08–R10. **ไม่เริ่ม P1 ใหม่หลัง 14:30; ถ้ากระทบเดโมให้ย้อนกลับทันที**

| ลำดับ | สิ่งเพิ่ม | ใช้ API ใหม่ไหม | หลักฐานที่ต้องแสดง | งบเวลา |
|---|---|---|---|---|
| 1 | Center ดูรายการผ่าน History ด้วย role/scope ที่ผู้จัดอนุญาต | ไม่ควรต้องเพิ่ม; ยืนยัน contract ก่อน | user อีก role เห็นรายการที่มีสิทธิ์; cross-branch ถูกปฏิเสธ | 20 นาที |
| 2 | Filter/page และสถานะที่อ่านง่าย พร้อม reference เพื่อค้นคืน | ไม่ | history query + UI และ T06 | 15 นาที |
| 3 | Correlation/reference ID ใน response, log และ audit | ไม่ | ไล่หนึ่งรายการจาก UI → API → audit ได้ | 15 นาที |
| 4 | JMeter comparison หลังแก้คอขวดจริง เช่น query/index/pool | ไม่ | ก่อน/หลังบน workload เดียวกัน; ถ้าวัดไม่ได้ไม่อ้างเร็วขึ้น | 20 นาที |

เลือก **อย่างมาก 1–2 รายการ** ที่เสริม P0 และอยู่ในสัญญา API. เวลารวมของ P1 ไม่เกิน 30 นาทีในช่วง sprint; หาก P0 เสี่ยง ให้ข้ามทั้งหมด. UI สวยขึ้นโดยไม่มีการใช้งานดีขึ้นไม่ใช่ P1 ที่คุ้ม

## P2 — แผนต่อยอดหลังแข่ง ไม่สาธิตเป็นของที่เสร็จแล้ว

Center กำหนดวงเงิน/อนุมัติ, dispatch, GL, SMTP/vendor, reporting/Crystal Reports, migration ข้อมูลเก่า, cutover/rollback production, Redis/queue/replica. ใส่ใน [legacy modernization](legacy-modernization.md) เป็น boundary และ roadmap. เพิ่มเป็น code เฉพาะเมื่อโจทย์จริงกำหนดและ C ตัด scope ใหม่ก่อน 10:30

## Gate ที่หยุดการอ้างผลงาน

- AAD ไม่พร้อม → ไม่เรียก mock login ว่า AAD migration
- DB2 ไม่พร้อม → ไม่เรียก fallback ว่า DB2 integration
- ไม่มี API สำหรับ approval/dispatch → receipt demo ต้องบอกว่าใช้ seeded eligible record; ห้ามพูดว่า request เดียววิ่งครบวงจร
- JMeter ยังไม่รัน → ไม่ใส่ throughput/p95/จำนวน concurrent users เป็นผลจริง
- Coverage ยังไม่วัด → ไม่อ้าง 70% หรือ 100%; เป้า coverage ต้องยืนยันกับผู้จัด
- 612 สาขา/1,980 บัญชีผู้ใช้จากข้อมูลโจทย์ ไม่ใช่ปริมาณการใช้งานพร้อมกัน

อ้างอิงหลักการ: [OWASP เรื่องสิทธิ์รายรายการ](https://api-security.owasp.org/editions/2023/en/0xa1-broken-object-level-authorization/), [Microsoft Entra auth code + PKCE](https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-auth-code-flow)
