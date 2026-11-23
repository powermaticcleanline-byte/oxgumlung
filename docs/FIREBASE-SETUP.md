# คู่มือตั้งค่า Firebase + GitHub Pages

ทำตามลำดับ ติ๊ก ☐ เป็น ☑ ได้ระหว่างทำ ทั้งหมดฟรี ไม่ต้องผูกบัตร

## สิ่งที่ต้องมี

- บัญชี Google (ใช้สร้าง Firebase)
- บัญชี GitHub
- ไฟล์ทั้งหมดในโฟลเดอร์ `kongklam`

---

## ส่วนที่ 1 · สร้างโปรเจกต์ Firebase

☐ 1. เข้า https://console.firebase.google.com → **Create a project**
☐ 2. ตั้งชื่อ เช่น `kongklam` → Google Analytics ปิดได้ → **Create project**

## ส่วนที่ 2 · เปิดล็อกอินด้วย Google

☐ 3. เมนูซ้าย **Build → Authentication → Get started**
☐ 4. แท็บ **Sign-in method** → เลือก **Google** → เปิด **Enable** → เลือกอีเมลติดต่อ → **Save**

## ส่วนที่ 3 · สร้างฐานข้อมูล Firestore

☐ 5. เมนูซ้าย **Build → Firestore Database → Create database**
☐ 6. Location: `asia-southeast1 (Singapore)` (ใกล้ไทยที่สุด เปลี่ยนทีหลังไม่ได้)
☐ 7. เลือก **Start in production mode** → **Create**
☐ 8. แท็บ **Rules** → ลบของเดิมทั้งหมด → วางเนื้อหาไฟล์ `firestore.rules` → **Publish**

> กฎนี้ทำให้แต่ละคนอ่านเขียนได้เฉพาะ `users/{uid}` ของตัวเอง ห้ามข้ามขั้นนี้

## ส่วนที่ 4 · เอาค่า config มาใส่แอป

☐ 9. รูปเฟือง ⚙️ ข้าง Project Overview → **Project settings**
☐ 10. เลื่อนลงไป **Your apps** → กดไอคอน **`</>`** (Web) → ตั้งชื่อ เช่น `kongklam-web` → ไม่ต้องติ๊ก Hosting → **Register app**
☐ 11. จะเห็นโค้ด `const firebaseConfig = { apiKey: "...", ... }`
☐ 12. เปิดไฟล์ `firebase-config.js` → เอาค่าแต่ละบรรทัดมาใส่แทน `ใส่ ...` ให้ครบ 6 ค่า

ตัวอย่างหลังแก้ (ค่าจริงของคุณจะต่างไป):

```js
window.FIREBASE_CONFIG = {
  apiKey: "AIzaSy...",
  authDomain: "kongklam.firebaseapp.com",
  projectId: "kongklam",
  storageBucket: "kongklam.appspot.com",
  messagingSenderId: "1234567890",
  appId: "1:1234567890:web:abc123"
};
```

## ส่วนที่ 5 · ขึ้น GitHub Pages

☐ 13. https://github.com/new → Repository name `kongklam` → **Public** → **Create repository**
☐ 14. กดลิงก์ **uploading an existing file** → ลากทุกไฟล์ในโฟลเดอร์ขึ้นไป รวมโฟลเดอร์ `docs` และไฟล์ `.nojekyll`
   - บน Mac ไฟล์ที่ขึ้นต้นด้วยจุดถูกซ่อน กด `Cmd + Shift + .` ใน Finder เพื่อให้เห็น
☐ 15. **Commit changes**
☐ 16. **Settings → Pages** → Source: **Deploy from a branch** → Branch: **main** / **(root)** → **Save**
☐ 17. รอ 1–2 นาที แล้วรีเฟรชหน้า Pages จะเห็นลิงก์ `https://ชื่อผู้ใช้.github.io/kongklam/`

## ส่วนที่ 6 · อนุญาตโดเมนให้ล็อกอินได้

