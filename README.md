# ระบบแจ้งปฏิทินการปฏิบัติงาน

โปรเจกต์นี้ใช้ Google Apps Script และ Google Sheets เป็น backend ส่วนหน้าในโฟลเดอร์ `site` เป็น PWA สำหรับเปิดระบบและติดตั้งเป็นแอป/ทางลัด โดยฝัง Google Apps Script Web App ไว้

## เผยแพร่ผ่าน GitHub Pages

1. สร้าง repository บน GitHub แล้วอัปโหลดไฟล์ทั้งหมดในโฟลเดอร์นี้
2. เปิด **Settings → Pages**
3. ใน **Build and deployment** เลือก **GitHub Actions**
4. push ไฟล์เข้า branch `main` (หรือสั่ง workflow **Deploy GitHub Pages** จากแท็บ **Actions**)
5. รอ workflow ทำงานสำเร็จ แล้วเปิด URL ที่ GitHub แสดง (รูปแบบ `https://<ชื่อผู้ใช้>.github.io/<ชื่อ repository>/`)
6. ใช้ URL ของ GitHub Pages เพื่อเพิ่มทางลัดหรือติดตั้งแอป

Workflow เผยแพร่เฉพาะโฟลเดอร์ `site` เพื่อให้ไฟล์ `Index.html` ของ Google Apps Script ไม่ชนกับ `index.html` ของ GitHub Pages ไฟล์ manifest และ service worker ใช้ path แบบ relative จึงรองรับ URL ที่อยู่ใต้ชื่อ repository

## เพิ่มทางลัดหน้าจอโฮม

- **Android/Chrome และเบราว์เซอร์เดสก์ท็อปที่รองรับ:** เลือก **ติดตั้งแอป** หรือ **เพิ่มไปยังหน้าจอหลัก** จากเมนูเบราว์เซอร์
- **iPhone/iPad:** เปิด URL GitHub Pages ใน Safari → **แชร์** → **เพิ่มไปยังหน้าจอโฮม**

ต้องเปิดผ่าน HTTPS (GitHub Pages ให้ HTTPS) การติดตั้ง PWA ไม่ได้ทำให้ระบบทำงานออฟไลน์ เพราะข้อมูลและฟังก์ชันยังประมวลผลผ่าน Google Apps Script

บนมือถือ หน้า `site/index.html` จะพาไปเปิด Google Apps Script Web App โดยตรง เพราะเบราว์เซอร์มือถือมักบล็อกคุกกี้ของบุคคลที่สามจนระบบที่ฝังใน iframe แสดงผลไม่ได้ (เดสก์ท็อปยังแสดงแบบฝัง และใช้ `?embed=1` เพื่อบังคับฝังบนมือถือได้)

## Google Apps Script

`Code.gs`, `Index.html` และ `Public.html` เป็นไฟล์ต้นฉบับฝั่ง Apps Script ส่วนลิงก์ Web App ปัจจุบันถูกใช้ใน `site/index.html` สำหรับแสดงระบบและเปิดแท็บใหม่ หากเปลี่ยน deployment ให้แก้ URL ทั้งสองตำแหน่งในไฟล์นั้น

## ความเป็นส่วนตัว

Repository แบบ Public จะเปิดเผยไฟล์ต้นฉบับทั้งหมด รวมถึง Spreadsheet ID ที่อยู่ใน `Code.gs` ควรตรวจสอบการแชร์สิทธิ์ของ Google Sheet และอย่าใส่รหัสผ่าน, LINE access token หรือข้อมูลส่วนบุคคลลงใน repository; เก็บค่าลับไว้ใน Script Properties ของ Apps Script
