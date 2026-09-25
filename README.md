# CDS Hackathon — คู่มือทีม 3 คน

ชุดเตรียมการแข่งขัน Cash Delivery System (CDS) สำหรับ Java, React/Node, Podman, JMeter และ GitHub Copilot เอกสารนี้เป็น **แผนและแม่แบบก่อนแข่ง**; ยังไม่มีแอปที่รันได้หรือผลทดสอบจริง

## เปิดใช้งานใน 3 นาที

1. เปิด repo นี้บน **เครื่องและ IDE ที่ผู้จัดอนุญาต** แล้วอ่าน [โจทย์และรายการที่ต้องยืนยัน](docs/brief.md)
2. เปิด [cookbook รายเวลา](docs/cookbook.md): เตรียม 60 นาที, แข่งตั้งแต่ 08:30 จนส่งก่อน 18:00, และ demo หลังส่ง
3. ให้ทั้งสามคนเปิด [คู่มือ GitHub Copilot](docs/copilot-setup.md) เพื่อตรวจ custom instructions, skills, agents และ prompt files ใน IDE จริง
4. C ยืนยัน [ข้อกำหนด P0/P1](docs/requirements.md) และ API contract ก่อน A/B ลงมือพร้อมกัน; บันทึกคำตอบผู้จัดใน [decisions](docs/decisions.md)
5. เมื่อทำงานจริง บันทึกหลักฐานใน [test matrix](docs/test-matrix.md), [performance](docs/performance.md) และ [AI usage](docs/ai-usage.md)

> กำหนดการที่ได้รับระบุส่งผ่าน SharePoint **ก่อน 18:00 น. เวลาไทย วันที่ 25 กันยายน 2026** ตั้งเป้าอัปโหลดเสร็จ 17:50 น. ยืนยันเวลาและรูปแบบไฟล์กับผู้จัดในวันแข่ง

## งานที่ต้องส่งให้เห็นจริง

สาม API คือ **Cash Withdrawal Request**, **Cash Withdrawal History Inquiry** และ **Cash Receipt Confirmation**. Demo หลัก: login → สาขาขอเบิก → ดูประวัติ → ยืนยันรับจากรายการที่มีสิทธิ์ → แสดงว่ากดซ้ำ/ข้ามสาขาถูกปฏิเสธ → เปิด audit และผลทดสอบ. หาก approval/dispatch ยังไม่มี ให้ระบุว่ารายการที่นำมายืนยันเป็น *seeded eligible fixture*.

ข้อเสนอเริ่มต้น: React UI + Java backend ชุดเดียวแบ่งโมดูล + ฐานข้อมูลที่ผู้จัดอนุมัติ. Node ใช้สำหรับ tooling ของ React. AAD/Entra ID ใช้ login และตรวจสิทธิ์ที่ API เมื่อได้รับ tenant/app/scope จริง. Podman ใช้รันชุดเดโม, JMeter ใช้เก็บผลโหลดจริง. ขอบเขตนี้ปรับตามโจทย์ที่ประกาศและ API standard ที่ผู้จัดให้

## แจกงานและ Copilot

| คน | เจ้าของไฟล์ | Agent ที่เลือกใน Copilot | Skill ที่สั่งใช้เมื่อจำเป็น | Prompt เริ่มต้น |
|---|---|---|---|---|
| A — Backend | `backend/` | `cds-backend` | `cds-contract`, `cds-verification` | `/cds-backend-slice` |
| B — Frontend | `frontend/` | `cds-frontend` | `cds-ui-review` | `/cds-frontend-slice` |
| C — Lead/Integration | `contracts/`, `infra/`, `tests/`, shared `docs/` | `cds-integration` | `cds-contract`, `cds-verification`, `cds-architecture-defense` | `/cds-kickoff` |

Agent = บทบาทและขอบเขตงาน; skill = วิธีทำงานเฉพาะเรื่อง; prompt = งานหนึ่งช่วงเวลา. C ใช้ `cds-legacy-modernization` เพิ่มในช่วง briefing/defense. ตารางเรียกใช้เต็มวันและเกณฑ์ผ่านอยู่ใน [cookbook](docs/cookbook.md). รายละเอียดเปิดใช้และทดสอบ Copilot อยู่ใน [copilot-setup](docs/copilot-setup.md). ถ้า IDE ไม่แสดง custom agent หรือ slash prompt ให้เปิดไฟล์ prompt ตามลิงก์แล้ววางข้อความใน Copilot Agent mode พร้อมระบุ skill ที่ต้องการ

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
- [ข้อกำหนด P0/P1/P2 และเกณฑ์ผ่าน](docs/requirements.md)
- [คู่มือเข้าใจ flow](docs/field-guide.md), [แผนยกระดับระบบเดิม](docs/legacy-modernization.md), [แผนเก็บคะแนนด้วยหลักฐาน](docs/judging-playbook.md)
- [วิธีใช้ GitHub Copilot agents, skills, prompts และ UI UX Pro Max](docs/copilot-setup.md)
- [Brief](docs/brief.md), [Decisions](docs/decisions.md), [Runbook เดิม](docs/runbook.md)
- [Test matrix](docs/test-matrix.md), [Performance](docs/performance.md), [AI usage](docs/ai-usage.md), [Submission checklist](docs/submission-checklist.md)

ห้ามกรอก coverage, throughput, ค่า p95 หรือเวลาที่ AI ช่วยประหยัดเป็นตัวเลขผลงาน หากยังไม่ได้วัดจริง. ใช้ข้อมูลสังเคราะห์ และเก็บ secrets/config ภายในช่องทางที่องค์กรอนุญาตเท่านั้น. เอกสารตั้งต้นทำด้วย ChatGPT ก่อนแข่ง; งาน GitHub Copilot ที่ทำในวันแข่งให้บันทึกแยกตามจริง

เอกสารอ้างอิง: [GitHub agent skills](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills), [Copilot customization](https://docs.github.com/en/copilot/reference/customization-cheat-sheet), [Copilot custom agents](https://docs.github.com/en/copilot/reference/custom-agents-configuration), [UI UX Pro Max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)
