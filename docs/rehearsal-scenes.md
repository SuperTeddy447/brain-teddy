# ลองใช้ README แบบเป็นฉาก — ทีม BE / FE / SA

เอกสารนี้เป็นบทซ้อมให้เห็นว่า [README](../README.md) ใช้อย่างไรในวันแข่ง. เวลาและผลลัพธ์เป็นเป้าหมาย; กติกาที่ผู้จัดประกาศจริงต้องมาก่อน. ในเอกสารอื่น **BE = A**, **FE = B**, **SA = C**. SA ในทีม 3 คนทำหน้าที่วิเคราะห์โจทย์, คุม API contract, integration, หลักฐานและการส่งงาน

## ฉาก 0 — ก่อนเริ่ม sprint 60 นาที: ตั้งโต๊ะทำงาน

**SA พูดกับทีม:** “วันนี้เราทำสาม API ให้ถูกและเดโมได้ก่อน. BE ดู Java/ข้อมูล, FE ดู React, ผมดู contract/integration/ส่งงาน. ทุกคนเปิด README กับ repo เดียวกัน”

1. ทั้งสามคนเปิด [คู่มือ Copilot](copilot-setup.md) และตรวจว่า agent, 5 skills, slash prompts ใช้ได้ใน IDE บริษัท
2. BE เช็ก Java/build/DB access; FE เช็ก Node/React/browser; SA เช็ก Git/Podman/JMeter, Entra owner และช่องทาง SharePoint
3. SA เขียน blocker ที่พบลง [decisions](decisions.md) พร้อมชื่อคนรับผิดชอบ; อ่าน [ตารางเตรียม 60 นาที](cookbook.md)

**ฉากจบเมื่อ:** รู้ว่าเครื่องมือไหนพร้อม/ติด และใครแก้. ไม่เสียเวลาทั้งทีมไล่ติดตั้งของเสริมพร้อมกัน

## ฉาก 1 — 08:30–10:00: ฟังโจทย์และตัดสินขอบเขต

SA เปิด [brief](brief.md), [requirements](requirements.md) และ [decisions](decisions.md), เลือก `cds-integration` ใน Copilot แล้วใช้ `/cds-discovery`. BE ถามเรื่อง amount, limit, state, DB2; FE ถามหน้าที่ต้องทำและบทบาทสาขา/ศูนย์เงินสด. SA ถาม API standard, AAD, สิทธิ์ข้อมูล, เกณฑ์คะแนน, ของที่ต้องส่ง และ deadline ใน Q&A

**ตัวอย่างการส่งงาน:** SA ส่งข้อความทีมว่า “ยืนยันแล้ว: [กฎจริง]. ยังไม่รู้: [รายการ+owner]. P0 วันนี้คือ [สาม API และข้อบังคับ]. P1 ทำได้เมื่อ P0 ผ่าน”

**ฉากจบเมื่อ:** ข้อกำหนดมีแหล่งที่มา; ความไม่รู้ถูกระบุว่า pending. ห้ามเอาตัวเลข limit หรือชื่อ state จากตัวอย่างในเอกสารไปเป็นกฎจริง

## ฉาก 2 — 10:00–10:30: SA ทำ contract แล้วปล่อย BE/FE

SA ใช้ `/cds-kickoff` ให้ Copilot ร่าง OpenAPI สาม operation. จากนั้นให้ BE ตรวจรูปเงิน, error, transaction, state/receipt; ให้ FE ตรวจว่า field และ response พอทำ form/history/confirm ได้. SA เป็นคนเดียวที่รวมการแก้ contract

**บทสนทนาตัวอย่าง:**

- BE: “สร้างคำขอแล้วได้เงินเลขอ้างอิงไหม? ส่งซ้ำด้วย key เดิมจะเกิดอะไร?”
- FE: “ถ้า token หมดอายุหรือไม่รู้ว่าคำขอสำเร็จหรือยัง หน้าจอควรแสดง error อะไร?”
- SA: “ผมใส่คำตอบลง contract และ decisions. ถ้าผู้จัดยังไม่ตอบ ผมติดป้าย proposed แล้วให้ทุกคนใช้ตัวอย่างเดียวกัน”

