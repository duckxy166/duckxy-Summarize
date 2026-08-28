# Duckxy Summarize

Skill สำหรับสรุปงานใน task ปัจจุบันแบบอิงหลักฐาน ว่ากำลังแก้ปัญหาอะไร ทำอะไรไปแล้ว มีอะไรเปลี่ยน และมีเรื่องใดที่ต้องรู้ก่อนทำงานต่อ

ใช้ได้กับทุก agent ที่อ่านสกิลรูปแบบ `SKILL.md` — Claude Code, Codex, Cursor, Antigravity, Gemini CLI และอื่น ๆ โดยไม่ผูกกับ host, model หรือชื่อ tool ใดเป็นการเฉพาะ

## ทำอะไรได้บ้าง

- สรุปเป้าหมายและสถานะของ task เป็น **เสร็จ**, **เสร็จบางส่วน** หรือ **ติดขัด** พร้อมเหตุผล
- แยกสิ่งที่ทำไปแล้วออกจากสิ่งที่ยังค้าง, ล้มเหลว, ถูกย้อนกลับ หรือยังไม่ได้ตรวจสอบ
- อธิบายการเปลี่ยนแปลงที่กระทบพฤติกรรม, ไฟล์สำคัญ, API, schema, configuration หรือ dependency เมื่อเกี่ยวข้อง
- รายงานผล test, build, lint และการตรวจสอบอื่น ๆ ที่รันจริงเท่านั้น
- บอกผลกระทบ, breaking changes, ขั้นตอนที่ต้องทำเอง, ความเสี่ยง และงานที่เหลือ
- แยก dirty changes ที่ไม่เกี่ยวข้องออกจากงานปัจจุบันเมื่อหลักฐานไม่เพียงพอ

## วิธีใช้

สกิลนี้เป็น explicit-only จึงต้องเรียกด้วยชื่อโดยตรง รูปแบบการเรียกขึ้นกับ agent ที่ใช้:

```text
/duckxy-summarize     # Claude Code, Cursor, Antigravity
$duckxy-summarize     # Codex
```

ระบุจุดเน้นเพิ่มเติมได้ เช่น:

```text
/duckxy-summarize เน้นผลกระทบต่อ API
```

## แหล่งข้อมูลที่ใช้

สกิลอ่านบริบทของ task ปัจจุบัน เช่น บทสนทนา, ผลจาก tools, เกณฑ์ความสำเร็จ, รายละเอียด issue ที่มีใน task และเมื่อมี repository ก็จะดู `git status`, diff, commit และไฟล์ที่เกี่ยวข้องแบบ read-only

หากไม่มีหลักฐานจาก repository, issue หรือผลทดสอบ สกิลจะสรุปจากบริบทที่มีและระบุช่องว่างของข้อมูลอย่างชัดเจน

## รูปแบบผลลัพธ์

ผลสรุปจะกระชับและใช้ภาษาของผู้ใช้ (ภาษาไทยเป็นค่าเริ่มต้น) โดยครอบคลุมหัวข้อ:

1. สถานะ
2. งานนี้ทำอะไร
3. ทำอะไรไปแล้ว
4. เปลี่ยนอะไรไปบ้าง
5. ตรวจสอบอะไรแล้ว
6. ฉันต้องรู้อะไรบ้าง

## ข้อจำกัด

`duckxy-summarize` เป็นสกิลสำหรับรายงานผลเท่านั้น จึงไม่แก้ไฟล์, สร้าง commit หรือ branch, หรือโพสต์/แก้ไข issue tracker เอง และจะไม่กล่าวอ้างว่างานหรือ test ผ่านหากไม่มีหลักฐานรองรับ

## ติดตั้ง

clone repository นี้ลงในโฟลเดอร์ skills ของ agent ที่ต้องการใช้:

| Agent | โฟลเดอร์ skills |
|---|---|
| Claude Code | `~/.claude/skills/duckxy-summarize` |
| Codex | `~/.codex/skills/duckxy-summarize` |
| Cursor | `~/.cursor/skills/duckxy-summarize` |
| Antigravity | `~/.gemini/antigravity/skills/duckxy-summarize` |
| Gemini CLI | `~/.gemini/skills/duckxy-summarize` |

```bash
git clone https://github.com/duckxy166/duckxy-summarize.git ~/.claude/skills/duckxy-summarize
```

### ใช้ร่วมกันหลาย agent (แนะนำ)

clone ไว้ที่เดียวแล้วทำ link ไปยังโฟลเดอร์ skills ของแต่ละ agent — pull ครั้งเดียวได้ครบทุกตัว

```bash
git clone https://github.com/duckxy166/duckxy-summarize.git ~/.agents/repos/duckxy-summarize
ln -s ~/.agents/repos/duckxy-summarize ~/.claude/skills/duckxy-summarize   # macOS / Linux
```

Windows (PowerShell, ไม่ต้องใช้สิทธิ์ admin):

```powershell
New-Item -ItemType Junction -Path "$env:USERPROFILE\.claude\skills\duckxy-summarize" -Target "$env:USERPROFILE\.agents\repos\duckxy-summarize"
```

จากนั้นเปิด task ใหม่ใน agent แล้วเรียกใช้สกิลตามรูปแบบของ agent นั้น
