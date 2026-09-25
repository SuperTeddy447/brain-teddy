# CDS Hackathon — คู่มือทีม 3 คน

ชุดเตรียมการแข่งขัน Cash Delivery System (CDS) สำหรับ Java, React/Node, Podman, JMeter และ GitHub Copilot เอกสารนี้เป็น **แผนและแม่แบบก่อนแข่ง**; ยังไม่มีแอปที่รันได้หรือผลทดสอบจริง

## วันแข่งเริ่มตรงนี้ — ทำตามแถวเวลา

ถ้าคุณเป็นคนคุมทีม **รับบท SA (ในเอกสารเดิมคือ C)**. อีกสองคนคือ **BE = A** และ **FE = B**. เปิด repo นี้บนเครื่อง/IDE ที่ผู้จัดอนุญาต แล้วให้ทั้งทีมเปิด README หน้าเดียวกัน. ทุกแถวมีสิ่งที่ต้องทำและเกณฑ์ก่อนย้ายไปแถวถัดไป; รายละเอียดย่อยอยู่ใน [cookbook](docs/cookbook.md). ถ้าอยากเห็นตัวอย่างการคุยและส่งงานจริง อ่าน [ฉากซ้อมวันแข่ง](docs/rehearsal-scenes.md)

| เวลาไทย | เปิดอะไรและทำอะไร | ผ่านเมื่อ |
|---|---|---|
| ก่อน sprint 60 นาที | ทั้งทีมทำ [ตารางเตรียม 60 นาที](docs/cookbook.md) และ [ตรวจ Copilot](docs/copilot-setup.md); แจก BE/FE/SA ตามตารางด้านล่าง | เครื่องมือพร้อมหรือ blocker มี owner |
| 08:30–10:00 | SA เปิด [brief](docs/brief.md), [requirements](docs/requirements.md), [decisions](docs/decisions.md), ใช้ `/cds-discovery` จดคำตอบผู้จัดและถาม Q&A | รู้กติกาจริง, 3 API, DB/AAD access, deadline และสิ่งที่ยังไม่รู้ |
| 10:00–10:30 | SA ใช้ `/cds-kickoff` ทำ contract; BE/FE ตรวจตัวอย่าง request/response และรับงาน | BE/FE ตกลง contract รุ่นเดียวกัน; blocker สำคัญมี owner |
| 10:30–12:00 | BE ใช้ `/cds-backend-slice`, FE ใช้ `/cds-frontend-slice`, SA ใช้ `/cds-integration` | withdrawal + history ต่อ UI → API → ข้อมูลที่เก็บไว้ได้ หรือชี้ blocker ได้ |
| 13:00–14:30 | BE ใช้ `/cds-receipt-hardening`, FE ทำ receipt UI, SA ตรวจสิทธิ์/fixture | receipt สำเร็จหนึ่งครั้ง และกดซ้ำหรือข้ามสาขาถูกปฏิเสธ |
| 14:30–16:00 | SA ใช้ `/cds-verify`; BE/FE แก้ defect; บันทึก [test matrix](docs/test-matrix.md) และ [ผล JMeter](docs/performance.md) | 3 API smoke ผ่าน; ผลทดสอบเป็นของที่รันจริง |
| 16:00–17:00 | SA ใช้ `/cds-modernization` แล้ว `/cds-rehearse`; BE/FE ช่วย clean start และซ้อม | คนอื่นเปิดระบบตามวิธีรันได้; เดโม 5–7 นาทีตรงกับระบบจริง |
| 17:00–17:50 | หยุดเพิ่ม feature; SA ใช้ `/cds-submit` และ [submission checklist](docs/submission-checklist.md) | ส่ง SharePoint แล้วเปิดไฟล์ที่ส่งตรวจสำเร็จ |
| หลัง 18:00 | ใช้ script ที่ซ้อมและ [คู่มือตอบกรรมการ](docs/field-guide.md) | อธิบายได้ว่าอะไรทำจริง อะไรเป็น fixture และอะไรเป็นแผนต่อยอด |

**ถ้าแถวใดยังไม่ผ่าน:** SA ระบุ blocker/owner ใน [decisions](docs/decisions.md), เลื่อนงานเสริม P1 ออก แล้วให้ทีมทำ P0 ที่ยังขาดก่อน. เกณฑ์ P0/P1/P2 อยู่ใน [requirements](docs/requirements.md). ถ้าพิมพ์ `/prompt` แล้วไม่ขึ้น ให้เปิดไฟล์ prompt ตามชื่อใน `.github/prompts/` และวางข้อความใน Copilot Agent mode ตาม [วิธีใช้ Copilot](docs/copilot-setup.md)

ระหว่างทำงาน ให้คนที่ทำงานบันทึก prompt, สิ่งที่คนแก้ และผลจริงใน [AI usage](docs/ai-usage.md). SA รวบรวมหลักฐานตาม [แผนเก็บคะแนน](docs/judging-playbook.md) ก่อนซ้อมเดโม

> กำหนดการที่ได้รับระบุส่งผ่าน SharePoint **ก่อน 18:00 น. เวลาไทย วันที่ 25 กันยายน 2026** ตั้งเป้าอัปโหลดเสร็จ 17:50 น. ยืนยันเวลาและรูปแบบไฟล์กับผู้จัดในวันแข่ง

