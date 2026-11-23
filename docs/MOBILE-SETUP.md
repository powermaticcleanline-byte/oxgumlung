# ตั้งค่าด้วยมือถือหรือแท็บเล็ต (ไม่ต้องมีคอม)

ใช้แค่เบราว์เซอร์ (Safari หรือ Chrome) ไม่ต้องใช้ Apps Script ใช้ Firebase แทน
ใช้แท็บเล็ตจะสะดวกกว่า เพราะหน้า Firebase กับ GitHub กว้าง

> **เคล็ดลับ:** ถ้าปุ่มไหนหาไม่เจอ ให้เปิดเว็บเป็น "เวอร์ชันเดสก์ท็อป"
> - Safari: ปุ่ม **AA** ในช่องที่อยู่ → **ขอเว็บไซต์เดสก์ท็อป**
> - Chrome: เมนู **⋮** → ติ๊ก **เว็บไซต์เวอร์ชันเดสก์ท็อป**

---

## ขั้นที่ 0 · แตกไฟล์ zip

- **iPhone/iPad:** เปิดแอป **ไฟล์ (Files)** → แตะ `kongklam.zip` หนึ่งครั้ง → จะได้โฟลเดอร์ `kongklam`
- **Android:** เปิดแอป **Files by Google** → แตะ `kongklam.zip` → **แตกไฟล์ (Extract)**

## ขั้นที่ 1 · สร้าง Firebase (ประมาณ 10 นาที)

1. เปิด https://console.firebase.google.com → ล็อกอิน Google → **Create a project** → ตั้งชื่อ `kongklam` → ปิด Google Analytics → **Create project**
2. เมนู ☰ (มุมซ้ายบน) → **Build → Authentication → Get started** → **Google** → เปิด **Enable** → เลือกอีเมล → **Save**
3. เมนู ☰ → **Build → Firestore Database → Create database** → Location **asia-southeast1** → **production mode** → **Create**
4. แท็บ **Rules** → ลบข้อความเดิมทั้งหมด → พิมพ์หรือวางข้อความนี้ → **Publish**

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{uid} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

5. รูปเฟือง ⚙️ → **Project settings** → เลื่อนลงไป **Your apps** → ไอคอน **`</>`** → ตั้งชื่อ `kongklam-web` → **Register app**
6. จะเห็นค่า `apiKey`, `authDomain`, `projectId`, `storageBucket`, `messagingSenderId`, `appId` → **แคปหน้าจอเก็บไว้** หรือกดค้างเพื่อคัดลอกใส่โน้ต (ใช้ในขั้นที่ 3)

## ขั้นที่ 2 · อัปโหลดขึ้น GitHub (ประมาณ 10 นาที)

1. เปิด https://github.com → สมัครหรือล็อกอิน
2. ปุ่ม **+** มุมขวาบน → **New repository** → ชื่อ `kongklam` → **Public** → **Create repository**
3. กดลิงก์ **uploading an existing file** (หรือ **Add file → Upload files**)
4. กด **choose your files** → เข้าโฟลเดอร์ `kongklam` → เลือกไฟล์เหล่านี้ (เลือกหลายไฟล์พร้อมกันได้):
   - `index.html`
   - `firebase-config.js`
   - `manifest.json`
   - `sw.js`
   - `icon-192.png`
   - `icon-512.png`
   - (โฟลเดอร์ `docs` กับไฟล์ `.nojekyll` ไม่ต้องอัปก็ได้ แอปใช้งานได้เหมือนกัน)
5. เลื่อนลง → **Commit changes**
6. **Settings** (แท็บบนสุดของ repository) → เมนู **Pages** → Source: **Deploy from a branch** → Branch: **main** / **(root)** → **Save**
7. รอ 1–2 นาที รีเฟรชหน้า Pages จะเห็น `https://ชื่อผู้ใช้.github.io/kongklam/`

## ขั้นที่ 3 · ใส่ค่า Firebase ในไฟล์ config (แก้บนเว็บ GitHub ได้เลย)

1. ใน repository แตะไฟล์ **`firebase-config.js`** → ไอคอน **ดินสอ ✏️** (Edit)
2. เปลี่ยนคำว่า `ใส่ apiKey`, `ใส่ authDomain` … ให้เป็นค่าจากขั้นที่ 1 ข้อ 6 **ให้ครบทั้ง 6 ค่า** โดยเหลือเครื่องหมาย `"` ไว้หน้าหลังเหมือนเดิม
3. **Commit changes** → **Commit changes**

ตัวอย่างหลังแก้:
```
apiKey: "AIzaSy...",
authDomain: "kongklam.firebaseapp.com",
```

## ขั้นที่ 4 · อนุญาตโดเมนให้ล็อกอินได้

Firebase → ☰ → **Authentication** → แท็บ **Settings** → **Authorized domains** → **Add domain** → พิมพ์ `ชื่อผู้ใช้.github.io` → **Add**

## ขั้นที่ 5 · ทดสอบและติดตั้งเป็นแอป (PWA)

1. เปิด `https://ชื่อผู้ใช้.github.io/kongklam/` → เจอหน้าแนะนำ 3 หน้า → **เข้าสู่ระบบด้วย Google**
2. ติดตั้งลงหน้าจอโฮม ไอคอนจะเป็นน้องก้อนกล้าม 8-bit:
   - **iPhone/iPad (Safari):** ปุ่มแชร์ ⬆️ → **เพิ่มไปยังหน้าจอโฮม** → **เพิ่ม**
   - **Android (Chrome):** เมนู ⋮ → **ติดตั้งแอป** หรือ **เพิ่มลงในหน้าจอหลัก**
3. เปิดจากไอคอน แอปจะเต็มจอ ไม่มีแถบเบราว์เซอร์

## ขั้นที่ 6 · ย้ายข้อมูลจากแอปเดิมใน Claude

1. แอปเดิม → **ฉัน → ข้อมูลของฉัน → สำรองข้อมูล → คัดลอก**
2. แอปใหม่ → **ฉัน → ข้อมูลของฉัน → นำเข้าข้อมูล** → กดค้างในช่อง → **วาง** → **นำเข้า**

---

## ติดปัญหา

| อาการ | วิธีแก้ |
|---|---|
| กดล็อกอินแล้วขึ้น `unauthorized-domain` | ทำขั้นที่ 4 |
| ไม่มีหน้าล็อกอิน เข้าแอปเลย | `firebase-config.js` ยังใส่ไม่ครบหรือลบ `"` ไป แก้ใหม่ในขั้นที่ 3 |
| ล็อกอินแล้วข้อมูลไม่ซิงก์ | เช็ก Rules ในขั้นที่ 1 ข้อ 4 ว่ากด Publish แล้ว |
| หน้าเว็บ 404 | รอ 2–3 นาที แล้วเช็ก Settings → Pages |
| iPhone กดล็อกอินแล้วเด้งกลับ | กดอีกครั้ง แอปจะเปลี่ยนไปล็อกอินแบบเปลี่ยนหน้าให้เอง |
| แก้ไฟล์แล้วแอปยังเหมือนเดิม | ปิดแอปแล้วเปิดใหม่ 1–2 ครั้ง |
