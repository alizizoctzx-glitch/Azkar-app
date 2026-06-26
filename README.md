<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>حصن المسلم الرقمي | Digital Azkar</title>
    <link href="https://fonts.googleapis.com/css2?family=Amiri&family=Tajawal:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg: #f8fafc;
            --card-bg: #ffffff;
            --text-dark: #1e293b;
            --text-muted: #64748b;
            --border: #e2e8f0;
            --primary: #0f766e;
            --accent: #d97706;
            --btn-bg: #f1f5f9;
            --overlay: rgba(0,0,0,0.4);
        }

        [data-theme="dark"] {
            --bg: #0f172a;
            --card-bg: #1e293b;
            --text-dark: #f8fafc;
            --text-muted: #94a3b8;
            --border: #334155;
            --btn-bg: #334155;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Tajawal', sans-serif; transition: background-color 0.3s, border-color 0.3s, color 0.3s; }
        body { background-color: var(--bg); color: var(--text-dark); padding: 20px; max-width: 600px; margin: 0 auto; }
        
        header { text-align: center; padding: 30px 25px; background: linear-gradient(135deg, #064e3b, #0f766e); color: white; border-radius: 16px; margin-bottom: 20px; position: relative; }
        header h1 { font-family: 'Amiri', serif; font-size: 32px; margin-bottom: 5px; }
        
        /* زرار الستينج فوق في اليمين */
        .settings-btn { position: absolute; top: 15px; right: 15px; background: rgba(255,255,255,0.2); border: none; color: white; padding: 8px; border-radius: 50px; cursor: pointer; font-size: 18px; display: flex; align-items: center; justify-content: center; width: 40px; height: 40px; }
        html[dir="ltr"] .settings-btn { right: auto; left: 15px; }

        /* نافذة الإعدادات المنبثقة */
        .settings-modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: var(--overlay); z-index: 1000; align-items: center; justify-content: center; }
        .settings-content { background: var(--card-bg); padding: 25px; border-radius: 16px; width: 90%; max-width: 400px; border: 1px solid var(--border); box-shadow: 0 10px 25px rgba(0,0,0,0.1); }
        .settings-content h3 { margin-bottom: 20px; border-bottom: 1px solid var(--border); padding-bottom: 10px; font-size: 20px; }
        .setting-row { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; }
        .setting-row span { font-weight: bold; }
        .setting-select { padding: 8px 12px; border-radius: 8px; border: 1px solid var(--border); background: var(--btn-bg); color: var(--text-dark); font-weight: bold; font-size: 14px; cursor: pointer; }
        .close-btn { width: 100%; padding: 12px; background: var(--primary); color: white; border: none; border-radius: 10px; font-weight: bold; cursor: pointer; font-size: 16px; margin-top: 10px; }

        .tabs { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 20px; }
        .tab-btn { flex: 1; min-width: 80px; padding: 12px; border: none; background-color: var(--btn-bg); color: var(--text-dark); font-weight: bold; border-radius: 10px; cursor: pointer; font-size: 14px; text-align: center; }
        .tab-btn.active { background-color: var(--primary); color: white; }
        
        .dhikr-section { display: none; }
        .dhikr-section.active { display: block; }
        
        .card { background-color: var(--card-bg); border-radius: 14px; padding: 20px; margin-bottom: 15px; border: 1px solid var(--border); box-shadow: 0 2px 4px rgba(0,0,0,0.02); }
        .text { font-family: 'Amiri', serif; font-size: 24px; line-height: 1.6; margin-bottom: 12px; text-align: justify; }
        html[dir="ltr"] .text { font-family: 'Tajawal', sans-serif; font-size: 18px; text-align: left; }
        .source { font-size: 13px; color: var(--text-muted); border-top: 1px dashed var(--border); padding-top: 8px; margin-bottom: 12px; }
        
        .counter-btn { width: 100%; padding: 14px; background-color: var(--btn-bg); border: 2px solid var(--border); color: var(--text-dark); font-weight: bold; border-radius: 10px; cursor: pointer; font-size: 16px; }
        .counter-btn.done { background-color: #d1fae5; border-color: #10b981; color: #065f46; }
        [data-theme="dark"] .counter-btn.done { background-color: #064e3b; color: #a7f3d0; }
    </style>
</head>
<body>

    <header>
        <button class="settings-btn" onclick="toggleSettings()">⚙️</button>
        <h1 id="mainTitle">أذكار المسلم</h1>
        <p id="mainSub">أذكارك اليومية في مكان واحد</p>
    </header>

    <div class="settings-modal" id="settingsModal">
        <div class="settings-content">
            <h3 id="settingsTitle">الإعدادات ⚙️</h3>
            <div class="setting-row">
                <span id="langLabel">اللغة</span>
                <select class="setting-select" id="langSelect" onchange="changeLanguage()">
                    <option value="ar">العربية</option>
                    <option value="en">English</option>
                </select>
            </div>
            <div class="setting-row">
                <span id="themeLabel">المظهر</span>
                <select class="setting-select" id="themeSelect" onchange="changeTheme()">
                    <option value="light">☀️ فاتح</option>
                    <option value="dark">🌙 داكن</option>
                </select>
            </div>
            <button class="close-btn" onclick="toggleSettings()" id="closeBtn">حفظ وإغلاق</button>
        </div>
    </div>

    <div class="tabs">
        <button class="tab-btn active" id="tabMorning" onclick="switchTab('morning')">الصباح</button>
        <button class="tab-btn" id="tabEvening" onclick="switchTab('evening')">المساء</button>
        <button class="tab-btn" id="tabSleeping" onclick="switchTab('sleeping')">النوم</button>
        <button class="tab-btn" id="tabGeneral" onclick="switchTab('general')">أذكار عامة</button>
    </div>

    <div id="morning" class="dhikr-section active">
        <div class="card">
            <p class="text" data-ar="أَصْبَحْنَا وَأَصْبَحَ الْمُلْكُ لِلَّهِ، وَالْحَمْدُ لِلَّهِ لَا إِلَهَ إِلَّا اللَّهُ وَحْدَهُ لَا شَرِيكَ لَهُ." data-en="We have entered upon morning and the whole kingdom belongs to Allah, praise is due to Allah."></p>
            <p class="source" data-ar="رواه مسلم" data-en="Narrated by Muslim"></p>
            <button class="counter-btn" onclick="count(this, 1)">1</button>
        </div>
        <div class="card">
            <p class="text" data-ar="اللَّهُمَّ بِكَ أَصْبَحْنَا، وَبِكَ أَمْسَيْنَا، وَبِكَ نَحْيَا، وَبِكَ نَمُوتُ، وَإِلَيْكَ النُّشُورُ." data-en="O Allah, by You we enter the morning and by You we enter the evening, by You we live and by You we die."></p>
            <p class="source" data-ar="رواه الترمذي" data-en="Narrated by Al-Tirmidhi"></p>
            <button class="counter-btn" onclick="count(this, 1)">1</button>
        </div>
    </div>

    <div id="evening" class="dhikr-section">
        <div class="card">
            <p class="text" data-ar="أَمْسَيْنَا وَأَمْسَى الْمُلْكُ لِلَّهِ، وَالْحَمْدُ لِلَّهِ لَا إِلَهَ إِلَّا اللَّهُ وَحْدَهُ لَا شَرِيكَ لَهُ." data-en="We have entered upon evening and the whole kingdom belongs to Allah, praise is due to Allah."></p>
            <p class="source" data-ar="رواه مسلم" data-en="Narrated by Muslim"></p>
            <button class="counter-btn" onclick="count(this, 1)">1</button>
        </div>
    </div>

    <div id="sleeping" class="dhikr-section">
        <div class="card">
            <p class="text" data-ar="بِاسْمِكَ رَبِّي وَضَعْتُ جَنْبِي، وَبِكَ أَرْفَعُهُ، فَإِنْ أَمْسَكْتَ نَفْسِي فَارْحَمْهَا." data-en="In Your name my Lord, I lie down and by Your glory I rise. If You take my soul, have mercy on it."></p>
            <p class="source" data-ar="البخاري ومسلم" data-en="Al-Bukhari & Muslim"></p>
            <button class="counter-btn" onclick="count(this, 1)">1</button>
        </div>
    </div>

    <div id="general" class="dhikr-section">
        <div class="card">
            <p class="text" data-ar="سُبْحَانَ اللَّهِ وَبِحَمْدِهِ." data-en="Glory be to Allah and praise is due to Him."></p>
            <p class="source" data-ar="مئة مرة تحط الخطايا" data-en="100 times wipes away sins"></p>
            <button class="counter-btn" onclick="count(this, 100)">100</button>
        </div>
        <div class="card">
            <p class="text" data-ar="لَا حَوْلَ وَلَا قُوَّةَ إِلَّا بِاللَّهِ." data-en="There is no might nor power except with Allah."></p>
            <p class="source" data-ar="كنز من كنوز الجنة" data-en="A treasure from Paradise"></p>
            <button class="counter-btn" onclick="count(this, 33)">33</button>
        </div>
    </div>

    <script>
        let currentLang = 'ar';

        const uiTexts = {
            ar: {
                title: "أذكار المسلم", sub: "أذكارك اليومية في مكان واحد", settings: "الإعدادات ⚙️",
                lang: "اللغة", theme: "المظهر", close: "حفظ وإغلاق",
                morning: "الصباح", evening: "المساء", sleeping: "النوم", general: "أذكار عامة",
                remaining: "المتبقي: ", done: "تم بنجاح ✓"
            },
            en: {
                title: "Muslim Azkar", sub: "Your daily remembrances in one place", settings: "Settings ⚙️",
                lang: "Language", theme: "Theme", close: "Save & Close",
                morning: "Morning", evening: "Evening", sleeping: "Sleeping", general: "General",
                remaining: "Remaining: ", done: "Completed ✓"
            }
        };

        function toggleSettings() {
            const modal = document.getElementById('settingsModal');
            modal.style.display = (modal.style.display === 'flex') ? 'none' : 'flex';
        }

        function switchTab(tabId) {
            document.querySelectorAll('.dhikr-section').forEach(sec => sec.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            document.getElementById(tabId).classList.add('active');
            event.currentTarget.classList.add('active');
        }

        function count(btn, max) {
            if (btn.classList.contains('done')) return;
            let currentText = btn.innerText.replace(/[^0-9]/g, '');
            let current = currentText === "" ? max : parseInt(currentText);
            
            if (current > 1) {
                current--;
                btn.innerText = uiTexts[currentLang].remaining + current;
            } else {
                btn.classList.add('done');
                btn.innerText = uiTexts[currentLang].done;
            }
        }

        function changeTheme() {
            const theme = document.getElementById('themeSelect').value;
            if (theme === 'dark') {
                document.documentElement.setAttribute('data-theme', 'dark');
            } else {
                document.documentElement.removeAttribute('data-theme');
            }
        }

        function changeLanguage() {
            currentLang = document.getElementById('langSelect').value;
            const html = document.documentElement;
            
            if (currentLang === 'en') {
                html.setAttribute('lang', 'en');
                html.setAttribute('dir', 'ltr');
            } else {
                html.setAttribute('lang', 'ar');
                html.setAttribute('dir', 'rtl');
            }
            updateDOMTexts();
        }

        function updateDOMTexts() {
            const t = uiTexts[currentLang];
            document.getElementById('mainTitle').innerText = t.title;
            document.getElementById('mainSub').innerText = t.sub;
            document.getElementById('settingsTitle').innerText = t.settings;
            document.getElementById('langLabel').innerText = t.lang;
            document.getElementById('themeLabel').innerText = t.theme;
            document.getElementById('closeBtn').innerText = t.close;
            document.getElementById('tabMorning').innerText = t.morning;
            document.getElementById('tabEvening').innerText = t.evening;
            document.getElementById('tabSleeping').innerText = t.sleeping;
            document.getElementById('tabGeneral').innerText = t.general;

            // تحديث نصوص الأذكار والمصادر والعدادات بناءً على اللغة المحددة
            document.querySelectorAll('[data-ar]').forEach(el => {
                el.innerText = el.getAttribute('data-' + currentLang);
            });

            document.querySelectorAll('.counter-btn').forEach(btn => {
                if (!btn.classList.contains('done')) {
                    let num = btn.innerText.replace(/[^0-9]/g, '');
                    btn.innerText = t.remaining + (num || btn.getAttribute('onclick').match(/\d+/)[0]);
                } else {
                    btn.innerText = t.done;
                }
            });
        }

        // تشغيل التهيئة عند فتح الصفحة لأول مرة
        updateDOMTexts();
    </script>
</body>
</html>
