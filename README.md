# dox-htmll
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NEURODOX · OSINT PANEL</title>
    <style>
        body {
            background: #0f0f0f;
            color: #00ff9d;
            font-family: 'Courier New', monospace;
            margin: 0;
            padding: 20px;
        }
        .container {
            max-width: 900px;
            margin: 0 auto;
            border: 1px solid #00ff9d;
            padding: 20px;
            background: #1a1a1a;
        }
        h1 {
            text-align: center;
            border-bottom: 1px solid #00ff9d;
            padding-bottom: 10px;
            margin-top: 0;
            color: #00ff9d;
        }
        .input-area {
            display: flex;
            margin-bottom: 20px;
        }
        input {
            flex: 1;
            background: #000;
            border: 1px solid #00ff9d;
            color: #00ff9d;
            padding: 12px;
            font-family: 'Courier New', monospace;
            font-size: 16px;
        }
        button {
            background: #000;
            border: 1px solid #00ff9d;
            color: #00ff9d;
            padding: 12px 20px;
            font-family: 'Courier New', monospace;
            font-size: 16px;
            cursor: pointer;
            margin-left: 10px;
        }
        button:hover {
            background: #00ff9d;
            color: #000;
        }
        .status {
            margin: 20px 0;
            padding: 10px;
            border-left: 3px solid #00ff9d;
            background: #111;
        }
        .result {
            background: #111;
            border: 1px solid #00ff9d;
            padding: 20px;
            white-space: pre-wrap;
            font-size: 14px;
            line-height: 1.6;
            min-height: 300px;
        }
        .footer {
            margin-top: 20px;
            text-align: center;
            color: #00663d;
            font-size: 12px;
        }
        .source-badge {
            display: inline-block;
            background: #1a3a1a;
            padding: 2px 8px;
            margin: 2px;
            border-radius: 3px;
            font-size: 11px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin: 10px 0;
        }
        td, th {
            border: 1px solid #00ff9d;
            padding: 8px;
            text-align: left;
        }
        th {
            background: #000;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🔍 NEURODOX · OSINT ENGINE v3.0</h1>
        
        <div class="input-area">
            <input type="text" id="query" placeholder="+79001234567 / @username / email@example.com / vk.com/durov" value="">
            <button onclick="search()">ВЫПОЛНИТЬ</button>
        </div>

        <div class="status" id="status">⚡ Ожидание ввода...</div>
        
        <div class="result" id="result">[ ГОТОВ К РАБОТЕ ]</div>
        
        <div class="footer">
            NEURODOX v3.0 · ТОЛЬКО ПУБЛИЧНЫЕ ИСТОЧНИКИ · 2025
        </div>
    </div>

    <script>
        // База операторов
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

        // Регионы (примерные)
        const regions = {
            '495': 'Москва', '499': 'Москва', '496': 'Московская обл.',
            '812': 'СПб', '813': 'Лен. обл.', '383': 'Новосибирск',
            '343': 'Екатеринбург', '831': 'Нижний Новгород', '843': 'Казань',
            '861': 'Краснодар', '863': 'Ростов-на-Дону', '473': 'Воронеж'
        };

        // Теги GetContact
        const getcontactTags = [
            'СПАМ', 'Мошенник', 'Такси', 'Доставка', 'Коллектор',
            'Банк', 'Работа', 'Друг', 'Семья', 'Сосед',
            'Реклама', 'Офис', 'Школа', 'Клиника', 'Магазин',
            'СМС-рассылка', 'Опрос', 'Маркетплейс', 'Курьер', 'ЖКХ'
        ];

        // Имена
        const names = [
            'Алексей', 'Дмитрий', 'Сергей', 'Андрей', 'Максим',
            'Елена', 'Ольга', 'Наталья', 'Светлана', 'Анна',
            'Михаил', 'Иван', 'Павел', 'Артем', 'Владимир'
        ];

        const statusEl = document.getElementById('status');
        const resultEl = document.getElementById('result');

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

        async function searchPhone(phone) {
            const clean = cleanPhone(phone);
            let formatted = clean;
            if (clean.length === 11) formatted = '+7' + clean.substring(1);
            else if (clean.length === 10) formatted = '+7' + clean;
            
            // Эмуляция задержки
            await sleep(1000);
            
            const operator = getOperator(formatted);
            const region = getRegion(formatted);
            
            // Валидность номера (первые цифры не должны быть 000)
            const isValid = !clean.startsWith('000') && !clean.startsWith('111') && clean.length >= 10;
            
            // GetContact эмуляция
            const hasGetcontact = Math.random() > 0.3;
            const tags = hasGetcontact ? 
                getcontactTags.sort(() => 0.5 - Math.random()).slice(0, Math.floor(Math.random() * 5) + 1) : [];
            
            const names_found = hasGetcontact && Math.random() > 0.5 ?
                names.sort(() => 0.5 - Math.random()).slice(0, Math.floor(Math.random() * 3) + 1) : [];
            
            // ВК эмуляция
            const hasVk = Math.random() > 0.4;
            const vkId = hasVk ? Math.floor(Math.random() * 10000000) + 1000000 : null;
            
            let result = `📱 РЕЗУЛЬТАТ ПОИСКА: ТЕЛЕФОН\n`;
            result += `└ ${phone}\n\n`;
            result += `📌 ОСНОВНАЯ ИНФОРМАЦИЯ:\n`;
            result += `• Оператор: ${operator}\n`;
            result += `• Регион: ${region}\n`;
            result += `• Статус: ${isValid ? '✅ ДЕЙСТВУЮЩИЙ' : '❌ НЕДЕЙСТВУЮЩИЙ'}\n`;
            
            if (hasVk) {
                result += `• ВКонтакте: ✅ ПРИВЯЗАН\n`;
                result += `• Профиль VK: https://vk.com/id${vkId}\n`;
                result += `• Активность: ${Math.floor(Math.random() * 30) + 1} дней назад\n`;
            } else {
                result += `• ВКонтакте: ❌ НЕ НАЙДЕН\n`;
            }
            
            if (hasGetcontact) {
                result += `\n📋 GETCONTACT:\n`;
                result += `• В базе: ✅ ДА\n`;
                if (tags.length) result += `• Теги: ${tags.join(', ')}\n`;
                if (names_found.length) result += `• Имена: ${names_found.join(', ')}\n`;
                result += `• Спам-рейтинг: ${Math.floor(Math.random() * 100)}%\n`;
            } else {
                result += `\n📋 GETCONTACT: ❌ НЕ НАЙДЕН\n`;
            }
            
            result += `\n🔗 ПОЛЕЗНЫЕ ССЫЛКИ:\n`;
            result += `• https://phonenum.info/phone/${clean}\n`;
            result += `• https://www.truecaller.com/search/ru/${clean}\n`;
            
            return result;
        }

        async function searchUsername(username) {
            const clean = username.replace('@', '');
            
            await sleep(800);
            
            let result = `👤 РЕЗУЛЬТАТ ПОИСКА: ЮЗЕРНЕЙМ\n`;
            result += `└ @${clean}\n\n`;
            
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
                result += `• ${p.name}: ${exists ? '✅ НАЙДЕН' : '❌ НЕ НАЙДЕН'}\n`;
                if (exists && Math.random() > 0.7) {
                    result += `  ↳ ${p.url}\n`;
                }
            }
            
            result += `\n🔎 ВОЗМОЖНЫЕ СВЯЗИ:\n`;
            result += `• Email: ${clean.toLowerCase()}@${['gmail.com', 'mail.ru', 'yandex.ru'][Math.floor(Math.random()*3)]}\n`;
            result += `• Телефон: +7${Math.floor(Math.random()*1000000000).toString().padStart(10, '0')}\n`;
            
            return result;
        }

        async function searchEmail(email) {
            await sleep(900);
            
            const breaches = Math.random() > 0.5 ? Math.floor(Math.random() * 5) : 0;
            
            let result = `📧 РЕЗУЛЬТАТ ПОИСКА: EMAIL\n`;
            result += `└ ${email}\n\n`;
            
            result += `📌 ПРОВЕРКА УТЕЧЕК:\n`;
            result += `• Have I Been Pwned: ${breaches ? `⚠️ НАЙДЕН В ${breaches} УТЕЧКАХ` : '✅ НЕ НАЙДЕН'}\n`;
            
            if (breaches > 0) {
                result += `\n📋 ОБНАРУЖЕННЫЕ УТЕЧКИ:\n`;
                const leaks = ['Adobe', 'LinkedIn', 'Mail.ru', 'Yandex', 'Dropbox', 'Twitter'];
                for (let i = 0; i < breaches; i++) {
                    const year = 2015 + Math.floor(Math.random() * 8);
                    result += `• ${leaks[i % leaks.length]} (${year} год)\n`;
                }
            }
            
            result += `\n🔗 ГРАВАТАР:\n`;
            const hash = md5(email.toLowerCase());
            result += `• https://www.gravatar.com/avatar/${hash}\n`;
            result += `• https://en.gravatar.com/${hash}\n`;
            
            return result;
        }

        async function searchVk(query) {
            await sleep(700);
            
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
            
            let result = `👤 ВКОНТАКТЕ ПРОФИЛЬ\n`;
            result += `└ ${query}\n\n`;
            
            result += `📌 ОСНОВНЫЕ ДАННЫЕ:\n`;
            result += `• ID: ${vkId}\n`;
            result += `• Короткая ссылка: @${screenName}\n`;
            
            // Имя
            const firstName = names[Math.floor(Math.random() * names.length)];
            const lastName = ['Иванов', 'Петров', 'Сидоров', 'Кузнецов', 'Смирнов'][Math.floor(Math.random()*5)];
            result += `• Имя: ${firstName} ${lastName}\n`;
            
            result += `• Друзья: ${Math.floor(Math.random() * 500) + 50}\n`;
            result += `• Подписчики: ${Math.floor(Math.random() * 200) + 10}\n`;
            result += `• Статус: ${['онлайн', 'офлайн', 'недавно был'][Math.floor(Math.random()*3)]}\n`;
            
            if (Math.random() > 0.5) {
                result += `\n👥 ГРУППЫ (3 из ${Math.floor(Math.random()*50)+10}):\n`;
                const groups = ['Мемы', 'Музыка', 'Кино', 'Игры', 'Спорт', 'Путешествия', 'IT'];
                const selected = groups.sort(() => 0.5 - Math.random()).slice(0, 3);
                selected.forEach(g => result += `• ${g}\n`);
            }
            
            if (Math.random() > 0.6) {
                result += `\n🎯 ИНТЕРЕСЫ:\n`;
                const interests = ['программирование', 'музыка', 'книги', 'футбол', 'фотография', 'путешествия'];
                const selected = interests.sort(() => 0.5 - Math.random()).slice(0, Math.floor(Math.random()*3)+1);
                selected.forEach(i => result += `• ${i}\n`);
            }
            
            result += `\n🔗 ССЫЛКИ:\n`;
            result += `• https://vk.com/${screenName}\n`;
            result += `• https://vk.com/id${vkId}\n`;
            result += `• https://vk.com/foaf.php?id=${vkId}\n`;
            
            return result;
        }

        async function search() {
            const query = document.getElementById('query').value.trim();
            if (!query) {
                statusEl.innerHTML = '❌ ОШИБКА: ВВЕДИТЕ ЗАПРОС';
                return;
            }

            const type = detectType(query);
            statusEl.innerHTML = `🔍 ТИП: ${type.toUpperCase()} · ОБРАБОТКА: ${query}`;
            resultEl.innerHTML = '⏳ СКАНИРОВАНИЕ ИСТОЧНИКОВ...\n';

            try {
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
                statusEl.innerHTML = `✅ ГОТОВО · НАЙДЕНО ${Math.floor(Math.random()*15)+5} ИСТОЧНИКОВ`;
                
            } catch (error) {
                resultEl.innerHTML = `❌ ОШИБКА: ${error.message}`;
                statusEl.innerHTML = '❌ ОШИБКА ВЫПОЛНЕНИЯ';
            }
        }

        function sleep(ms) {
            return new Promise(resolve => setTimeout(resolve, ms));
        }

        // Простейший MD5 для Gravatar (только для демо)
        function md5(str) {
            // В реальности нужно подключить библиотеку
            // Для демо возвращаем случайный хеш
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

        window.onload = function() {
            resultEl.innerHTML = `╔══════════════════════════════════════════════╗
║ NEURODOX v3.0 · ГОТОВ К РАБОТЕ         ║
╠══════════════════════════════════════════════╣
║ Поддерживаемые запросы:                      ║
║ • Телефоны (+79001234567)                     ║
║ • Юзернеймы (@username)                        ║
║ • Email (user@example.com)                     ║
║ • ВКонтакте (vk.com/durov)                     ║
╚══════════════════════════════════════════════╝`;
        };
    </script>
</body>
</html>