☐ 18. Firebase → **Authentication → Settings → Authorized domains → Add domain**
☐ 19. ใส่ `ชื่อผู้ใช้.github.io` (ไม่ต้องมี https:// และไม่ต้องมี /kongklam) → **Add**

## ส่วนที่ 7 · ทดสอบ

☐ 20. เปิดลิงก์แอป → เจอหน้าแนะนำ 3 หน้า → หน้าล็อกอิน → **เข้าสู่ระบบด้วย Google**
☐ 21. แก้ชื่อเล่นในหน้า ฉัน → กลับไปดู Firebase → **Firestore Database → Data** ต้องเห็น `users` → เอกสารชื่อยาวๆ (uid) → มีข้อมูล `state`
☐ 22. เปิดแอปในอีกเครื่อง ล็อกอินบัญชีเดียวกัน ข้อมูลต้องตรงกัน

## ส่วนที่ 8 · ย้ายข้อมูลจากแอปเดิม

☐ 23. แอปเดิมใน Claude → **ฉัน → ข้อมูลของฉัน → สำรองข้อมูล → คัดลอก**
☐ 24. แอปใหม่ (ล็อกอินแล้ว) → **ฉัน → ข้อมูลของฉัน → นำเข้าข้อมูล** → วาง → **นำเข้า**

---

## แก้ปัญหาที่เจอบ่อย

| อาการ | สาเหตุ | วิธีแก้ |
|---|---|---|
| กดล็อกอินแล้วขึ้น `auth/unauthorized-domain` | ยังไม่ได้ทำส่วนที่ 6 | เพิ่ม `ชื่อผู้ใช้.github.io` ใน Authorized domains |
| ไม่มีหน้าล็อกอินขึ้นเลย แอปเข้าหน้าหลักทันที | `firebase-config.js` ยังเป็น `ใส่ ...` หรือพิมพ์ผิด | เช็กว่าใส่ครบ 6 ค่า มีเครื่องหมาย `"` และ `,` ครบ |
| ล็อกอินได้ แต่ข้อมูลไม่ขึ้น Firestore / ขึ้น `permission-denied` | ยังไม่ได้วางกฎ หรือวางไม่ครบ | ทำข้อ 8 ใหม่ แล้วกด Publish |
| หน้า GitHub Pages ขึ้น 404 | Pages ยังไม่เสร็จ หรือเลือก branch ผิด | รอ 2–3 นาที เช็ก Settings → Pages ว่าเป็น main / (root) |
| บน iPhone กดล็อกอินแล้วเด้งกลับมาเฉยๆ | Safari บล็อกป๊อปอัป แอปจะสลับไปแบบเปลี่ยนหน้าให้เอง | กดอีกครั้ง หรือ Settings → Safari → ปิด Block Pop-ups ชั่วคราว |
| อัปเดต `index.html` แล้วยังเห็นของเก่า | มือถือเก็บแอปไว้ในเครื่อง (ใช้ออฟไลน์ได้) | ปิดแล้วเปิดแอปใหม่ 1–2 ครั้ง |

## โครงข้อมูลใน Firestore

```
users/{uid}
  state:   { name, profile, routines[], history[], body[], schedule[], goal, rest, ach, ... }
  updated: เวลาที่บันทึกล่าสุด (ms)
```

- เก็บทั้งหมดเป็นเอกสารเดียวต่อคน (จำกัด 1 MiB ต่อเอกสาร พอสำหรับประวัติหลายร้อยครั้ง)
- ถ้าวันหลังประวัติเยอะมาก หรือจะทำฟีเจอร์เพื่อน/ลีก ค่อยแยก `history` ออกเป็นคอลเลกชันย่อย `users/{uid}/history/{id}`

## โควตาแพ็กเกจฟรี (Spark)

- เก็บข้อมูลได้ 1 GiB, อ่าน 50,000 ครั้ง/วัน, เขียน 20,000 ครั้ง/วัน
- เกินโควตา บริการจะหยุดชั่วคราวจนถึงรอบถัดไป ไม่มีค่าใช้จ่าย
- ดูการใช้งานได้ที่ Firestore → แท็บ **Usage**
