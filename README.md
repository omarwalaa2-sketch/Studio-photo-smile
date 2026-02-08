<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Studio Photo Smile - التطبيق الرسمي</title>
    <style>
        :root { --primary-color: #e91e63; --secondary-color: #25d366; --bg-gray: #f4f7f6; }
        body { font-family: 'Segoe UI', Tahoma, sans-serif; background-color: var(--bg-gray); margin: 0; padding: 15px; color: #333; }
        .container { max-width: 500px; margin: auto; background: white; border-radius: 25px; overflow: hidden; box-shadow: 0 15px 35px rgba(0,0,0,0.1); }
        .header { background: linear-gradient(135deg, var(--primary-color), #ad1457); color: white; padding: 25px 20px; text-align: center; }
        #statusBox { margin-top: 10px; padding: 8px; border-radius: 20px; font-size: 0.9em; font-weight: bold; }
        .gallery-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; padding: 15px; }
        .gallery-grid img { width: 100%; height: 90px; object-fit: cover; border-radius: 12px; cursor: pointer; transition: 0.3s; }
        .form-section { padding: 20px; }
        label { display: block; margin-top: 12px; font-weight: bold; font-size: 0.9em; }
        input, select { width: 100%; padding: 12px; margin-top: 5px; border: 1.5px solid #eee; border-radius: 10px; box-sizing: border-box; font-size: 1em; }
        .btn { display: block; width: 100%; padding: 15px; margin-top: 20px; border-radius: 12px; text-decoration: none; color: white; font-weight: bold; border: none; cursor: pointer; font-size: 1.1em; text-align: center; }
        .btn-wa { background-color: var(--secondary-color); box-shadow: 0 4px 15px rgba(37, 211, 102, 0.3); }
        .btn-link { background-color: #f0f2f5; color: #555; font-size: 0.9em; margin-top: 10px; }
        .modal, .preview-card, .policy-modal { display: none; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); z-index: 3000; background: white; border-radius: 20px; padding: 20px; width: 85%; max-width: 380px; box-shadow: 0 0 40px rgba(0,0,0,0.2); }
        .overlay { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.7); z-index: 2000; }
        .preview-item { display: flex; justify-content: space-between; border-bottom: 1px solid #eee; padding: 8px 0; }
        .footer-privacy { font-size: 0.8em; color: #999; text-align: center; padding: 20px; cursor: pointer; text-decoration: underline; }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <h1>Studio Photo Smile 📸</h1>
        <p>تصوير 4K - فوتو وفيديو - تعديل وطباعة</p>
        <div id="statusBox">جاري التحقق من وقت العمل...</div>
    </div>

    <div class="gallery-grid">
        <img src="https://picsum.photos/400/400?random=1" onclick="openFullImage(this.src)">
        <img src="https://picsum.photos/400/400?random=2" onclick="openFullImage(this.src)">
        <img src="https://picsum.photos/400/400?random=3" onclick="openFullImage(this.src)">
    </div>

    <div class="form-section">
        <a href="https://www.facebook.com/share/1HNcg2o6ja/" target="_blank" class="btn btn-link">🔵 صفحتنا على فيسبوك</a>
        
        <hr style="border: 0.5px solid #eee; margin: 20px 0;">
        <h3>حجز موعد جديد</h3>
        
        <label>الاسم الكامل:</label>
        <input type="text" id="clientName" placeholder="اكتب اسمك هنا">

        <label>نوع الخدمة:</label>
        <select id="serviceType">
            <option value="الصور الفوريه">1. الصور الفوريه</option>
            <option value="تعديل صوره مرسله و طباعتها">2. تعديل صوره مرسله و طباعتها</option>
            <option value="تصوير حفلة (كتب كتاب/حنه/زفاف/تخرج)">3. تصوير حفلة (صور + فيديو + مونتاج)</option>
            <option value="جلسة تصوير داخليه / خارجيه">4. جلسة تصوير داخليه / خارجيه</option>
            <option value="تغطية مناقشة (ماجستير/دكتوراه/مشروع تخرج)">5. تغطية مناقشة (ماجستير/دكتوراه/مشروع تخرج)</option>
            <option value="تصوير و طباعه تخريجات الأطفال KG1/KG2">6. تصوير و طباعه تخريجات الأطفال KG1/KG2</option>
            <option value="تغطية فعاليات">7. تغطية فعاليات</option>
        </select>

        <label>التاريخ:</label>
        <input type="date" id="bookingDate">

        <label>الوقت (متاح يومياً من 7 مساءً):</label>
        <input type="time" id="bookingTime">

        <button onclick="showBookingPreview()" class="btn btn-wa">حجز الموعد الآن</button>
    </div>

    <div class="footer-privacy" onclick="togglePolicy()">سياسة الخصوصية والأمان 🛡️</div>
</div>

<div id="overlay" class="overlay" onclick="closeAll()"></div>
<div id="imageModal" class="modal" style="background:none; box-shadow:none; text-align:center;">
    <img id="fullImg" src="" style="max-width: 100%; border-radius: 15px;">
</div>

<div id="previewCard" class="preview-card">
    <h3 style="color: var(--primary-color);">تأكيد بيانات الحجز</h3>
    <div class="preview-item"><span>الاسم:</span> <strong id="pName"></strong></div>
    <div class="preview-item"><span>الخدمة:</span> <strong id="pService"></strong></div>
    <div class="preview-item"><span>التاريخ:</span> <strong id="pDate"></strong></div>
    <div class="preview-item"><span>الوقت:</span> <strong id="pTime"></strong></div>
    <button onclick="confirmAndSubmit()" class="btn btn-wa">تأكيد وإرسال واتساب</button>
    <button onclick="closeAll()" class="btn btn-link">تعديل</button>
</div>

<div id="policyModal" class="policy-modal">
    <h3>خصوصيتك تهمنا 🛡️</h3>
    <p>في ستوديو سمايل، نضمن تشفير صورك وعدم نشرها إلا بإذنك الخطّي، وبياناتك الشخصية آمنة لدينا تماماً.</p>
    <button onclick="closeAll()" class="btn btn-wa">فهمت ذلك</button>
</div>

<script>
    function updateStatus() {
        const now = new Date();
        const hour = now.getHours();
        const box = document.getElementById('statusBox');
        if (hour >= 19 || hour < 1) {
            box.innerHTML = "🟢 متاحون الآن لاستقبالكم";
            box.style.backgroundColor = "rgba(0,0,0,0.1)";
        } else {
            box.innerHTML = "🟡 الاستوديو مغلق - نفتح يومياً الساعة 7 مساءً";
            box.style.backgroundColor = "rgba(0,0,0,0.1)";
        }
    }
    updateStatus();

    function openFullImage(src) {
        document.getElementById('fullImg').src = src;
        document.getElementById('imageModal').style.display = 'block';
        document.getElementById('overlay').style.display = 'block';
    }

    function showBookingPreview() {
        const name = document.getElementById('clientName').value;
        const date = document.getElementById('bookingDate').value;
        const time = document.getElementById('bookingTime').value;
        if(!name || !date || !time) return alert("يرجى ملء كافة البيانات");
        document.getElementById('pName').innerText = name;
        document.getElementById('pService').innerText = document.getElementById('serviceType').value;
        document.getElementById('pDate').innerText = date;
        document.getElementById('pTime').innerText = time;
        document.getElementById('previewCard').style.display = 'block';
        document.getElementById('overlay').style.display = 'block';
    }

    function closeAll() {
        document.querySelectorAll('.modal, .preview-card, .policy-modal, .overlay').forEach(el => el.style.display = 'none');
    }

    function togglePolicy() {
        document.getElementById('policyModal').style.display = 'block';
        document.getElementById('overlay').style.display = 'block';
    }

    async function confirmAndSubmit() {
        const name = document.getElementById('pName').innerText;
        const service = document.getElementById('pService').innerText;
        const date = document.getElementById('pDate').innerText;
        const time = document.getElementById('pTime').innerText;
        const msg = `حجز جديد من التطبيق 📸%0Aالاسم: ${name}%0Aالخدمة: ${service}%0Aالتاريخ: ${date}%0Aالوقت: ${time}`;
        window.open(`https://wa.me/962788693107?text=${msg}`, '_blank');
    }
</script>
</body>
</html>
