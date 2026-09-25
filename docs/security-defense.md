# OWASP สำหรับ CDS — ตรวจอะไรและตอบกรรมการอย่างไร

สถานะ: **ชุดตรวจสำหรับวันแข่ง ยังไม่ใช่ผลผ่านความปลอดภัย**. ในโจทย์สาม API ให้ใช้ [OWASP API Security Top 10:2023](https://owasp.org/projects/api-security-project) เป็นรายการหลัก; [OWASP Top 10:2025](https://top10.owasp.org/2025/) เป็นรายการสำหรับเว็บแอปโดยรวม. สองชุดนี้คนละชื่อและคนละขอบเขต. SA ถามผู้จัดว่าต้องอ้างฉบับใด หากไม่ระบุ ให้กล่าวทั้งคู่โดยโยงกับสิ่งที่ทดสอบจริง

## สินทรัพย์และทางเข้าที่เราต้องปกป้อง

ข้อมูลสำคัญ: คำขอเบิก, จำนวนเงิน/วงเงิน, สาขา, สถานะรับเงิน, token, audit. ทางเข้าหลัก: React → Java API → DB และการเชื่อม Entra/DB2 ที่อนุญาต. ผู้โจมตีตัวอย่าง: ผู้ใช้สาขาที่ล็อกอินถูกต้องแต่ลองเปลี่ยน `branchId` หรือ `requestId`; ผู้ที่มี token ผิด audience; ผู้ใช้ที่ส่งคำขอซ้ำ/พร้อมกัน

## Map OWASP API Top 10 กับสาม API

| หมวด | จุดเสี่ยงใน CDS | สิ่งที่ต้องตรวจ/พิสูจน์ | Owner / หลักฐาน | ผลจริง |
|---|---|---|---|---|
| API1 Broken Object Level Authorization | เดา ID รายการของสาขาอื่นแล้วอ่าน/ยืนยัน | query และ mutation ตรวจสิทธิ์ต่อ *รายการ* ที่ API; สาขามาจาก identity/server | BE; T05 | Not run |
| API2 Broken Authentication | token หาย, หมดอายุ, issuer/audience ผิด | API ตรวจลายเซ็น/issuer/audience/เวลาและสิทธิ์ตาม config จริง; ไม่ใช้ ID token แทน access token | BE+SA; T04 | Not run |
| API3 Broken Object Property Level Authorization | ส่ง `branchId`, `status`, `approvedAmount` หรือ audit actor เอง | รับเฉพาะ field ใน contract; field ที่ server เป็นเจ้าของเปลี่ยนไม่ได้และไม่รั่วใน response | BE; T20 | Not run |
| API4 Unrestricted Resource Consumption | history ดึงหน้ามหาศาล, payload ใหญ่, request ถี่ | page size/payload จำกัด; load test แสดงเครื่อง/ภาระงานและ error จริง | BE+SA; T06/T22 | Not run |
| API5 Broken Function Level Authorization | role สาขาเรียกสิทธิ์ Center หรือยืนยันรายการผิดบทบาท | API ตรวจ role/scope แยกตาม operation; UI ซ่อนปุ่มเป็นเพียง UX | BE; T21 | Not run |
| API6 Unrestricted Access to Sensitive Business Flows | ส่ง request ที่ limit, ยืนยันรับ หรือ retry อย่างอัตโนมัติซ้ำ | กฎวงเงิน, state, idempotency/concurrency, audit และ rate/abuse control ตามข้อกำหนด | BE+SA; T03/T08–T12 | Not run |
| API7 Server Side Request Forgery | endpoint รับ URL แล้ว server ไปเรียกปลายทางที่ผู้ใช้เลือก | ยืนยันว่า 3 APIs ไม่มี user-supplied URL; หากมี integration URL ให้ใช้ allowlist/config ฝั่ง server | SA; contract/code review | Not run |
| API8 Security Misconfiguration | debug/admin route เปิด, CORS กว้าง, secrets ใน repo/log | ตรวจ config ที่รันจริง, route, error detail, log และ `.gitignore`; ห้ามอ้างผ่านหากยังไม่ได้ตรวจ | SA+BE; T23 | Not run |
| API9 Improper Inventory Management | มี endpoint ทดลอง/เวอร์ชันเก่าที่ไม่อยู่ OpenAPI | เทียบ route ที่เปิดจริงกับ OpenAPI และ scope/owner | SA; T25 | Not run |
| API10 Unsafe Consumption of APIs | เชื่อข้อมูลจาก Entra/DB2/GL/SMTP โดยไม่ตรวจ | ตรวจ token/ข้อมูลภายนอกตาม contract; ส่วน GL/SMTP ที่ยังไม่เชื่อมให้ระบุ future ไม่อ้างว่าปลอดภัยแล้ว | SA+BE; integration evidence หรือ Not run | Not run |

**เสริมสำหรับเว็บแอป:** OWASP Top 10:2025 ให้เตรียมตอบเรื่อง access control, injection/parameterized queries, software supply chain, misconfiguration, logging และ exceptional conditions. FE ตรวจว่า UI ไม่เก็บ token/ข้อมูลสาขาเก่าอย่างผิดวิธีและไม่แสดง error ละเอียดเกินไป; BE ตรวจว่า validation/authorization อยู่ที่ API. ตาราง API ข้างบนคือ checklist หลักของเดโม

## รอบตรวจ 30 นาทีในช่วง 14:30–16:00

1. **14:30–14:35 — SA ตั้งโจทย์:** เลือก synthetic users สองสาขา, Center role ถ้ามี, token ที่หมดอายุ/ผิด audience และรายการ eligible. บันทึก revision/environment
2. **14:35–14:50 — BE+SA ทดสอบ P0:** T04 (token), T05 (ข้ามสาขา), T20 (แก้ field ของ server), T21 (role ผิด), T09–T11 (ยืนยันซ้ำ/พร้อมกัน). FE ช่วยดูว่าหน้าจอไม่ทำให้ผู้ใช้เข้าใจผิด
3. **14:50–14:55 — SA ตรวจ config/inventory:** T23/T25 เท่าที่ทำได้; บันทึก Not run ถ้าไม่ทัน
4. **14:55–15:00 — ตัดสิน:** defect ที่ทำให้เงิน/สิทธิ์ผิดให้ BE แก้ก่อน JMeter และ demo; SA อัปเดต [test matrix](test-matrix.md) ด้วยคำสั่ง/ผลจริง

หาก AAD หรือ DB2 ไม่มี access ให้ทดสอบสิ่งที่ทำได้ใน fallback และระบุว่า token/DB2 integration ยัง Blocked. ไม่มีคำว่า “ผ่าน OWASP Top 10 ทั้งหมด” จากการตรวจ 30 นาที

## คำถามกรรมการที่ควรซ้อม

| ถาม | ตอบโดยชี้หลักฐาน |
|---|---|
| ถ้าสาขา A เปลี่ยน ID เป็นของสาขา B จะเห็นไหม? | “API ตรวจสิทธิ์บน record ทุกครั้ง; นี่คือ T05 กับ response และข้อมูลที่ไม่เปลี่ยน” |
| Token ถูกต้องแต่เป็นของ API อื่นล่ะ? | “ตรวจ `aud` และ issuer ตาม config; T04 ใช้ token ผิด audience แล้วปฏิเสธ” หรือบอกว่า AAD ถูก block หากยังไม่ทดสอบจริง |
| ส่ง `branchId`/`status` ใน body เองได้ไหม? | “field นี้ server เป็นเจ้าของ; T20 พิสูจน์ว่าไม่ถูกแก้หรือเปิดเผยเกิน contract” |
| กดรับเงินพร้อมกันสองครั้งล่ะ? | “transaction/unique หรือ concurrency guard ทำให้มีหนึ่ง transition; T09–T11 และ audit เป็นหลักฐาน” |
| ป้องกัน SQL injection, endpoint ทดลองและ secret อย่างไร? | “ใช้ parameterized queries/ตรวจ route เทียบ OpenAPI/ตรวจ config และ repo; ชี้ T23–T25 ที่รันแล้ว พร้อมรายการที่ยังไม่รัน” |
| ผ่าน OWASP ครบหรือยัง? | “เราทำ threat map ครบ API Top 10; ผลทดสอบที่ผ่านจริงอยู่ใน test matrix. หมวดที่ไม่เกี่ยวหรือยังไม่เชื่อมถูกระบุเหตุผลและสถานะ” |

อ้างอิง: [OWASP API Security Top 10](https://api-security.owasp.org/editions/2023/en/0x11-t10/), [Microsoft Entra access-token validation](https://learn.microsoft.com/en-us/entra/identity-platform/access-tokens), [OWASP Top 10:2025](https://top10.owasp.org/2025/)
