# PSD 3D-eNGINNo IFM SMART BUILDING

ระบบบริหารอาคารแบบ 3D + หลังบ้าน IFM (Hard + Soft Services) ใช้ฐานข้อมูลเดียวกันบน Google Sheets

| ไฟล์ | คืออะไร |
|---|---|
| `index.html` | **หน้าหลัก 3D Smart Building** – อาคาร 3D ที่สร้างจากพื้นที่จริงใน IFM, มีคน/เครื่องจักรเคลื่อนไหว, แจ้งซ่อม/ปิดงาน/ส่งแม่บ้าน/จองห้องประชุมได้จากโมเดล |
| `admin.html` | **หลังบ้าน (Back Office)** – IFM Management เดิมทุกโมดูล: Command Center, Helpdesk, Engineering, Facility, Housekeeping, Security, Pest, Garden, Linen, Manpower, Vendor, SLA/KPI, Users |
| `mobile.html` | **IFM Mobile** – สำหรับช่าง / แม่บ้าน / ผู้แจ้งงาน |
| `apps-script/Code.gs` | **Backend** (Google Apps Script) – สร้าง/อ่าน/เขียนฐานข้อมูล Google Sheets, ระบบผู้ใช้ + สิทธิ์ |
| `apps-script/appsscript.json` | Manifest ของ Apps Script (ไม่บังคับ) |

ทั้ง 3 หน้าใช้ **บัญชีผู้ใช้, การตั้งค่า, ข้อมูลชุดเดียวกัน** เมื่อเปิดจากเว็บเดียวกัน (GitHub Pages)

---

## 1) อัปขึ้น GitHub Pages
1. อัปโหลด `index.html`, `admin.html`, `mobile.html`, `README.md` และโฟลเดอร์ `apps-script/` ไปที่ root ของ repo
2. Settings → Pages → Source: **Deploy from a branch** → Branch `main` / `(root)` → Save
3. เปิด `https://<user>.github.io/<repo>/` → จะเข้าหน้า 3D ทันที

## 2) ติดตั้ง Apps Script (ครั้งเดียว)
1. ไปที่ <https://script.google.com> → **New project** → ตั้งชื่อ `PSD 3D-eNGINNo Backend`
2. ลบโค้ดเดิม → วางทั้งไฟล์ `apps-script/Code.gs`
3. เลือกฟังก์ชัน **`setup`** → **Run** → อนุญาตสิทธิ์ (Drive, Sheets, Mail)
4. ดู **Execution log** → จด **API KEY** และ **รหัสผ่าน admin**
   - ระบบจะสร้างโฟลเดอร์ `PSD 3D-eNGINNo IFM SMART BUILDING` ใน Google Drive พร้อม Spreadsheet **Registry**
5. **Deploy → New deployment → Web app**
   - Execute as: **Me** · Who has access: **Anyone** → Deploy → คัดลอก URL ที่ลงท้าย `/exec`

> แก้โค้ดภายหลัง: Deploy → Manage deployments → ✏ Edit → Version: **New version** (URL เดิมใช้ต่อได้)

## 3) เชื่อมเว็บกับฐานข้อมูล
1. เปิด `https://<user>.github.io/<repo>/admin.html?srv=<URL /exec>` (ระบบจำ URL ให้เอง)
2. เข้าสู่ระบบ `admin` + รหัสจาก Execution log → เปลี่ยนรหัสผ่านที่เมนูบัญชี
3. **Settings → ☁ ฐานข้อมูล Google Sheets** → ใส่ **รหัสอาคาร** เช่น `HTL-01` → กด **“สร้าง / เชื่อม Spreadsheet อาคารนี้”**
4. ที่ **Settings → โหมดข้อมูล** เลือก **✅ ข้อมูลจริง** → กด **เริ่มอาคารจริง** (ล้างข้อมูลตัวอย่าง) แล้วกรอก **พื้นที่ (Areas)**, ทรัพย์สิน, งาน HK ฯลฯ
5. เปิด **Building Critical Status → 🏗 ตั้งค่าอาคาร / ✏ แก้ไขผังห้อง** เพื่อกำหนดชั้นและตำแหน่งห้อง → หน้า 3D ใช้ผังเดียวกันนี้
6. เปิด `index.html` → ปุ่ม **🔗 ข้อมูลจริง IFM** จะแสดงอาคารจากข้อมูลจริง และดึงจาก Google Sheets อัตโนมัติทุก ~30 วินาที
7. เพิ่มผู้ใช้ที่ **ผู้ใช้งาน & สิทธิ์** แล้วส่งลิงก์ `…/mobile.html?srv=<URL>` ให้ทีมงาน

## ฐานข้อมูล Google Sheets
```
Google Drive / PSD 3D-eNGINNo IFM SMART BUILDING /
├─ PSD 3D-eNGINNo · Registry      ← Users · Sites · Log (ฐานกลางของทุกระบบ)
└─ <business> / <SITE> /
   ├─ IFM · <business> · <SITE>   ← 1 ชีตต่อ 1 ตาราง
   │    tickets, assets, pm, areas, hkTasks, hkInsp, patrols, secInc, visitors,
   │    pestSt, pestVisit, gdnZones, gdnTasks, linenItems, linenTx, omEquip, omTest,
   │    omLog, meters, defects, compliance, staff, vendors, maContract, kpiCustom,
   │    bookings (ห้องประชุมจากหน้า 3D) · _config (ตั้งค่าอาคาร/ผังชั้น) · _meta
   ├─ media/   ← รูปถ่าย/ลายเซ็นที่อัปโหลด (แปลงจาก base64 เป็นไฟล์ Drive อัตโนมัติ)
   └─ data/    ← ข้อมูลที่ยาวเกิน 1 เซลล์
```

## หน้า 3D อ่านข้อมูลอะไร
| ตาราง | แสดงผลใน 3D |
|---|---|
| `areas` + `_config.bldg` | ห้อง 1 ห้องต่อ 1 พื้นที่ · ชนิดห้องเดาจากชื่อ (Chiller, ICU, OR, Kitchen, Lobby, Office, Meeting, Warehouse, Production Line, Car Park …) |
| `assets` | ผูกกับเครื่องจักร 3D · Running = หมุน/ไฟเขียว · Warning = ไฟเหลือง · Down = หยุด + วงแดง ⚠ |
| `tickets` | ป้าย 🔧 · In Progress/Assigned → ช่าง, แม่บ้าน, รปภ. หรือทีม Pest เดินเข้าไปทำงานตาม Service |
| `hkTasks` | งานกะปัจจุบัน → แม่บ้านถูพื้น + ป้ายพื้นเปียก · Missed → ป้ายเตือน |
| `secInc` / `pestSt` / `defects` | ป้าย 🛡 🐜 🏗 และสถานะวิกฤตของห้อง |
| `bookings` | ห้องประชุม: มีคนประชุม / ว่าง |

ปุ่ม **🧪 Demo 3D** ใช้ข้อมูลจำลอง 7 ประเภทอาคาร (รวม Education) สำหรับนำเสนอ

## บทบาทผู้ใช้ (บังคับที่ Server)
`admin` ทุกอย่าง · `fm` อาคารที่ได้รับสิทธิ์ + ตั้งค่า + จัดการผู้ใช้ระดับล่าง · `sup` / `tech` แก้ไขเฉพาะ Module ที่ได้รับสิทธิ์ · `client` / `requester` แจ้งงานและดูสถานะ

---
PSD 3D-eNGINNo · IFM SMART BUILDING · 2026
