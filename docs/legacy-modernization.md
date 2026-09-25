# ยกระดับ CDS เดิมแบบทำได้ในวันแข่ง

ข้อมูลเดิมที่ได้รับในข้อความโจทย์สรุป: ASP Classic/VB6, DB2 v11, Crystal Reports/on-prem; มี IAM/AD, SMTP, reporting, audit/SOC และ GL ที่เกี่ยวข้อง. **ต้องตรวจเวอร์ชัน, ownership และการเชื่อมจริงกับผู้จัด**. วันนี้เลือกแทนที่เพียงสามความสามารถผ่าน API ใหม่ และแสดงเส้นทางย้ายส่วนอื่นแบบมีลำดับ

## ภาพจากเดิมไปใหม่

| ด้าน | ระบบเดิม/โจทย์ที่ทราบ | สิ่งที่พิสูจน์ใน hackathon | ขอบเขตหลังแข่ง |
|---|---|---|---|
| หน้าจอ | ASP Classic/VB6 | React task flow สำหรับขอเบิก, history, receipt | ย้ายหน้าจออื่นทีละงาน; ทดสอบผู้ใช้สาขาจริง |
| ตัวตน | IAM/AD เดิม; ต้องไป AAD/Entra | Login จริงเมื่อได้ app registration; API ตรวจ token และ branch role | migration บัญชี, provisioning, offboarding, policy |
| Business API | logic เดิมอยู่หลายจุด | Java API 3 รายการ, contract และ tests | ทำ adapter/route ไป logic เดิมที่ยังไม่ย้าย |
| ข้อมูล | DB2; schema/access ยังต้องยืนยัน | ต่อ DB2 ที่อนุมัติ หรือ persistent fallback ที่ประกาศชัด | mapping schema, reconciliation, migration, cutover |
| Audit/report | Crystal Reports, audit/SOC | audit ต่อธุรกรรมและ reference ที่ตามรอยได้ | เชื่อม reporting/SOC ตาม interface ที่อนุมัติ |
| Integration | GL/SMTP/vendor | วาด boundary และระบุ owner; ไม่สร้างผลสำเร็จปลอม | contract, retry, reconciliation, operational ownership |

## หลักการย้ายทีละส่วน

1. **กำหนดเจ้าของข้อมูลต่อธุรกรรม**: รายการใหม่ 3 API เขียนลง datastore ที่ผู้จัดอนุมัติ. ห้ามให้ legacy กับระบบใหม่เขียนรายการเดียวกันโดยไม่มี single writer/reconciliation plan
2. **แยกขอบเขตด้วย contract**: API ใหม่เป็นจุดใช้งานที่มี version และ business semantics. UI ไม่คุยกับ DB2 ตรง และไม่อ้างว่า proxy/adapter มีอยู่หากยังไม่ได้สร้าง
3. **ตรวจ mapping ก่อนโยกข้อมูลจริง**: status, amount, branch ID, timezone, reference, audit และเลขรายการเก่าอาจไม่ตรง. เก็บ mapping เป็น `TBD` จนเห็น schema และผู้รับผิดชอบ
4. **เตรียม coexist/rollback**: อธิบายว่าจะสลับเฉพาะ flow ที่ทดสอบครบ และต้องมี routing, compare/reconcile, support และ rollback plan ก่อน production. ใน hackathon สิ่งนี้เป็นแผน ไม่ใช่ระบบที่ทำแล้ว
5. **แก้คอขวดจากหลักฐาน**: เริ่ม pagination/index/pool/transactions. เพิ่ม cache/queue/replica เมื่อ workload และ consistency model รองรับ; อย่า cache วงเงินหรือสถานะรับเงินโดยไม่แก้ความสดของข้อมูล

รูปแบบนี้เป็นการย้ายแบบค่อยเป็นค่อยไปตาม [AWS Strangler Fig guidance](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/strangler-fig.html) โดย **ไม่บังคับ** ว่าวันนี้ต้องมี proxy หรือ microservices. วันนี้ทำ modernization slice + migration map ที่ตรวจสอบได้

## สิ่งที่ทีมพูดกับกรรมการได้ใน 60 วินาที

> เราเลือกย้ายสามธุรกรรมที่โจทย์กำหนดก่อน โดยทำ contract, สิทธิ์สาขา, transaction, audit และผลทดสอบให้ตรวจได้. ระบบเดิมยังต้องอยู่สำหรับงานที่ไม่ได้ย้าย; เราแสดง boundary กับ DB2/AAD/GL/รายงานตามสถานะจริง. ก่อน production ต้องยืนยัน schema, data ownership, การ reconcile และ rollback. ผลโหลดวันนี้ใช้ตัดสินใจเรื่อง index/pool ก่อนเพิ่มระบบกระจาย

## Checklist ของ C ในวันแข่ง (รวมไม่เกิน 15 นาที)

- ช่วง briefing: ถามว่า 3 API ต้องอ่าน/เขียน DB2 เดิมโดยตรงหรือ datastore ใหม่, ใครเป็นเจ้าของ branch mapping และ state
- 16:00–16:15: เติมตารางข้างบนจาก implementation จริง; อัปเดต diagram ว่าอะไร `implemented`, `mocked`, `blocked`, `future`
- 16:15: ให้ A ตรวจ data/transaction claim, B ตรวจ UI claim แล้วใช้ข้อความ 60 วินาทีนี้ซ้อมตอบ