**ฉากจบเมื่อ:** BE/FE ยอมรับ request/response/error ตัวอย่างเดียวกัน และรู้งานที่ตัวเองทำ. นี่คือจุดเริ่มเขียนโค้ดคู่ขนาน

## ฉาก 3 — 10:30–12:00: ทำงานสามทางพร้อมกัน

| คน | งานแรก | Prompt | สิ่งที่ส่งต่อ |
|---|---|---|---|
| BE | Java withdrawal + history + persistence + tests ขั้นแรก | `/cds-backend-slice` | API ที่เรียกได้, ตัวอย่าง response, คำสั่งทดสอบ |
| FE | React form + history และสถานะ loading/error | `/cds-frontend-slice` | หน้าจอที่ใช้ contract เดียวกัน; mock แยกไว้ชัด |
| SA | ตรวจ AAD/DB access, fixture, Podman/smoke path | `/cds-integration` | blocker, env names, ลำดับเปิดระบบและ smoke |

FE เริ่มจากตัวอย่างใน contract ได้ ไม่ต้องรอ BE เขียนเสร็จ. เมื่อ BE มี API ให้ FE เปลี่ยนจาก mock ไปเรียกจริงและทดสอบหนึ่งคำขอจากหน้าจอ. **ทุก 15–20 นาที** ทุกคนตอบสั้น ๆ: “รันอะไรผ่าน / ติดอะไร / ต้องให้ใครช่วย / จะส่งอะไรตอน checkpoint”

**ฉากจบเมื่อ:** สร้างคำขอจาก UI แล้ว history เห็นรายการหลัง reload หรือทีมมี blocker เฉพาะจุดที่แก้ต่อได้. ถ้า field ไม่ตรง SA ตัดสิน contract และแจ้งทั้งคู่ก่อนแก้

## ฉาก 4 — 11:30–12:00: เช็กของจริงก่อนพัก

SA เรียก `/cds-verify` ทำ smoke withdrawal/history. BE แสดงผล test ที่รันจริง; FE แสดง API request จาก browser. SA บันทึก Passed/Failed/Blocked ใน [test matrix](test-matrix.md). ถ้าหน้าจอสวยแต่ยังเรียก API ไม่ได้ ให้หยุดแต่ง UI และแก้เส้นทางนี้ก่อน

**ฉากจบเมื่อ:** ทีมรู้ว่า UI → API → DB ผ่านจริงหรือไม่ และมีลิสต์ defect ที่ BE/FE รับไปแก้หลังพัก

## ฉาก 5 — 13:00–14:30: ยืนยันรับเงินและป้องกันการทำซ้ำ

BE ใช้ `/cds-receipt-hardening` ทำ receipt transaction, สิทธิ์, audit และ retry. FE เพิ่มหน้าทบทวนยอด/สาขา/เลขอ้างอิงก่อนกดยืนยัน. SA เตรียมรายการสังเคราะห์ที่มี state *eligible* ตามกฎที่ยืนยันแล้ว แล้วทดสอบ success + duplicate + cross-branch

**ข้อควรจำ:** รายการถอนที่เพิ่งสร้างอาจยังรับไม่ได้ เพราะสาม API ไม่ได้ครอบคลุม approval/dispatch เสมอไป. หากขั้นกลางไม่ได้ทำ SA ต้องบอกกรรมการตรง ๆ ว่ารายการสำหรับ receipt ถูก seed มาเพื่อสาธิต API นี้ ดู [คำอธิบาย flow](field-guide.md)

**ฉากจบเมื่อ:** รับได้หนึ่งครั้ง, ส่งซ้ำไม่เกิดผลการเงินซ้ำ, ผู้ใช้ผิดสาขาไม่ทำรายการได้

## ฉาก 6 — 14:30–16:00: พิสูจน์งาน

