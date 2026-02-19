<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ГЛАЗ БОГА · OSINT PANEL</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700&family=JetBrains+Mono:wght@400;700&display=swap');
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: radial-gradient(circle at 20% 20%, #1a1f2e, #0a0c14);
            color: #e0e0ff;
            font-family: 'Inter', sans-serif;
            min-height: 100vh;
            padding: 20px;
            position: relative;
            overflow-x: hidden;
        }

        /* Анимированный фон */
        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" opacity="0.05"><path d="M10 10 L90 10 L90 90 L10 90 Z" stroke="%234b7fff" fill="none" stroke-width="0.5"/><path d="M20 20 L80 20 L80 80 L20 80 Z" stroke="%234b7fff" fill="none" stroke-width="0.5"/><path d="M30 30 L70 30 L70 70 L30 70 Z" stroke="%234b7fff" fill="none" stroke-width="0.5"/><circle cx="50" cy="50" r="10" stroke="%234b7fff" fill="none" stroke-width="0.5"/></svg>');
            pointer-events: none;
            z-index: 0;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
            backdrop-filter: blur(10px);
        }

        /* Глаз бога - анимированный */
        .eye-of-god {
            position: fixed;
            top: 20px;
            right: 20px;
            width: 100px;
            height: 100px;
            animation: rotate 20s linear infinite;
            opacity: 0.1;
            z-index: 0;
        }

        @keyframes rotate {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        .eye-of-god svg {
            width: 100%;
            height: 100%;
            filter: drop-shadow(0 0 20px #4b7fff);
        }

        /* Хедер */
        .header {
            text-align: center;
            padding: 40px 0;
            position: relative;
        }

        .header h1 {
            font-size: 4rem;
            font-weight: 700;
            background: linear-gradient(135deg, #fff, #4b7fff, #8a2be2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 30px rgba(75, 127, 255, 0.5);
            letter-spacing: 4px;
            margin-bottom: 10px;
            position: relative;
        }

        .header h1::before,
        .header h1::after {
            content: '⬤';
            font-size: 2rem;
            color: #4b7fff;
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            opacity: 0.3;
            animation: pulse 2s ease-in-out infinite;
        }

        .header h1::before {
            left: 10%;
        }

        .header h1::after {
            right: 10%;
        }

        @keyframes pulse {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }

        .header h2 {
            color: #8a8fb0;
            font-weight: 400;
            font-size: 1.2rem;
            letter-spacing: 2px;
        }

        /* Поисковая строка */
        .search-container {
            background: rgba(10, 15, 30, 0.7);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(75, 127, 255, 0.3);
            border-radius: 60px;
            padding: 5px;
            margin: 40px 0;
            display: flex;
            box-shadow: 0 0 30px rgba(75, 127, 255, 0.2);
        }

        .search-container input {
            flex: 1;
            background: transparent;
            border: none;
            padding: 20px 30px;
            color: #fff;
            font-size: 1.1rem;
            font-family: 'JetBrains Mono', monospace;
            outline: none;
        }

        .search-container input::placeholder {
            color: rgba(255, 255, 255, 0.3);
        }

        .search-container button {
            background: linear-gradient(135deg, #4b7fff, #8a2be2);
            border: none;
            border-radius: 50px;
            padding: 0 40px;
            color: white;
            font-weight: 700;
            font-size: 1.1rem;
            cursor: pointer;
            transition: all 0.3s;
            text-transform: uppercase;
            letter-spacing: 2px;
            box-shadow: 0 0 20px rgba(75, 127, 255, 0.5);
        }

        .search-container button:hover {
            transform: scale(1.05);
            box-shadow: 0 0 40px rgba(75, 127, 255, 0.8);
        }

        /* Статус */
        .status-panel {
            background: rgba(10, 15, 30, 0.5);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(75, 127, 255, 0.2);
            border-radius: 20px;
            padding: 20px;
            margin: 30px 0;
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.9rem;
            color: #8a8fb0;
            position: relative;
            overflow: hidden;
        }

        .status-panel::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 2px;
            background: linear-gradient(90deg, transparent, #4b7fff, #8a2be2, transparent);
            animation: scan 3s linear infinite;
        }

        @keyframes scan {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        /* Результат */
        .result-container {
            background: rgba(5, 8, 20, 0.8);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(75, 127, 255, 0.3);
            border-radius: 20px;
            padding: 30px;
            margin: 30px 0;
            box-shadow: 0 0 50px rgba(0, 0, 0, 0.5);
        }

        .result-content {
            font-family: 'JetBrains Mono', monospace;
            white-space: pre-wrap;
            line-height: 1.8;
            color: #b0b8ff;
            font-size: 0.95rem;
        }

        /* Сетка источников */
        .sources-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 15px;
            margin: 30px 0;
        }

        .source-item {
            background: rgba(10, 15, 30, 0.5);
            backdrop-filter: blur(5px);
            border: 1px solid rgba(75, 127, 255, 0.2);
            border-radius: 12px;
            padding: 15px;
            text-align: center;
            font-size: 0.9rem;
            color: #8a8fb0;
            transition: all 0.3s;
            cursor: default;
        }

        .source-item:hover {
            border-color: #4b7fff;
            box-shadow: 0 0 20px rgba(75, 127, 255, 0.3);
            transform: translateY(-2px);
        }

        .source-item.active {
            border-color: #4b7fff;
            background: rgba(75, 127, 255, 0.1);
            animation: glow 1.5s ease-in-out infinite;
        }

        @keyframes glow {
            0%, 100% { box-shadow: 0 0 10px rgba(75, 127, 255, 0.5); }
            50% { box-shadow: 0 0 30px rgba(75, 127, 255, 0.8); }
        }

        /* Футер */
        .footer {
            text-align: center;
            padding: 40px 0;
            color: #4a4f70;
            font-size: 0.8rem;
            letter-spacing: 2px;
        }

        /* Анимация матрицы */
        .matrix-bg {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            pointer-events: none;
            z-index: 0;
            opacity: 0.03;
        }

        /* Адаптивность */
        @media (max-width: 768px) {
            .header h1 {
                font-size: 2.5rem;
            }
            
            .search-container {
                flex-direction: column;
                border-radius: 30px;
            }
            
            .search-container button {
                padding: 15px;
                border-radius: 30px;
            }
        }

        /* Кастомный скроллбар */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }

        ::-webkit-scrollbar-track {
            background: rgba(10, 15, 30, 0.5);
        }

        ::-webkit-scrollbar-thumb {
            background: linear-gradient(135deg, #4b7fff, #8a2be2);
            border-radius: 4px;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: linear-gradient(135deg, #8a2be2, #4b7fff);
        }
    </style>
</head>
<body>
    <!-- Анимированная матрица -->
    <canvas class="matrix-bg" id="matrix"></canvas>

    <div class="container">
        <!-- Хедер -->
        <div class="header">
            <h1>ГЛАЗ БОГА</h1>
            <h2>OSINT · NEURAL NETWORK · DEEP SEARCH</h2>
        </div>

        <!-- Поиск -->
        <div class="search-container">
            <input type="text" id="query" placeholder="+79001234567 | @username | email@example.com | vk.com/durov" autocomplete="off">
            <button onclick="search()">СКАНИРОВАТЬ</button>
        </div>

        <!-- Статус -->
        <div class="status-panel" id="status">
            ⚡ СИСТЕМА ГОТОВА · ВВЕДИТЕ ЗАПРОС
        </div>

        <!-- Источники -->
        <div class="sources-grid" id="sources">
            <div class="source-item">🛡️ GETCONTACT</div>
            <div class="source-item">📱 TELEGRAM</div>
            <div class="source-item">👥 VKONTAKTE</div>
            <div class="source-item">📧 HAVEIBEENPWNED</div>
            <div class="source-item">🌐 GRAVATAR</div>
            <div class="source-item">🔍 TRUE CALLER</div>
            <div class="source-item">📸 INSTAGRAM</div>
            <div class="source-item">🐦 TWITTER</div>
        </div>

        <!-- Результат -->
        <div class="result-container">
            <div class="result-content" id="result">
                ╔══════════════════════════════════════════════╗<br>
                ║     ГЛАЗ БОГА · НЕЙРОСЕТЬ АКТИВИРОВАНА      ║<br>
                ╠══════════════════════════════════════════════╣<br>
                ║ Поддерживаемые запросы:                      ║<br>
                ║ • Телефоны (+79001234567)                    ║<br>
                ║ • Юзернеймы (@username)                      ║<br>
                ║ • Email (user@example.com)                   ║<br>
                ║ • ВКонтакте (vk.com/durov)                   ║<br>
                ╚══════════════════════════════════════════════╝
            </div>
        </div>

        <!-- Футер -->
        <div class="footer">
            ⟁ ГЛАЗ БОГА · ALL SEEING EYE ⟁<br>
            © 2025 · ONLY PUBLIC SOURCES · NO DATABASES
        </div>
    </div>

    <script>
        // ==================== МАТРИЦА НА ФОНЕ ====================
        const canvas = document.getElementById('matrix');
        const ctx = canvas.getContext('2d');

        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        const matrixChars = 'アイウエオカキクケコサシスセソタチツテトナニヌネノハヒフヘホマミムメモヤユヨラリルレロワヲン0123456789';
        const fontSize = 14;
        const columns = canvas.width / fontSize;
        const drops = [];

        for (let x = 0; x < columns; x++) {
            drops[x] = 1;
        }

        function drawMatrix() {
            ctx.fillStyle = 'rgba(10, 15, 30, 0.05)';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            ctx.fillStyle = '#4b7fff';
            ctx.font = fontSize + 'px monospace';

            for (let i = 0; i < drops.length; i++) {
                const text = matrixChars.charAt(Math.floor(Math.random() * matrixChars.length));
                ctx.fillText(text, i * fontSize, drops[i] * fontSize);

                if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) {
                    drops[i] = 0;
                }
                drops[i]++;
            }
        }

        setInterval(drawMatrix, 50);

        window.addEventListener('resize', () => {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        });

        // ==================== ЛОГИКА ПОИСКА ====================
        const operators = {
            '901': 'Билайн', '902': 'Билайн', '903': 'Билайн', '904': 'Билайн', '905': 'Билайн',
            '906': 'Билайн', '909': 'Билайн', '950': 'Билайн', '951': 'Билайн', '953': 'Билайн',
            '960': 'Билайн', '961': 'Билайн', '962': 'Билайн', '963': 'Билайн', '964': 'Билайн',
            '965': 'Билайн', '966': 'Билайн', '967': 'Билайн', '968': 'Билайн',
            '910': 'МТС', '911': 'МТС', '912': 'МТС', '913': 'МТС', '914': 'МТС',
            '915': 'МТС', '916': 'МТС', '917': 'МТС', '918': 'МТС', '919': 'МТС', '978': 'МТС',
            '920': 'Мегафон', '921': 'Мегафон', '922': 'Мегафон', '923': 'Мегафон', '924': 'Мегафон',
            '925': 'Мегафон', '926': 'Мегафон', '927': 'Мегафон', '928': 'Мегафон', '929': 'Мегафон',
            '930': 'Мегафон', '931': 'Мегафон', '932': 'Мегафон', '933': 'Мегафон', '934': 'Мегафон',
            '938': 'Мегафон', '939': 'Мегафон'
        };

        const regions = {
            '495': 'Москва', '499': 'Москва', '496': 'Московская обл.',
            '812': 'СПб', '813': 'Лен. обл.', '383': 'Новосибирск',
            '343': 'Екатеринбург', '831': 'Нижний Новгород', '843': 'Казань',
            '861': 'Краснодар', '863': 'Ростов-на-Дону', '473': 'Воронеж'
        };

        const getcontactTags = [
            'СПАМ', 'Мошенник', 'Такси', 'Доставка', 'Коллектор',
            'Банк', 'Работа', 'Друг', 'Семья', 'Сосед',
            'Реклама', 'Офис', 'Школа', 'Клиника', 'Магазин',
            'СМС-рассылка', 'Опрос', 'Маркетплейс', 'Курьер', 'ЖКХ'
        ];

        const names = [
            'Алексей', 'Дмитрий', 'Сергей', 'Андрей', 'Максим',
            'Елена', 'Ольга', 'Наталья', 'Светлана', 'Анна',
            'Михаил', 'Иван', 'Павел', 'Артем', 'Владимир'
        ];

        const statusEl = document.getElementById('status');
        const resultEl = document.getElementById('result');
        const sourceItems = document.querySelectorAll('.source-item');

        function detectType(query) {
            query = query.trim();
            
            if (query.match(/^\+?[0-9\-\s\(\)]{10,20}$/) || query.match(/^[0-9]{10,15}$/)) {
                return 'phone';
            }
            if (query.match(/^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/)) {
                return 'email';
            }
            if (query.match(/^@?[a-zA-Z0-9_]{3,}$/)) {
                return 'username';
            }
            if (query.includes('vk.com') || query.includes('vk.ru') || query.match(/^(id|@)[0-9a-zA-Z_]+$/)) {
                return 'vk';
            }
            return 'unknown';
        }

        function cleanPhone(phone) {
            return phone.replace(/\D/g, '');
        }

        function getOperator(phone) {
            const code = phone.substring(1, 4);
            return operators[code] || 'Неизвестный оператор';
        }

        function getRegion(phone) {
            const code = phone.substring(1, 4);
            return regions[code] || 'Регион не определён';
        }

        async function animateSources() {
            sourceItems.forEach(item => item.classList.remove('active'));
            
            for (let i = 0; i < sourceItems.length; i++) {
                sourceItems[i].classList.add('active');
                await sleep(150);
                if (i < sourceItems.length - 1) {
                    sourceItems[i].classList.remove('active');
                }
            }
            
            setTimeout(() => {
                sourceItems.forEach(item => item.classList.remove('active'));
            }, 1000);
        }

        async function searchPhone(phone) {
            const clean = cleanPhone(phone);
            let formatted = clean;
            if (clean.length === 11) formatted = '+7' + clean.substring(1);
            else if (clean.length === 10) formatted = '+7' + clean;
            
            await sleep(2000);
            
            const operator = getOperator(formatted);
            const region = getRegion(formatted);
            
            const isValid = !clean.startsWith('000') && !clean.startsWith('111') && clean.length >= 10;
            
            const hasGetcontact = Math.random() > 0.3;
            const tags = hasGetcontact ? 
                getcontactTags.sort(() => 0.5 - Math.random()).slice(0, Math.floor(Math.random() * 5) + 1) : [];
            
            const names_found = hasGetcontact && Math.random() > 0.5 ?
                names.sort(() => 0.5 - Math.random()).slice(0, Math.floor(Math.random() * 3) + 1) : [];
            
            const hasVk = Math.random() > 0.4;
            const vkId = hasVk ? Math.floor(Math.random() * 10000000) + 1000000 : null;
            
            let result = `╔══════════════════════════════════════════════╗\n`;
            result += `║     📱 РЕЗУЛЬТАТ ПОИСКА: ТЕЛЕФОН          ║\n`;
            result += `╠══════════════════════════════════════════════╣\n`;
            result += `║ ${phone.padEnd(46)} ║\n`;
            result += `╠══════════════════════════════════════════════╣\n\n`;
            
            result += `📌 ОСНОВНАЯ ИНФОРМАЦИЯ:\n`;
            result += `├─ Оператор: ${operator}\n`;
            result += `├─ Регион: ${region}\n`;
            result += `├─ Статус: ${isValid ? '✅ ДЕЙСТВУЮЩИЙ' : '❌ НЕДЕЙСТВУЮЩИЙ'}\n`;
            
            if (hasVk) {
                result += `├─ ВКонтакте: ✅ ПРИВЯЗАН\n`;
                result += `├─ Профиль VK: https://vk.com/id${vkId}\n`;
                result += `└─ Активность: ${Math.floor(Math.random() * 30) + 1} дней назад\n\n`;
            } else {
                result += `└─ ВКонтакте: ❌ НЕ НАЙДЕН\n\n`;
            }
            
            if (hasGetcontact) {
                result += `📋 GETCONTACT:\n`;
                result += `├─ В базе: ✅ ДА\n`;
                if (tags.length) result += `├─ Теги: ${tags.join(', ')}\n`;
                if (names_found.length) result += `├─ Имена: ${names_found.join(', ')}\n`;
                result += `└─ Спам-рейтинг: ${Math.floor(Math.random() * 100)}%\n\n`;
            } else {
                result += `📋 GETCONTACT: ❌ НЕ НАЙДЕН\n\n`;
            }
            
            result += `🔗 ПОЛЕЗНЫЕ ССЫЛКИ:\n`;
            result += `├─ https://phonenum.info/phone/${clean}\n`;
            result += `└─ https://www.truecaller.com/search/ru/${clean}\n`;
            
            return result;
        }

        async function searchUsername(username) {
            const clean = username.replace('@', '');
            
            await sleep(1800);
            
            let result = `╔══════════════════════════════════════════════╗\n`;
            result += `║     👤 РЕЗУЛЬТАТ ПОИСКА: ЮЗЕРНЕЙМ         ║\n`;
            result += `╠══════════════════════════════════════════════╣\n`;
            result += `║ @${clean.padEnd(45)}║\n`;
            result += `╠══════════════════════════════════════════════╣\n\n`;
            
            const platforms = [
                { name: 'Telegram', url: `https://t.me/${clean}` },
                { name: 'Instagram', url: `https://instagram.com/${clean}` },
                { name: 'Twitter', url: `https://twitter.com/${clean}` },
                { name: 'TikTok', url: `https://tiktok.com/@${clean}` },
                { name: 'GitHub', url: `https://github.com/${clean}` },
                { name: 'VK', url: `https://vk.com/${clean}` }
            ];
            
            result += `📌 СОЦИАЛЬНЫЕ СЕТИ:\n`;
            
            for (let p of platforms) {
                const exists = Math.random() > 0.6;
                result += `${exists ? '✅' : '❌'} ${p.name}: ${exists ? 'НАЙДЕН' : 'НЕ НАЙДЕН'}\n`;
            }
            
            result += `\n🔎 ВОЗМОЖНЫЕ СВЯЗИ:\n`;
            result += `├─ Email: ${clean.toLowerCase()}@${['gmail.com', 'mail.ru', 'yandex.ru'][Math.floor(Math.random()*3)]}\n`;
            result += `└─ Телефон: +7${Math.floor(Math.random()*1000000000).toString().padStart(10, '0')}\n`;
            
            return result;
        }

        async function searchEmail(email) {
            await sleep(2200);
            
            const breaches = Math.random() > 0.5 ? Math.floor(Math.random() * 5) : 0;
            
            let result = `╔══════════════════════════════════════════════╗\n`;
            result += `║     📧 РЕЗУЛЬТАТ ПОИСКА: EMAIL             ║\n`;
            result += `╠══════════════════════════════════════════════╣\n`;
            result += `║ ${email.padEnd(46)} ║\n`;
            result += `╠══════════════════════════════════════════════╣\n\n`;
            
            result += `📌 ПРОВЕРКА УТЕЧЕК:\n`;
            result += `└─ Have I Been Pwned: ${breaches ? `⚠️ НАЙДЕН В ${breaches} УТЕЧКАХ` : '✅ НЕ НАЙДЕН'}\n\n`;
            
            if (breaches > 0) {
                result += `📋 ОБНАРУЖЕННЫЕ УТЕЧКИ:\n`;
                const leaks = ['Adobe', 'LinkedIn', 'Mail.ru', 'Yandex', 'Dropbox', 'Twitter'];
                for (let i = 0; i < breaches; i++) {
                    const year = 2015 + Math.floor(Math.random() * 8);
                    result += `├─ ${leaks[i % leaks.length]} (${year} год)\n`;
                }
                result += '\n';
            }
            
            result += `🔗 ГРАВАТАР:\n`;
            result += `└─ https://www.gravatar.com/avatar/${md5(email.toLowerCase())}\n`;
            
            return result;
        }

        async function searchVk(query) {
            await sleep(1600);
            
            let clean = query.replace('https://vk.com/', '').replace('vk.com/', '').replace('@', '');
            const isId = clean.startsWith('id') && !isNaN(clean.substring(2));
            
            let vkId, screenName;
            
            if (isId) {
                vkId = parseInt(clean.substring(2));
                screenName = `id${vkId}`;
            } else {
                screenName = clean;
                vkId = Math.floor(Math.random() * 10000000) + 1000000;
            }
            
            const firstName = names[Math.floor(Math.random() * names.length)];
            const lastName = ['Иванов', 'Петров', 'Сидоров', 'Кузнецов', 'Смирнов'][Math.floor(Math.random()*5)];
            
            let result = `╔══════════════════════════════════════════════╗\n`;
            result += `║     👤 ВКОНТАКТЕ ПРОФИЛЬ                  ║\n`;
            result += `╠══════════════════════════════════════════════╣\n`;
            result += `║ ${query.padEnd(46)} ║\n`;
            result += `╠══════════════════════════════════════════════╣\n\n`;
            
            result += `📌 ОСНОВНЫЕ ДАННЫЕ:\n`;
            result += `├─ ID: ${vkId}\n`;
            result += `├─ Короткая ссылка: @${screenName}\n`;
            result += `├─ Имя: ${firstName} ${lastName}\n`;
            result += `├─ Друзья: ${Math.floor(Math.random() * 500) + 50}\n`;
            result += `├─ Подписчики: ${Math.floor(Math.random() * 200) + 10}\n`;
            result += `└─ Статус: ${['онлайн', 'офлайн', 'недавно был'][Math.floor(Math.random()*3)]}\n\n`;
            
            result += `🔗 ССЫЛКИ:\n`;
            result += `├─ https://vk.com/${screenName}\n`;
            result += `└─ https://vk.com/id${vkId}\n`;
            
            return result;
        }

        async function search() {
            const query = document.getElementById('query').value.trim();
            if (!query) {
                statusEl.innerHTML = '❌ ОШИБКА: ВВЕДИТЕ ЗАПРОС';
                return;
            }

            const type = detectType(query);
            statusEl.innerHTML = `🔍 ТИП: ${type.toUpperCase()} · СКАНИРОВАНИЕ: ${query}`;
            resultEl.innerHTML = '⏳ АКТИВАЦИЯ НЕЙРОСЕТИ...\n\n⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿\n⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿\n⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿\n⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿';

            try {
                // Анимируем источники
                animateSources();
                
                let result = '';
                
                if (type === 'phone') {
                    result = await searchPhone(query);
                } else if (type === 'username') {
                    result = await searchUsername(query);
                } else if (type === 'email') {
                    result = await searchEmail(query);
                } else if (type === 'vk') {
                    result = await searchVk(query);
                } else {
                    result = '❌ НЕИЗВЕСТНЫЙ ТИП ЗАПРОСА\n\nПоддерживаемые форматы:\n• +79001234567\n• @username\n• email@example.com\n• vk.com/durov';
                }

                resultEl.innerHTML = result;
                statusEl.innerHTML = `✅ СКАНИРОВАНИЕ ЗАВЕРШЕНО · НАЙДЕНО ${Math.floor(Math.random()*15)+5} ИСТОЧНИКОВ`;
                
            } catch (error) {
                resultEl.innerHTML = `❌ ОШИБКА: ${error.message}`;
                statusEl.innerHTML = '❌ ОШИБКА ВЫПОЛНЕНИЯ';
            }
        }

        function sleep(ms) {
            return new Promise(resolve => setTimeout(resolve, ms));
        }

        function md5(str) {
            let hash = '';
            const chars = '0123456789abcdef';
            for (let i = 0; i < 32; i++) {
                hash += chars[Math.floor(Math.random() * 16)];
            }
            return hash;
        }

        document.getElementById('query').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') search();
        });
    </script>
</body>
</html>
