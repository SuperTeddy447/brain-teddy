# เปิดใช้ GitHub Copilot กับชุดเตรียม CDS

เอกสารนี้ใช้กับ **GitHub Copilot Agent mode ใน VS Code** เป็นหลัก. IDE/นโยบายองค์กรอาจแสดงชื่อเมนูไม่เหมือนกัน; ทดสอบบนเครื่องแข่งขันจริงก่อนเริ่มงาน. ถ้าคุณ clone repo นี้มาแล้ว ไฟล์ `.github/` อยู่ใน repo **ไม่ต้องสั่ง agent คัดลอก skill ไปวางอีกครั้ง**

## 4 ชั้นที่ใช้ร่วมกัน

| ชั้น | ไฟล์ | ความหมาย | คุณทำอะไรใน VS Code |
|---|---|---|---|
| คำสั่งกลาง | `.github/copilot-instructions.md` | กติกาที่ใช้ทุกงานใน repo | เปิด root ของ repo; ดู References ในคำตอบว่าอ่านไฟล์นี้ |
| Agent | `.github/agents/cds-backend.agent.md` ฯลฯ | บทบาท/ขอบเขตไฟล์ของ BE, FE, SA | เลือก agent ที่ตรงกับคนจากตัวเลือกของ Copilot Chat |
| Skill | `.github/skills/<name>/SKILL.md` | วิธีตรวจหรือทำเรื่องเฉพาะ เช่น contract/security | Copilot อาจเลือกเอง; ในงานสำคัญพิมพ์ `Use cds-api-security-review; read its SKILL.md` และตรวจว่าอ่านจริง |
| Prompt file | `.github/prompts/<name>.prompt.md` | ใบสั่งงานหนึ่งช่วงเวลา | เลือก prompt จาก Attach context → Prompt หรือพิมพ์ `/ชื่อ` **ถ้า IDE แสดง** แล้วเติมข้อมูลโจทย์จริง |

**จำง่าย:** agent = ใครทำ, skill = ทำอย่างไร, prompt = ทำงานชิ้นไหนตอนนี้, instructions = กฎร่วม. `/cds-kickoff` เป็นชื่อ prompt file ในชุดนี้; การพิมพ์ `Use cds-contract` คือสั่งให้อ่าน skill. ชื่อที่ขึ้นหลัง `/` ต่างกันตาม Copilot surface และรุ่น; ให้ดู prompt picker ใน IDE จริง

ชื่อ agent กับ skill ต่างกัน: **BE = A, FE = B, SA = C** ใน cookbook. ให้แต่ละคนเลือก agent ของตัวเองแล้วใช้ prompt ตามเวลา. Skill ถูกเลือกจากงานที่ทำหรือระบุชื่อใน prompt. ไม่ควรเริ่มสาม agent ให้แก้ไฟล์เดียวกันพร้อมกัน

## เช็กให้ใช้งานได้ใน 5 นาที

