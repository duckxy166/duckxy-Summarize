# Duckxy Summarize

Skill สำหรับสรุปงานใน task ปัจจุบันแบบอิงหลักฐาน — กำลังแก้ปัญหาอะไร ทำอะไรไปแล้ว มีอะไรเปลี่ยน และมีเรื่องใดที่ต้องรู้ก่อนทำงานต่อ

ใช้ได้กับทุก agent ที่อ่านสกิลรูปแบบ `SKILL.md` — Claude Code, Codex, Cursor, Antigravity, Gemini CLI และอื่น ๆ โดยไม่ผูกกับ host, model หรือชื่อ tool ใดเป็นการเฉพาะ

---

## ติดตั้ง (คัดลอกไปวางได้เลย)

เลือก agent ที่ใช้ แล้วรันคำสั่งเดียวจบ

**Claude Code**

```bash
git clone https://github.com/duckxy166/duckxy-Summarize.git ~/.claude/skills/duckxy-summarize
```

**Codex**

```bash
git clone https://github.com/duckxy166/duckxy-Summarize.git ~/.codex/skills/duckxy-summarize
```

**Cursor**

```bash
git clone https://github.com/duckxy166/duckxy-Summarize.git ~/.cursor/skills/duckxy-summarize
```

**Antigravity**

```bash
git clone https://github.com/duckxy166/duckxy-Summarize.git ~/.gemini/antigravity/skills/duckxy-summarize
```

**Gemini CLI**

```bash
git clone https://github.com/duckxy166/duckxy-Summarize.git ~/.gemini/skills/duckxy-summarize
```

> Windows ให้รันใน **Git Bash** จะได้ใช้ `~` ได้เหมือนกัน · ถ้าใช้ PowerShell เปลี่ยน `~` เป็น `$env:USERPROFILE` และใช้ `\` แทน `/`

### ตรวจว่าติดตั้งสำเร็จ

1. **ปิดแล้วเปิด agent ใหม่** (สกิลถูกอ่านตอนเริ่ม session)
2. เปิด task ใหม่แล้วพิมพ์ `/duckxy-summarize` (Codex ใช้ `$duckxy-summarize`)
3. ถ้าชื่อสกิลขึ้นมาให้เลือก = ติดตั้งสำเร็จ

ถ้าไม่ขึ้น ให้เช็คว่าในโฟลเดอร์ที่ clone ไปมีไฟล์ `SKILL.md` อยู่ชั้นบนสุดจริง — ต้องเป็น `…/skills/duckxy-summarize/SKILL.md` ไม่ใช่ซ้อนอีกชั้น

### อัปเดตเป็นเวอร์ชันล่าสุด

```bash
cd ~/.claude/skills/duckxy-summarize && git pull
```

### ใช้หลาย agent พร้อมกัน (แนะนำ)

clone ไว้ที่เดียวแล้ว link ไปยังโฟลเดอร์ skills ของแต่ละตัว — `git pull` ครั้งเดียวอัปเดตครบทุก agent

**macOS / Linux**

```bash
mkdir -p ~/.agents/repos ~/.claude/skills ~/.codex/skills
git clone https://github.com/duckxy166/duckxy-Summarize.git ~/.agents/repos/duckxy-summarize
ln -s ~/.agents/repos/duckxy-summarize ~/.claude/skills/duckxy-summarize
ln -s ~/.agents/repos/duckxy-summarize ~/.codex/skills/duckxy-summarize
```

**Windows (PowerShell — ไม่ต้องใช้สิทธิ์ admin)**

```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\repos", "$env:USERPROFILE\.claude\skills"
git clone https://github.com/duckxy166/duckxy-Summarize.git "$env:USERPROFILE\.agents\repos\duckxy-summarize"
New-Item -ItemType Junction -Path "$env:USERPROFILE\.claude\skills\duckxy-summarize" -Target "$env:USERPROFILE\.agents\repos\duckxy-summarize"
```

---

## วิธีเรียกใช้

สกิลนี้เป็น explicit-only จึงต้องเรียกด้วยชื่อโดยตรง รูปแบบขึ้นกับ agent ที่ใช้:

```text
/duckxy-summarize     # Claude Code, Cursor, Antigravity, Gemini CLI
$duckxy-summarize     # Codex
```

ระบุจุดเน้นเพิ่มเติมได้ เช่น:

```text
/duckxy-summarize เน้นผลกระทบต่อ API
```

## ทำอะไรได้บ้าง

- สรุปเป้าหมายและสถานะของ task เป็น **เสร็จ**, **เสร็จบางส่วน** หรือ **ติดขัด** พร้อมเหตุผล
- แยกสิ่งที่ทำไปแล้วออกจากสิ่งที่ยังค้าง, ล้มเหลว, ถูกย้อนกลับ หรือยังไม่ได้ตรวจสอบ
- อธิบายการเปลี่ยนแปลงที่กระทบพฤติกรรม, ไฟล์สำคัญ, API, schema, configuration หรือ dependency เมื่อเกี่ยวข้อง
- รายงานผล test, build, lint และการตรวจสอบอื่น ๆ ที่รันจริงเท่านั้น
- บอกผลกระทบ, breaking changes, ขั้นตอนที่ต้องทำเอง, ความเสี่ยง และงานที่เหลือ
- แยก dirty changes ที่ไม่เกี่ยวข้องออกจากงานปัจจุบันเมื่อหลักฐานไม่เพียงพอ

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
