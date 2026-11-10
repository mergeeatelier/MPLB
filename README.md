# วิธีเอาแอพ MPLB. ขึ้นมือถือ (ฟรี)

## 1. วางแอพบน GitHub Pages (ทำครั้งเดียว ~10 นาที)
1. สมัคร https://github.com (ฟรี)
2. กด **New repository** → ตั้งชื่อ เช่น `ngern` → เลือก **Public** → Create
3. กด **uploading an existing file** → ลากไฟล์ทั้งหมดในโฟลเดอร์นี้ขึ้นไป (index.html, manifest.json, sw.js, icon-192.png, icon-512.png) → Commit
4. ไปที่ **Settings → Pages** → Source: *Deploy from a branch* → Branch: `main` / `root` → Save
5. รอ 1–2 นาที จะได้ลิงก์ `https://ชื่อผู้ใช้.github.io/ngern/`

> ทางเลือกที่ง่ายกว่า: https://app.netlify.com/drop แล้วลากทั้งโฟลเดอร์ใส่ ได้ลิงก์ทันที (ควรสมัครบัญชีเพื่อไม่ให้ลิงก์หมดอายุ)

## 2. ติดตั้งบนมือถือ
- **iPhone:** เปิดลิงก์ใน Safari → ปุ่มแชร์ → "เพิ่มไปยังหน้าจอโฮม"
- **Android:** เปิดใน Chrome → เมนู ⋮ → "ติดตั้งแอป"

## 3. สำรองข้อมูล
ข้อมูลอยู่ในเครื่องเท่านั้น ไปที่ไอคอนรูปคนมุมขวาบน → ตั้งค่า → **ส่งออกไฟล์สำรอง** อย่างน้อยเดือนละครั้ง แล้วเก็บไว้ใน iCloud / Google Drive

## 4. (ไม่บังคับ) ซิงก์ข้ามเครื่องด้วยบัญชี Google — ฟรี ~15 นาที
1. ไปที่ https://console.firebase.google.com → **Add project** → ตั้งชื่อ (ปิด Google Analytics ได้) → Create
2. เมนู **Build → Authentication** → Get started → แท็บ Sign-in method → เปิด **Google** → Save
3. ในหน้า Authentication → **Settings → Authorized domains** → Add domain → ใส่ `ชื่อผู้ใช้.github.io` (หรือโดเมน Netlify ของคุณ)
4. เมนู **Build → Firestore Database** → Create database → location `asia-southeast1` → Start in **production mode**
5. แท็บ **Rules** ของ Firestore → แทนที่ทั้งหมดด้วยข้อความนี้ แล้วกด Publish
```
rules_version = '2';
service cloud.firestore {
  match /databases/{db}/documents {
    match /users/{uid}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
  }
}
```
6. หน้าแรกของโปรเจกต์ → ไอคอน **</>** (Add app → Web) → ตั้งชื่อ → Register → คัดลอกส่วน `const firebaseConfig = {...}`
7. ในแอพ: ตั้งค่า → ซิงก์ข้ามเครื่องด้วย Google → วางค่าที่คัดลอก → บันทึกการตั้งค่า → **เข้าสู่ระบบด้วย Google**
8. เครื่องอื่น: เปิดลิงก์เดียวกัน ทำข้อ 7 ซ้ำ (วางค่าเดิม ล็อกอินอีเมลเดียวกัน)

หมายเหตุ
- ฟรีภายใต้โควตา Firebase แผน Spark เหลือเฟือสำหรับคนเดียว
- รายการแยกเก็บเป็นรายปี (ปีละไม่เกินประมาณ 5,000 รายการ) จึงใช้ได้ยาวไม่จำกัดจำนวนปี
- ข้อมูลอยู่ในโปรเจกต์ Firebase ของคุณเอง เฉพาะบัญชี Google ของคุณอ่านได้ (ตาม Rules ข้อ 5)
- ถ้าแก้ข้อมูลพร้อมกันสองเครื่องตอนออฟไลน์ เครื่องที่บันทึกทีหลังจะทับ
- iPhone: ถ้ากดล็อกอินจากไอคอนหน้าจอโฮมแล้วไม่มีหน้าต่างขึ้น ให้ล็อกอินผ่าน Safari ปกติก่อน

## อัปเดตแอพในอนาคต
อัปโหลด index.html ใหม่ทับไฟล์เดิมใน GitHub ข้อมูลในมือถือไม่หาย (ผูกกับลิงก์เดิม)
