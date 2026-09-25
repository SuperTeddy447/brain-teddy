# แผนเก็บคะแนนด้วยหลักฐาน

สัดส่วนด้านล่างมาจาก **สรุปโจทย์ที่ผู้ใช้แนบ** ยังไม่ใช่ scoring sheet ที่ตรวจจากผู้จัด. C ยืนยันอีกครั้งช่วง 09:45–10:00 และแก้แผนหากต่างไป. เป้าคือแสดงผลงานที่พิสูจน์ได้ ไม่รับประกันผลการแข่งขัน

| หมวดในสรุป | สัดส่วนที่ระบุ | หลักฐานที่ทีมต้องชี้ให้เห็น | เจ้าของ |
|---|---:|---|---|
| AI Engineering Approach | 30% | prompt ที่ใช้จริง 2–3 ตัว, diff ที่ Copilot เสนอ, จุดที่คนแก้, tests หลังแก้ใน `ai-usage.md` | C รวบรวม; A/B ส่งตัวอย่าง |
| Engineering Excellence | 30% | OpenAPI ตรง code, modules อ่านง่าย, money/state/auth/audit decisions, diagram ตรง implementation | A/C |
| Testing & Reliability | 15% | test matrix success/negative/concurrency, ผล coverage ถ้าวัดได้, JMeter environment/latency/errors | A/C |
| Code Quality & Review | 15% | reviewed diff/commit, defect ที่พบและแก้, ไม่มี secrets, README รันซ้ำได้ | ทุกคน |
| Presentation & Technical Defense | 10% | demo 3 APIs จริง, one rejected operation, modernization map, Q&A ตรงกับผลทดสอบ | C นำ, A/B ตอบ |

**ผลตอบแทนต่อเวลา:** การทำ API ที่ถูกต้องและพิสูจน์ได้ช่วยหลายหมวดพร้อมกัน. 10 นาทีที่ใช้ทำ test + human review + บันทึก Copilot evidence มักคุ้มกว่า 10 นาทีทำจอใหม่ที่ไม่อยู่ในโจทย์. อย่าแต่งตัวเลข coverage/performance/AI time saved

## ชุดหลักฐานขั้นต่ำที่ C รวบรวมก่อน 17:00

1. **หนึ่ง contract**: OpenAPI 3 operations + examples + decisions ที่อ้างคำตอบผู้จัด
2. **หนึ่ง trace**: UI reference → API response → persisted record → audit event จาก synthetic data
3. **หนึ่ง failure**: cross-branch หรือ duplicate receipt ถูกปฏิเสธและ state ไม่เปลี่ยน
4. **หนึ่ง AI story**: prompt → ข้อเสนอ Copilot → human correction → test ก่อน/หลัง หรือ defect ที่เจอจริง
5. **หนึ่ง load story**: JMeter command/workload/environment + p95/error/ข้อจำกัด ถ้ารันได้
6. **หนึ่ง modernization story**: ของเดิมคืออะไร, อะไรย้ายแล้ว, อะไรยังไม่ย้าย และขั้นต่อไป

## Demo ที่ควรเล่า

ปัญหาระบบเดิม → ทำไมเลือก 3 flow → เดโมรายการจริง/กรณีปฏิเสธ → contract และ audit → AI evidence → tests/load → migration map/ข้อจำกัด. ซ้อมให้จบในเวลาที่ผู้จัดให้. หากเวลาเดโมสั้น ให้ย่อคำพูดก่อนลดหลักฐานจากระบบจริง