1. เปิด **root ของ repo นี้** ใน VS Code ที่ลงชื่อเข้าใช้ Copilot ของบริษัท แล้วเปิด Copilot Chat → Agent mode
2. ถาม `Summarize the repository instructions and list the CDS skills you can access. Do not edit files.` ตรวจว่าคำตอบอ้าง `.github/copilot-instructions.md` และพบ 6 skills. ถ้าไม่พบ ให้เปิดไฟล์ที่เกี่ยวข้องใน editor แล้วสั่งอ่าน path โดยตรง
3. ดูตัวเลือก custom agent ว่ามี `cds-backend`, `cds-frontend`, `cds-integration`. ถ้าไม่มี ให้ใช้ Agent mode ปกติและวางเนื้อหา agent profile ตามบทบาทก่อน prompt งาน
4. เปิด Attach context → Prompt... แล้วหา `cds-kickoff`; ลองพิมพ์ `/` ดูว่าชื่อ prompt ขึ้นไหม. หากไม่ขึ้น ให้ตรวจ setting `chat.promptFiles` ของ workspace ตาม [GitHub Docs](https://docs.github.com/en/copilot/how-tos/configure-custom-instructions-in-your-ide/add-repository-instructions-in-your-ide). ถ้า IDE ยังไม่รองรับ ให้เปิด `.github/prompts/cds-kickoff.prompt.md` แล้ววางเนื้อหาใน chat
5. ให้ SA ทดสอบแบบอ่านอย่างเดียวด้วยข้อความ `Read docs/brief.md and docs/decisions.md. List the five blocking unknowns with owners. Do not edit files.` ตรวจว่า Copilot ไม่แต่งข้อมูล AAD/DB2 แล้วค่อยใช้ `cds-kickoff` เมื่อถึง contract gate

Skill `cds-legacy-modernization` ใช้โดย SA ตอนฟังข้อมูลระบบเดิมและช่วง 16:00–16:15. Skill `cds-api-security-review` ใช้ร่วมกับ BE ตอนตรวจ OWASP. Prompt `cds-explain-flow` ใช้ถามเรื่อง flow เป็นภาษาไทยแบบอ่านอย่างเดียว. อ่าน [requirements](requirements.md), [field guide](field-guide.md) และ [security defense](security-defense.md) ประกอบ

## วิธีสั่งงานหนึ่งรอบแบบจับมือทำ

1. SA เปิด [cookbook](cookbook.md) ดูเวลาปัจจุบันและเลือกแถวงาน. ตัวอย่าง 10:00 คือ contract gate
2. เลือก agent `cds-integration` ใน Copilot Chat → Agent mode แล้วเลือก prompt `cds-kickoff` จาก Prompt picker. แนบ [brief](brief.md) กับ [decisions](decisions.md) ที่แก้ตามผู้จัดแล้ว
3. เติมข้อความท้าย prompt: `Confirmed by organizer: [กฎจริง]. Pending: [สิ่งที่ยังไม่รู้]. Use cds-contract. Return files changed, tests actually run, and blockers.`
4. ให้ Copilot เสนอ/แก้ไฟล์ → SA ตรวจ diff กับโจทย์จริง → BE/FE ตรวจ contract ที่จะใช้ → SA บันทึกข้อสรุป. คำตอบ “done” ของ agent ต้องมีหลักฐานก่อนถือว่างานผ่าน
5. เมื่อถึงแถวถัดไป BE/FE เลือก agent ของตัวเองและ prompt ของช่วงนั้น. จบงานให้ส่งผลทดสอบและข้อแก้จาก AI ไป [AI usage](ai-usage.md)

ถ้า skill ไม่ถูกอ่าน ให้สั่งเจาะจง: `Read .github/skills/cds-contract/SKILL.md and apply it to the current OpenAPI task.` ถ้า custom agent ไม่ขึ้น ให้ใช้ Agent mode ปกติและเปิด `.github/agents/cds-integration.agent.md` เป็นบริบท. ถ้า prompt picker ไม่ขึ้น ให้วางเนื้อหา `.github/prompts/cds-kickoff.prompt.md` เอง

## ใช้ prompt ไหนเมื่อไร

ใช้ `.github/prompts/*.prompt.md` เป็นชุดหลักสำหรับ IDE. ตารางนี้เป็นทางลัด; ผลที่ต้องผ่านแต่ละช่วงอยู่ใน [cookbook](cookbook.md)

| เวลา/เหตุการณ์ | คน / agent | Prompt หลัก | Skill ที่ควรยืนยันว่าอ่าน | เอกสารที่เปิดก่อน |
|---|---|---|---|---|
| Briefing/Q&A | SA / `cds-integration` | `cds-discovery` | `cds-contract`, `cds-legacy-modernization` | `brief.md`, `requirements.md`, `decisions.md` |
| 10:00 contract | SA / `cds-integration` | `cds-kickoff` | `cds-contract`, `cds-architecture-defense` | `brief.md`, `decisions.md` |
| 10:30 slice แรก | BE / `cds-backend` | `cds-backend-slice` | `cds-contract` | OpenAPI ที่ SA ยืนยัน |
| 10:30 slice แรก | FE / `cds-frontend` | `cds-frontend-slice` | `cds-ui-review` | OpenAPI ที่ SA ยืนยัน |
| 10:30 integration | SA / `cds-integration` | `cds-integration` | `cds-verification` | `decisions.md`, OpenAPI |
| 11:30 checkpoint | SA / `cds-integration` | `cds-verify` | `cds-verification` | `test-matrix.md` |
| 13:00 receipt | BE / `cds-backend` | `cds-receipt-hardening` | `cds-contract`, `cds-verification` | OpenAPI, `decisions.md` |
| 13:00 receipt UI | FE / `cds-frontend` | `cds-frontend-slice` + ระบุ `receipt` | `cds-ui-review` | OpenAPI, `field-guide.md` |
| 14:30 security | SA+BE / `cds-integration` | `cds-security-review` | `cds-api-security-review` | `security-defense.md`, `test-matrix.md` |
| 14:30 verification/JMeter | SA / `cds-integration` | `cds-verify` | `cds-verification` | `test-matrix.md`, `performance.md` |
| 16:00 migration/Q&A | SA / `cds-integration` | `cds-modernization` | `cds-legacy-modernization` | `legacy-modernization.md` |
| 16:15 rehearsal | SA / `cds-integration` | `cds-rehearse` | `cds-architecture-defense` | `field-guide.md`, `security-defense.md` |
| 17:00 submission | SA / `cds-integration` | `cds-submit` | `cds-verification` | `submission-checklist.md` |
| เมื่อไม่เข้าใจ flow | ใครก็ได้ / Agent mode | `cds-explain-flow` | skill ที่เกี่ยวข้อง | `field-guide.md`; ไม่แก้โค้ด |

### แล้ว `docs/prompts/00–03` ใช้เมื่อไร?

ไฟล์ใน `docs/prompts/` เป็น **prompt ข้อความรุ่นแรกสำหรับเปิดอ่านหรือคัดลอกเอง**. มันไม่ใช่ `.prompt.md` ใน `.github/prompts/` จึงไม่ต้องคาดว่าจะขึ้นเป็น slash prompt. ใช้ชุด `.github/prompts/` เป็นหลัก; หาก IDE ไม่อ่าน prompt file เลย ให้คัดลอกข้อความจากไฟล์ที่ตรงงานหนึ่งชุดเท่านั้น

| ไฟล์เก่า | งานเดียวกันในชุดที่ใช้ปัจจุบัน |
|---|---|
| `docs/prompts/00-kickoff.md` | `.github/prompts/cds-kickoff.prompt.md` |
| `docs/prompts/01-backend.md` | `cds-backend-slice` แล้ว `cds-receipt-hardening` |
| `docs/prompts/02-frontend.md` | `cds-frontend-slice` |
| `docs/prompts/03-integration.md` | `cds-integration` แล้ว `cds-verify` |

ไม่ต้องส่ง prompt เก่าและใหม่ซ้ำใน task เดียว. หาก briefing เปลี่ยนโจทย์ ให้ยึด `brief.md`/`decisions.md` รุ่นล่าสุดแทนข้อความเตรียมล่วงหน้า

GitHub ระบุว่า project skills อยู่ใน `.github/skills`, custom agents ใน `.github/agents`, และ prompt files ใน `.github/prompts`. Prompt files ยังมีข้อจำกัดตาม IDE. ดู [skills](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills), [custom agents](https://docs.github.com/en/copilot/reference/custom-agents-configuration), [prompt files](https://docs.github.com/en/copilot/how-tos/configure-custom-instructions-in-your-ide/add-repository-instructions-in-your-ide)

## เลือกโมเดลและคุมงบ

ตรวจ **โมเดลที่บัญชีองค์กรอนุญาตและหน้า usage/billing จริง** ของแต่ละคนก่อนแข่ง. ใช้โมเดลที่ถนัดกับงานทั่วไป; เปลี่ยนไปโมเดล reasoning ที่แรงกว่าเฉพาะตรวจ state/concurrency/security หรือแก้ blocker ซับซ้อน. เก็บวงเงินส่วนหนึ่งไว้ช่วง 16:00–18:00 และดู usage ที่ 12:00/16:00. ไม่ฝังชื่อโมเดลหรือราคาใน agent profiles เพราะรายการและการคิดค่าบริการเปลี่ยนได้. ดู [แผน Copilot](https://docs.github.com/en/copilot/get-started/plans) และ [billing](https://docs.github.com/en/copilot/reference/copilot-billing)

## UI UX Pro Max — ตัวเลือกของ FE

ใช้เมื่อผู้จัดและองค์กรอนุญาต third-party assets และมีเวลา setup สูงสุด 10 นาที. ตรวจ [repo ต้นทาง](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill), license, เวอร์ชันและไฟล์ที่จะเพิ่มก่อนติดตั้ง. จาก root ของ repo ทดลองแบบ dry run ตาม CLI รุ่นที่เลือก; **อย่าใช้ `--force` กับ repo ของทีม**:

```sh
npx ui-ux-pro-max-cli@<approved-version> init --ai copilot --dry-run
npx ui-ux-pro-max-cli@<approved-version> init --ai copilot
```

แทน `<approved-version>` ด้วยเวอร์ชันที่ทีมตรวจแล้ว และตรวจผลที่ CLI สร้างจริง (บางรุ่นเป็น prompt/slash command ใน `.github/prompts/`). เปิดเมนู `/` ดูวิธีเรียกตามรุ่นที่ติดตั้ง. ให้ใช้กับแบบฟอร์มถอนเงิน, ตารางประวัติ, receipt review และ error states; ตรวจ diff ก่อนรับงาน. ถ้าไม่พร้อมใน 10 นาที ให้ FE ใช้ `cds-ui-review` ที่อยู่ใน repo นี้

## บันทึกการใช้ AI

หลังจบงานแต่ละช่วง ให้ผู้ทำบันทึก prompt ที่ใช้, ไฟล์ที่ AI เปลี่ยน, ส่วนที่คนแก้, คำสั่งทดสอบและผลจริงใน `docs/ai-usage.md`. ข้อมูลเตรียมก่อนแข่งจาก ChatGPT ต้องไม่ถูกรวมเป็นงานที่ Copilot ทำในวันแข่ง
