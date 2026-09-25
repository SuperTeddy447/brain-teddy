# เปิดใช้ GitHub Copilot กับชุดเตรียม CDS

เอกสารนี้ใช้กับ **GitHub Copilot Agent mode ใน VS Code** เป็นหลัก. IDE/นโยบายองค์กรอาจแสดงชื่อเมนูไม่เหมือนกัน; ทดสอบบนเครื่องแข่งขันจริงก่อนเริ่มงาน

## ของที่ repo เตรียมไว้

| ชนิด | ตำแหน่ง | มีหน้าที่ | วิธีเรียก |
|---|---|---|---|
| คำสั่งกลาง | `.github/copilot-instructions.md` | กติกาที่ใช้ทุกงาน | เปิด repo แล้วตรวจ References ในคำตอบ Copilot |
| Skill | `.github/skills/<name>/SKILL.md` | ขั้นตอนเฉพาะ domain | ขอใน prompt ว่า `Use cds-contract` ฯลฯ; ตรวจว่า agent อ่านจริง |
| Agent | `.github/agents/*.agent.md` | บทบาท A/B/C | เลือกชื่อ agent จากตัวเลือกของ Copilot Chat |
| Prompt file | `.github/prompts/*.prompt.md` | งานหนึ่ง milestone | ใน Chat พิมพ์ `/ชื่อไฟล์` เช่น `/cds-kickoff` |

ชื่อ agent กับ skill ต่างกัน: ให้คน A/B/C **เลือก agent ของตัวเอง** แล้วใช้ prompt ตามเวลา. Skill ถูกเลือกจากงานที่ทำหรือระบุชื่อใน prompt. ไม่ควรเริ่มสาม agent ให้แก้ไฟล์เดียวกันพร้อมกัน

## เช็กให้ใช้งานได้ใน 5 นาที

1. เปิด **root ของ repo นี้** ใน VS Code ที่ลงชื่อเข้าใช้ Copilot ของบริษัท แล้วเปิด Copilot Chat → Agent mode
2. ถาม `Summarize the repository instructions and list the CDS skills you can access. Do not edit files.` ตรวจว่าคำตอบอ้าง `.github/copilot-instructions.md` และพบ 5 skills. ถ้าไม่พบ ให้เปิดไฟล์ที่เกี่ยวข้องใน editor แล้วสั่งอ่าน path โดยตรง
3. ดูตัวเลือก custom agent ว่ามี `cds-backend`, `cds-frontend`, `cds-integration`. ถ้าไม่มี ให้ใช้ Agent mode ปกติและวางเนื้อหา agent profile ตามบทบาทก่อน prompt งาน
4. พิมพ์ `/` แล้วตรวจว่ามี prompt เช่น `/cds-kickoff`. ถ้า IDE ไม่รองรับ prompt files ให้เปิด `.github/prompts/cds-kickoff.prompt.md` แล้ววางเนื้อหาใน chat
5. ให้ C ทดสอบแบบอ่านอย่างเดียวด้วยข้อความ `Read docs/brief.md and docs/decisions.md. List the five blocking unknowns with owners. Do not edit files.` ตรวจว่า Copilot ไม่แต่งข้อมูล AAD/DB2 แล้วค่อยใช้ `/cds-kickoff` เมื่อถึง contract gate

Skill ที่เพิ่ม: `cds-legacy-modernization` ใช้โดย C ตอนฟังข้อมูลระบบเดิมและช่วง 16:00–16:15 เพื่ออัปเดต migration map. Prompt `/cds-modernization` ทำเอกสารนี้; `/cds-explain-flow` ใช้ถามเรื่อง flow เป็นภาษาไทยแบบอ่านอย่างเดียว. อ่าน [requirements](requirements.md) และ [field guide](field-guide.md) ประกอบ

GitHub ระบุว่า project skills อยู่ใน `.github/skills`, custom agents ใน `.github/agents`, และ prompt files ใน `.github/prompts`. Prompt files ยังมีข้อจำกัดตาม IDE. ดู [skills](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills), [custom agents](https://docs.github.com/en/copilot/reference/custom-agents-configuration), [prompt files](https://docs.github.com/en/copilot/how-tos/configure-custom-instructions-in-your-ide/add-repository-instructions-in-your-ide)

## เลือกโมเดลและคุมงบ

ตรวจ **โมเดลที่บัญชีองค์กรอนุญาตและหน้า usage/billing จริง** ของแต่ละคนก่อนแข่ง. ใช้โมเดลที่ถนัดกับงานทั่วไป; เปลี่ยนไปโมเดล reasoning ที่แรงกว่าเฉพาะตรวจ state/concurrency/security หรือแก้ blocker ซับซ้อน. เก็บวงเงินส่วนหนึ่งไว้ช่วง 16:00–18:00 และดู usage ที่ 12:00/16:00. ไม่ฝังชื่อโมเดลหรือราคาใน agent profiles เพราะรายการและการคิดค่าบริการเปลี่ยนได้. ดู [แผน Copilot](https://docs.github.com/en/copilot/get-started/plans) และ [billing](https://docs.github.com/en/copilot/reference/copilot-billing)

## UI UX Pro Max — ตัวเลือกของ B

ใช้เมื่อผู้จัดและองค์กรอนุญาต third-party assets และมีเวลา setup สูงสุด 10 นาที. ตรวจ [repo ต้นทาง](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill), license, เวอร์ชันและไฟล์ที่จะเพิ่มก่อนติดตั้ง. จาก root ของ repo ทดลองแบบ dry run ตาม CLI รุ่นที่เลือก; **อย่าใช้ `--force` กับ repo ของทีม**:

```sh
npx ui-ux-pro-max-cli@<approved-version> init --ai copilot --dry-run
npx ui-ux-pro-max-cli@<approved-version> init --ai copilot
```

แทน `<approved-version>` ด้วยเวอร์ชันที่ทีมตรวจแล้ว และตรวจผลที่ CLI สร้างจริง (บางรุ่นเป็น prompt/slash command ใน `.github/prompts/`). เปิดเมนู `/` ดูวิธีเรียกตามรุ่นที่ติดตั้ง. ให้ใช้กับแบบฟอร์มถอนเงิน, ตารางประวัติ, receipt review และ error states; ตรวจ diff ก่อนรับงาน. ถ้าไม่พร้อมใน 10 นาที ให้ B ใช้ `cds-ui-review` ที่อยู่ใน repo นี้

## บันทึกการใช้ AI

หลังจบงานแต่ละช่วง ให้ผู้ทำบันทึก prompt ที่ใช้, ไฟล์ที่ AI เปลี่ยน, ส่วนที่คนแก้, คำสั่งทดสอบและผลจริงใน `docs/ai-usage.md`. ข้อมูลเตรียมก่อนแข่งจาก ChatGPT ต้องไม่ถูกรวมเป็นงานที่ Copilot ทำในวันแข่ง
