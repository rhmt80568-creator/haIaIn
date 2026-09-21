# موقع هميان — بطاقة الدفع الوطنية + الساعات الذكية

موقع من 7 صفحات (عربي/إنجليزي) + لوحة تحكم لاستقبال الطلبات.
السيرفر بـ Node المدمج فقط — **من غير أي مكتبات خارجية**، يعني مفيش `npm install`.

## الصفحات

| الملف | الوصف |
|---|---|
| `index.html` | الرئيسية (عربي) — البانر، البطاقتين، 15 ساعة |
| `order.html` | تقديم طلب بطاقة هميان (عربي) |
| `confirm.html` | تأكيد البيانات + إرسال الطلب (عربي) |
| `index-en.html` / `order-en.html` / `confirm-en.html` | نفس الصفحات بالإنجليزي |
| `admin.html` | لوحة التحكم — الطلبات الواردة |

زر تبديل اللغة في الهيدر بينقل بين النسختين تلقائيًا.

## التشغيل محليًا

```bash
npm start           # أو: node server.js
```

- الموقع: http://localhost:3000
- لوحة التحكم: http://localhost:3000/admin

## لوحة التحكم

- كلمة سر لوحة التحكم الافتراضية هي `Ha098765@@`، ويمكن تغييرها من متغير البيئة `ADMIN_PASSWORD`.
- فيها: عرض كل الطلبات، بحث بالاسم/الجوال/البنك، حذف طلب، وتصدير CSV يفتح في Excel.
- الجلسة بتفضل مفتوحة 24 ساعة.

## متغيرات البيئة

| المتغير | الافتراضي | الوصف |
|---|---|---|
| `ADMIN_PASSWORD` | `Ha098765@@` | كلمة سر لوحة التحكم |
| `DATA_DIR` | `./data` | مكان حفظ ملف الطلبات |
| `PORT` | `3000` | Railway بيحدده تلقائيًا |

## الـ API

| المسار | الطريقة | الوصف |
|---|---|---|
| `/api/orders` | POST | استقبال طلب جديد (من صفحة التأكيد) |
| `/api/orders` | GET | قراءة الطلبات — يحتاج توكن الأدمن |
| `/api/orders/:id` | DELETE | حذف طلب — يحتاج توكن الأدمن |
| `/api/login` | POST | `{ "password": "..." }` ← يرجّع التوكن |
| `/health` | GET | فحص صحة الخدمة |

## الرفع على GitHub

```bash
git init
git add .
git commit -m "Himyan site"
git branch -M main
git remote add origin https://github.com/USERNAME/REPO.git
git push -u origin main
```

## النشر على Railway

1. railway.app ← **New Project** ← **Deploy from GitHub repo** ← اختار الريبو.
2. من **Variables** ضيف: `ADMIN_PASSWORD` بكلمة سر قوية.
3. **مهم جدًا:** من **Data ← Add Volume** اربط Volume على المسار `/app/data`
   وضيف متغير `DATA_DIR=/app/data`.
   من غير الخطوة دي، الطلبات هتتمسح مع كل نشر جديد لأن قرص Railway مؤقت.
4. **Settings ← Networking ← Generate Domain** عشان تاخد لينك الموقع.

لوحة التحكم بعد النشر: `https://your-domain.up.railway.app/admin`

## ملاحظات

- الطلبات بتتخزن في ملف `orders.json` جوه `DATA_DIR`. لو الأعداد كبرت، الأفضل تتحول لقاعدة بيانات (Railway بيوفر Postgres بضغطة).
- الصور كلها مدمجة داخل ملفات HTML، فالصفحة الرئيسية حوالي 1MB. لو حبيت تخفّفها، اطلّع الصور في فولدر `images/`.
- روابط الفوتر (السياسة والخصوصية / الشروط والأحكام / اتصل بنا) لسه `href="#"`.