SA ใช้ `/cds-verify` เก็บ smoke/JMeter และข้อมูลเครื่อง/ภาระงานใน [performance](performance.md). BE รัน tests เรื่องเงิน, สิทธิ์, state, concurrent receipt และ rollback ตามที่ทำได้. FE ทดสอบ loading, error, session หมดอายุ, keyboard และจอแคบ. ทุกคนส่งหลักฐาน Copilot จริงให้ SA ลง [AI usage](ai-usage.md): prompt → ข้อเสนอ → สิ่งที่คนแก้ → ผลทดสอบ

**ฉากจบเมื่อ:** สาม API มีผลทดสอบที่อ้างได้, รู้กรณีที่ยังไม่ผ่าน, ไม่ใส่ตัวเลขประสิทธิภาพที่ยังไม่ได้วัด

## ฉาก 7 — 16:00–17:00: ซ้อมนำเสนอและตอบกรรมการ

SA ใช้ `/cds-modernization` ไม่เกิน 15 นาที ปรับ [แผนระบบเดิมไปใหม่](legacy-modernization.md) ให้ตรงของที่ทำ. จากนั้นใช้ `/cds-rehearse`; ให้ FE เป็นคนคลิกเดโม, BE อธิบายเงิน/transaction, SA เปิดเรื่องและตอบ architecture/AI evidence. ให้คนที่ไม่ได้ตั้งเครื่องเป็นคนทำ clean start ตาม README

**ลำดับเดโม:** login/branch → ขอเบิก → history → receipt จาก eligible fixture → กดยืนยันซ้ำ/ข้ามสาขาแล้วถูกปฏิเสธ → audit/test/JMeter → อะไรยังไม่ได้ทำ. ใช้ [แผนเก็บคะแนน](judging-playbook.md) ตรวจว่ามีหลักฐานครบ

**ฉากจบเมื่อ:** เดโมจบภายในเวลาที่ผู้จัดให้ และทีมพูดตรงกับ source/ผลทดสอบ

## ฉาก 8 — 17:00–17:50: หยุดเพิ่มงานและส่ง

SA ใช้ `/cds-submit` กับ [submission checklist](submission-checklist.md). BE ตรวจ revision และคำสั่งเปิด API; FE ตรวจลิงก์/หน้าจอ/ข้อมูลเดโม. SA ส่งตามช่องทางจริงและเปิดไฟล์ที่อัปโหลดตรวจอีกครั้ง. 17:50–18:00 เผื่อแก้ปัญหาการส่ง

**ฉากจบเมื่อ:** ไฟล์ที่ผู้จัดได้รับเปิดได้และมีหลักฐานเวลาส่ง. หลัง 18:00 ใช้ script ที่ซ้อมแล้ว

## ทริคที่คุ้มที่สุดในทีม 3 คน

- **SA ไม่ควรเป็นคนทำทุกอย่างเอง:** SA เป็นเจ้าของ contract/decision/การรวมหลักฐาน; BE ช่วย clean start และ backend tests, FE ช่วยคลิกเดโมและ UI evidence
- **หนึ่งตัวอย่างข้อมูลกลาง:** ใช้ branch, amount, reference และสถานะที่ตกลงกันชุดเดียวตลอด API/UI/tests/demo ด้วยข้อมูลสังเคราะห์
- **เปลี่ยน contract ผ่าน SA คนเดียว:** ป้องกัน BE/FE ทำ field คนละชื่อ; แจ้งผลกระทบใน [decisions](decisions.md)
- **คิดคะแนนจากหลักฐาน:** เก็บ Copilot prompt และ human correction ตอนงานจบ ไม่รอ 17:00; ทุกข้ออ้างเรื่อง performance/coverage ต้องมีผลจริง
- **ถ้าไม่ผ่าน checkpoint ให้ลดขอบเขต:** หยุด P1 และแก้ P0 ก่อน; ไม่มีเวลาให้ระบบเสริมที่เพิ่มจุดเสีย

ถ้าคุณติด flow ใดระหว่างซ้อม ให้เปิด [field guide](field-guide.md) หรือใช้ `/cds-explain-flow` ใน Copilot พร้อมระบุชื่อ flow เช่น “ทำไม withdrawal ใหม่ยัง receipt ไม่ได้?”