## งานที่ต้องส่งให้เห็นจริง

สาม API คือ **Cash Withdrawal Request**, **Cash Withdrawal History Inquiry** และ **Cash Receipt Confirmation**. Demo หลัก: login → สาขาขอเบิก → ดูประวัติ → ยืนยันรับจากรายการที่มีสิทธิ์ → แสดงว่ากดซ้ำ/ข้ามสาขาถูกปฏิเสธ → เปิด audit และผลทดสอบ. หาก approval/dispatch ยังไม่มี ให้ระบุว่ารายการที่นำมายืนยันเป็น *seeded eligible fixture*.

ข้อเสนอเริ่มต้น: React UI + Java backend ชุดเดียวแบ่งโมดูล + ฐานข้อมูลที่ผู้จัดอนุมัติ. Node ใช้สำหรับ tooling ของ React. AAD/Entra ID ใช้ login และตรวจสิทธิ์ที่ API เมื่อได้รับ tenant/app/scope จริง. Podman ใช้รันชุดเดโม, JMeter ใช้เก็บผลโหลดจริง. ขอบเขตนี้ปรับตามโจทย์ที่ประกาศและ API standard ที่ผู้จัดให้

## แจกงาน BE / FE / SA และ Copilot

| คน | เจ้าของไฟล์ | Agent ที่เลือกใน Copilot | Skill ที่สั่งใช้เมื่อจำเป็น | Prompt เริ่มต้น |
|---|---|---|---|---|
| BE (A) | `backend/` | `cds-backend` | `cds-contract`, `cds-verification` | `/cds-backend-slice` |
| FE (B) | `frontend/` | `cds-frontend` | `cds-ui-review` | `/cds-frontend-slice` |
| SA (C) | `contracts/`, `infra/`, `tests/`, shared `docs/` | `cds-integration` | `cds-contract`, `cds-verification`, `cds-architecture-defense`, `cds-legacy-modernization` | `/cds-kickoff` |

Agent = บทบาทและขอบเขตงาน; skill = วิธีทำงานเฉพาะเรื่อง; prompt = งานหนึ่งช่วงเวลา. ตารางเรียกใช้เต็มวันและเกณฑ์ผ่านอยู่ใน [cookbook](docs/cookbook.md). รายละเอียดเปิดใช้และทดสอบ Copilot อยู่ใน [copilot-setup](docs/copilot-setup.md). ถ้า IDE ไม่แสดง custom agent หรือ slash prompt ให้เปิดไฟล์ prompt ตามลิงก์แล้ววางข้อความใน Copilot Agent mode พร้อมระบุ skill ที่ต้องการ

## Milestones

| เส้นตาย | หลักฐานที่ต้องมี |
|---|---|
| 10:30 | Contract 3 APIs และ blocker AAD/DB ที่มี owner |
| 12:00 | UI เรียก withdrawal และ history ผ่าน API รอบแรก หรือมี blocker ที่ชี้จุดได้ |
| 14:30 | Receipt, authorization, retry/concurrency strategy |
| 16:00 | Smoke/integration tests และ JMeter รอบเล็กพร้อมผลจริง |
| 17:00 | Clean start และ rehearsal ผ่าน |
| 17:50 | ส่งและเปิดไฟล์จาก SharePoint ตรวจแล้ว |

## เอกสารในชุดนี้

- [Cookbook รายเวลาและ prompt mapping](docs/cookbook.md)
- [ฉากซ้อมวันแข่ง: BE/FE/SA ทำอะไรและส่งต่ออย่างไร](docs/rehearsal-scenes.md)
- [ข้อกำหนด P0/P1/P2 และเกณฑ์ผ่าน](docs/requirements.md)
- [คู่มือเข้าใจ flow](docs/field-guide.md), [แผนยกระดับระบบเดิม](docs/legacy-modernization.md), [แผนเก็บคะแนนด้วยหลักฐาน](docs/judging-playbook.md)
- [วิธีใช้ GitHub Copilot agents, skills, prompts และ UI UX Pro Max](docs/copilot-setup.md)
- [Brief](docs/brief.md), [Decisions](docs/decisions.md), [Runbook เดิม](docs/runbook.md)
- [Test matrix](docs/test-matrix.md), [Performance](docs/performance.md), [AI usage](docs/ai-usage.md), [Submission checklist](docs/submission-checklist.md)

ห้ามกรอก coverage, throughput, ค่า p95 หรือเวลาที่ AI ช่วยประหยัดเป็นตัวเลขผลงาน หากยังไม่ได้วัดจริง. ใช้ข้อมูลสังเคราะห์ และเก็บ secrets/config ภายในช่องทางที่องค์กรอนุญาตเท่านั้น. เอกสารตั้งต้นทำด้วย ChatGPT ก่อนแข่ง; งาน GitHub Copilot ที่ทำในวันแข่งให้บันทึกแยกตามจริง

เอกสารอ้างอิง: [GitHub agent skills](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills), [Copilot customization](https://docs.github.com/en/copilot/reference/customization-cheat-sheet), [Copilot custom agents](https://docs.github.com/en/copilot/reference/custom-agents-configuration), [UI UX Pro Max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)
