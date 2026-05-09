<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Happy Decoder — расшифровка VPN-подписок Happ / VLESS / Sub</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: linear-gradient(135deg, #0b1120 0%, #0a0f1a 100%);
            font-family: 'Inter', system-ui, -apple-system, 'Segoe UI', 'Poppins', sans-serif;
            padding: 2rem 1rem;
            min-height: 100vh;
            color: #eef2ff;
        }

        /* main container */
        .decoder-container {
            max-width: 1400px;
            margin: 0 auto;
            background: rgba(15, 23, 42, 0.65);
            backdrop-filter: blur(16px);
            border-radius: 2rem;
            border: 1px solid rgba(56, 189, 248, 0.2);
            box-shadow: 0 25px 40px -15px rgba(0, 0, 0, 0.5);
            overflow: hidden;
        }

        /* header */
        .hero {
            padding: 1.8rem 2rem;
            background: rgba(2, 6, 23, 0.6);
            border-bottom: 1px solid rgba(56, 189, 248, 0.3);
        }
        .hero h1 {
            font-size: 2.2rem;
            font-weight: 700;
            background: linear-gradient(125deg, #ffffff, #7dd3fc, #38bdf8);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            display: inline-flex;
            align-items: center;
            gap: 12px;
        }
        .badge {
            background: #0f2f44;
            border-radius: 60px;
            padding: 4px 12px;
            font-size: 0.7rem;
            font-weight: 500;
            color: #a5f3fc;
            border: 1px solid #22d3ee60;
        }
        .subhead {
            margin-top: 10px;
            color: #9ab3d5;
            font-size: 0.85rem;
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            align-items: center;
        }

        /* main grid */
        .grid-main {
            display: flex;
            flex-wrap: wrap;
            gap: 1.8rem;
            padding: 2rem;
        }
        .input-panel {
            flex: 1.2;
            min-width: 320px;
        }
        .output-panel {
            flex: 2;
            min-width: 380px;
        }
        .glass-card {
            background: rgba(30, 41, 59, 0.55);
            backdrop-filter: blur(4px);
            border-radius: 1.5rem;
            border: 1px solid rgba(71, 85, 105, 0.5);
            padding: 1.4rem;
            transition: all 0.2s;
        }
        .card-title {
            font-weight: 500;
            margin-bottom: 1rem;
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 1rem;
            letter-spacing: 0.2px;
            color: #b9d0f8;
        }
        textarea {
            width: 100%;
            background: #0b1120;
            border: 1px solid #2d3a5e;
            border-radius: 1.2rem;
            padding: 1rem;
            color: #e2e8f0;
            font-family: 'JetBrains Mono', 'Fira Code', monospace;
            font-size: 0.8rem;
            resize: vertical;
            outline: none;
            transition: 0.2s;
        }
        textarea:focus {
            border-color: #38bdf8;
            box-shadow: 0 0 0 2px #38bdf830;
        }
        .btn-group {
            display: flex;
            gap: 12px;
            margin-top: 1.2rem;
            flex-wrap: wrap;
        }
        button {
            background: #1e2a44;
            border: none;
            padding: 0.5rem 1.2rem;
            border-radius: 2rem;
            font-weight: 500;
            font-size: 0.8rem;
            cursor: pointer;
            transition: 0.2s;
            color: #f0f4ff;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            border: 1px solid #3b4b6e;
        }
        button.primary {
            background: #0f2f44;
            border-color: #2c7da0;
            color: #b9f3ff;
            box-shadow: 0 0 8px rgba(0,180,255,0.2);
        }
        button.primary:hover {
            background: #1e4a6e;
            transform: translateY(-1px);
        }
        button:hover {
            background: #2f3d60;
            border-color: #5b7a9e;
        }

        /* result area */
        .result-header {
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            margin-bottom: 12px;
            flex-wrap: wrap;
        }
        .server-list {
            background: #050a15cc;
            border-radius: 1.2rem;
            padding: 0.8rem;
            max-height: 520px;
            overflow-y: auto;
            font-family: monospace;
        }
        .server-item {
            background: #0f1422b3;
            margin-bottom: 8px;
            padding: 10px 14px;
            border-radius: 1rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 8px;
            border-left: 3px solid #2dd4bf;
            transition: 0.1s;
        }
        .server-code {
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.75rem;
            word-break: break-all;
            color: #cbd5ff;
        }
        .copy-mini {
            background: #2d3b5a;
            padding: 4px 12px;
            border-radius: 30px;
            font-size: 0.7rem;
        }
        .footer-note {
            padding: 1rem 2rem;
            border-top: 1px solid rgba(56, 189, 248, 0.2);
            font-size: 0.7rem;
            color: #7e90b0;
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
        }
        .mode-tag {
            background: #0f212e;
            border-radius: 30px;
            padding: 2px 8px;
            font-size: 0.6rem;
            color: #7dd3fc;
        }
        ::-webkit-scrollbar {
            width: 5px;
        }
        ::-webkit-scrollbar-track {
            background: #101624;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb {
            background: #2f4b70;
            border-radius: 10px;
        }
        @media (max-width: 760px) {
            .grid-main { padding: 1rem; }
            .hero { padding: 1.2rem; }
        }
    </style>
</head>
<body>
<div class="decoder-container">
    <div class="hero">
        <div style="display: flex; justify-content: space-between; flex-wrap: wrap; gap: 12px;">
            <div>
                <h1>✨ Happy Decoder <span class="badge">happ://crypt* | VLESS | Sub</span></h1>
                <div class="subhead">
                    🔓 Расшифровка зашифрованных VPN-подписок Happ (crypt → RSA/ChaCha) + парсинг обычных sub-ссылок
                    <span class="mode-tag">автоопределение crypt → crypt5</span>
                </div>
            </div>
            <div id="statsLive" style="background:#0a0f1e; border-radius: 40px; padding: 6px 14px; font-size:0.75rem;">⚡ готов</div>
        </div>
    </div>

    <div class="grid-main">
        <!-- левая панель ввода -->
        <div class="input-panel">
            <div class="glass-card">
                <div class="card-title">
                    <span>📎 Ссылка подписки (happ://crypt* / sub:// / обычный URL)</span>
                </div>
                <textarea id="subInput" rows="6" placeholder="happ://crypt5:aHR0cHM6Ly9leGFtcGxlLmNvbS9zdWI/dG9rZW49WFhY
https://sub.whiteness.site/WK3QvAj_6qbG2Cmx
vless://... 
sub://https://vpn.example/sub">
                </textarea>
                <div class="btn-group">
                    <button id="decodeBtn" class="primary">🔓 Расшифровать & парсить серверы</button>
                    <button id="clearBtn">🗑 Очистить</button>
                    <button id="exampleBtn">📋 Примеры (happ + sub)</button>
                </div>
                <div style="margin-top: 14px; font-size:0.7rem; background:#00000030; border-radius: 1rem; padding: 8px;">
                    🧠 Поддерживаются: <strong>happ://crypt, crypt2...crypt5</strong> (base64 → расшифровка RSA/ChaCha симулирована, но реальное декодирование требует ключей — мы парсим прямую ссылку),
                    обычные <strong>sub://</strong> и любые ссылки <strong>vless://, vmess://, trojan://</strong>. Из подписки извлекаем VLESS-серверы и REALITY параметры.
                </div>
            </div>
        </div>

        <!-- правая панель результатов -->
        <div class="output-panel">
            <div class="glass-card" style="height: 100%; display: flex; flex-direction: column;">
                <div class="result-header">
                    <span>🌐 Извлечённые серверы (простой формат + VLESS-URI)</span>
                    <button id="copyAllBtn" style="background:#2d3b5a; padding: 4px 14px;">📋 Копировать всё</button>
                </div>
                <div id="serverListArea" class="server-list">
                    <div style="color:#8e9bb5; text-align: center; padding: 32px;">✨ Ожидание ссылки... вставьте happ://crypt* или sub://...</div>
                </div>
                <div id="infoCounter" style="margin-top: 8px; font-size: 0.7rem; text-align: right; color: #84a7d0;"></div>
            </div>
        </div>
    </div>
    <div class="footer-note">
        <span>⚡ Расшифровка happ://crypt* : извлекаем встроенную ссылку подписки (base64). Для полной дешифровки RSA/ChaCha потребовались бы приватные ключи, но сервис имитирует получение реальной конфигурации через прямой URL. <strong>Локальная работа, данные не отправляются.</strong></span>
        <span>🔹 поддерживаются sub. домены и любые VLESS/trojan строки</span>
    </div>
</div>

<script>
    (function() {
        // ---------- Универсальный парсер Happ / sub / VLESS ----------
        // Эмуляция расшифровки happ://crypt*: извлекаем зашифрованную часть, декодируем base64,
        // получаем URL реальной подписки. Потом загружаем данные через прокси? нет, CORS.
        // Но поскольку мы локальный инструмент, мы будем извлекать из happ-ссылки конечный url подписки
        // (в реальном Happy Decoder там происходит расшифровка, но для демонстрации мы покажем, как ссылка преобразуется в прямую подписку,
        // а также пользователь может вставить уже готовую raw sub-ссылку. Также мы сможем вытянуть серверы если пользователь вставит сам контент.
        // Дополнительно: сделаем парсинг уже готового текста подписки (например, если пользователь вставит конфиг).
        
        // Функция: из строки пытаемся извлечь все VLESS/VMess и другие прокси, а также прямые ссылки на sub.
        function extractAllServers(inputText) {
            let serversList = []; // уникальные строки серверов вида "host:port" или полные vless://
            if (!inputText || typeof inputText !== 'string') return serversList;
            
            const lines = inputText.split(/\r?\n/);
            for (let line of lines) {
                line = line.trim();
                if (!line) continue;
                
                // 1) если это happ://crypt* ссылка — расшифровываем до URL подписки
                if (line.match(/^happ:\/\/crypt\d*:/i)) {
                    let decodedSubUrl = decodeHappLink(line);
                    if (decodedSubUrl && decodedSubUrl.startsWith('http')) {
                        serversList.push(`[Subscription] ${decodedSubUrl}`);
                        // дополнительно: выводим как служебную строку, но также симулируем, что можно получить серверы, если загрузить
                        // Однако автоматически fetch нельзя из-за CORS. Но дадим пользователю подсказку.
                        serversList.push(`ℹ️ RAW подписка: ${decodedSubUrl} (скопируйте и вставьте содержимое в поле для парсинга VLESS)`);
                    }
                    continue;
                }
                
                // 2) поддержка sub:// ссылок (превращаем в реальный URL)
                if (line.startsWith('sub://')) {
                    let subUrl = line.slice(6);
                    if (subUrl.startsWith('http')) {
                        serversList.push(`[Sub-link] ${subUrl}`);
                    } else {
                        serversList.push(`[Sub] ${subUrl}`);
                    }
                    continue;
                }
                
                // 3) парсинг стандартных прокси-ссылок
                let parsed = parseProxyUri(line);
                if (parsed) {
                    if (!serversList.includes(parsed)) serversList.push(parsed);
                    continue;
                }
                
                // 4) если строка похожа на URL подписки (https://...) и не happ/sub, добавляем как ссылку на подписку
                if (line.startsWith('http://') || line.startsWith('https://')) {
                    serversList.push(`📡 Subscription source: ${line}`);
                    continue;
                }
                
                // 5) если строка является просто host:port
                if (/^[\w.-]+:\d{1,5}$/.test(line)) {
                    if (!serversList.includes(line)) serversList.push(line);
                }
            }
            return serversList;
        }
        
        // расшифровка happ://crypt* (эмуляция — извлечение base64 payload и декод)
        function decodeHappLink(happUrl) {
            try {
                // формат: happ://crypt5:base64data  или happ://crypt:base64data
                let match = happUrl.match(/^happ:\/\/crypt\d*:(.+)$/i);
                if (!match) return null;
                let b64payload = match[1];
                // удаляем возможные фрагменты #
                b64payload = b64payload.split('#')[0];
                let decoded = atob(b64payload);
                // в decoded может быть прямая ссылка подписки, либо конфиг JSON
                if (decoded.startsWith('http://') || decoded.startsWith('https://')) {
                    return decoded;
                } else {
                    // возможно внутри JSON. ищем url
                    try {
                        let json = JSON.parse(decoded);
                        if (json.url) return json.url;
                        if (json.subscription_url) return json.subscription_url;
                        return null;
                    } catch(e) {
                        // если внутри просто текст, который может быть ссылкой
                        let urlMatch = decoded.match(/(https?:\/\/[^\s]+)/);
                        if (urlMatch) return urlMatch[1];
                        return null;
                    }
                }
            } catch(e) { console.warn(e); return null; }
        }
        
        // универсальный парсер vless:// vmess:// trojan:// ss://
        function parseProxyUri(uri) {
            if (uri.startsWith('vless://')) {
                return parseVlessLike(uri);
            }
            if (uri.startsWith('trojan://')) {
                return parseTrojan(uri);
            }
            if (uri.startsWith('vmess://')) {
                return parseVmess(uri);
            }
            if (uri.startsWith('ss://')) {
                return parseShadowsocks(uri);
            }
            if (uri.startsWith('ssr://')) {
                return parseSSR(uri);
            }
            return null;
        }
        
        function parseVlessLike(url) {
            try {
                let raw = url.slice(8);
                let atIndex = raw.indexOf('@');
                if (atIndex === -1) return null;
                let afterAt = raw.substring(atIndex + 1);
                let hostPortPart = afterAt.split('?')[0].split('#')[0];
                let hp = hostPortPart.split(':');
                if (hp.length >= 2) {
                    let host = hp[0];
                    let port = hp[1];
                    let user = raw.substring(0, atIndex);
                    if (user && host && port) return `vless://${user}@${host}:${port}`;
                    return `${host}:${port}`;
                }
                return null;
            } catch(e) { return null; }
        }
        
        function parseTrojan(url) {
            try {
                let raw = url.slice(9);
                let atIndex = raw.indexOf('@');
                if (atIndex === -1) return null;
                let password = raw.substring(0, atIndex);
                let afterAt = raw.substring(atIndex + 1);
                let hostPort = afterAt.split('?')[0].split('#')[0];
                let [host, port] = hostPort.split(':');
                if (host && port) return `trojan://${password}@${host}:${port}`;
                return null;
            } catch(e) { return null; }
        }
        
        function parseVmess(url) {
            try {
                let b64part = url.slice(8);
                b64part = b64part.split('#')[0];
                let decoded = atob(b64part);
                let config = JSON.parse(decoded);
                let host = config.addr || config.host;
                let port = config.port;
                let id = config.id || '';
                if (host && port) return `vmess://${id}@${host}:${port}  (decoded)`;
                return null;
            } catch(e) { return null; }
        }
        
        function parseShadowsocks(url) {
            try {
                let b64part = url.slice(5);
                if (b64part.includes('@')) {
                    let [methodPass, hostPort] = b64part.split('@');
                    let [host, port] = hostPort.split(':');
                    if (host && port) return `ss://${methodPass}@${host}:${port}`;
                } else {
                    let clean = b64part.split('#')[0];
                    let decoded = atob(clean);
                    if (decoded.includes('@')) {
                        let [mp, hp] = decoded.split('@');
                        let [host, port] = hp.split(':');
                        if (host && port) return `ss://${mp}@${host}:${port}`;
                    }
                }
                return null;
            } catch(e) { return null; }
        }
        
        function parseSSR(url) {
            try {
                let b64 = url.slice(6);
                let decoded = atob(b64);
                let parts = decoded.split(':');
                if (parts.length >= 6) {
                    let server = parts[0];
                    let port = parts[1];
                    return `ssr://${server}:${port}`;
                }
                return null;
            } catch(e) { return null; }
        }
        
        // функция для рендеринга серверов (из ввода)
        function renderServers() {
            const rawText = document.getElementById('subInput').value;
            if (!rawText.trim()) {
                document.getElementById('serverListArea').innerHTML = '<div style="color:#8ba0c2; text-align:center; padding:36px;">💡 Введите happ://crypt*, sub:// или ссылку на подписку с VLESS</div>';
                document.getElementById('infoCounter').innerHTML = '';
                document.getElementById('statsLive').innerHTML = '⚡ ожидание ссылки';
                return;
            }
            
            let servers = extractAllServers(rawText);
            // фильтр пустых и дублей (сохраняем уникальность)
            servers = [...new Map(servers.map(s => [s, s])).values()];
            const container = document.getElementById('serverListArea');
            const counterSpan = document.getElementById('infoCounter');
            const statsSpan = document.getElementById('statsLive');
            
            if (servers.length === 0) {
                container.innerHTML = '<div style="color:#f28b82; text-align:center; padding:28px;">❌ Не удалось распознать серверы или ссылки. Проверьте формат (happ://crypt* / sub:// / vless://). Для подписок сперва получите конфигурационный текст.</div>';
                counterSpan.innerText = '🔍 не найдено элементов';
                statsSpan.innerHTML = '⚠️ 0 узлов';
                return;
            }
            
            let html = `<div style="margin-bottom: 12px;">✅ Найдено элементов: <strong>${servers.length}</strong> (серверы, ссылки, подписки)</div>`;
            servers.forEach((sv, idx) => {
                let escaped = escapeHtml(sv);
                html += `
                    <div class="server-item">
                        <div class="server-code" style="flex:1;">${idx+1}. ${escaped}</div>
                        <button class="copy-mini copy-single" data-value="${escapeAttr(sv)}">📋 Копировать</button>
                    </div>
                `;
            });
            container.innerHTML = html;
            counterSpan.innerText = `📡 итого уникальных записей: ${servers.length}`;
            statsSpan.innerHTML = `✨ распознано: ${servers.length}`;
            
            // привязать события кнопок
            document.querySelectorAll('.copy-single').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    let val = btn.getAttribute('data-value');
                    if (val) copyToClipboard(val);
                    showToast(`Скопировано: ${val.substring(0, 60)}`);
                });
            });
            window.currentServers = servers;
        }
        
        function escapeHtml(str) {
            return str.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;');
        }
        function escapeAttr(str) {
            return str.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;').replace(/"/g, '&quot;');
        }
        
        async function copyToClipboard(text) {
            try {
                await navigator.clipboard.writeText(text);
            } catch(err) {
                let ta = document.createElement('textarea');
                ta.value = text;
                document.body.appendChild(ta);
                ta.select();
                document.execCommand('copy');
                document.body.removeChild(ta);
            }
        }
        
        function showToast(msg) {
            let toast = document.getElementById('globalToast');
            if (!toast) {
                let div = document.createElement('div');
                div.id = 'globalToast';
                div.style.position = 'fixed';
                div.style.bottom = '25px';
                div.style.left = '50%';
                div.style.transform = 'translateX(-50%)';
                div.style.backgroundColor = '#0f172a';
                div.style.color = '#b9f3ff';
                div.style.padding = '10px 24px';
                div.style.borderRadius = '60px';
                div.style.fontSize = '0.8rem';
                div.style.zIndex = '9999';
                div.style.border = '1px solid #22d3ee';
                div.style.backdropFilter = 'blur(12px)';
                div.style.fontWeight = '500';
                document.body.appendChild(div);
                toast = div;
            }
            toast.innerText = msg;
            toast.style.opacity = '1';
            clearTimeout(window.toastTimeout);
            window.toastTimeout = setTimeout(() => {
                toast.style.opacity = '0';
            }, 2000);
        }
        
        function copyAll() {
            if (window.currentServers && window.currentServers.length) {
                let all = window.currentServers.join('\n');
                copyToClipboard(all);
                showToast(`📋 Скопировано ${window.currentServers.length} элементов`);
            } else {
                showToast("Нет данных для копирования");
            }
        }
        
        function clearAll() {
            document.getElementById('subInput').value = '';
            renderServers();
        }
        
        function setExamples() {
            document.getElementById('subInput').value = `happ://crypt5:aHR0cHM6Ly9zdWIud2hpdGVuZXNzLnNpdGUvV0szUXZBai82cWJHMkNteA==
https://sub.whiteness.site/WK3QvAj_6qbG2Cmx
vless://9f4e2d1c-3b2a-4c5d-8e7f-6a5b4c3d2e1f@lon.reality-server.com:443?encryption=none&security=reality&pbk=abcd#UK-REALITY
trojan://mySecurePass@trojan-vpn.xyz:443?security=tls
sub://https://vpn-example.com/sub/v1?token=abc123
vmess://eyJhZGQiOiJmci12bWVzcy5jbG91ZCIsInBvcnQiOjQ0MywiaWQiOiI2YzE5ZDU4OS0zMjE0LTQ3ODktYmRlZi0xMjM0NTY3ODkwIiwicHMiOiLms6LkuK3ml6AifQ==`;
            renderServers();
        }
        
        // инициализация
        window.currentServers = [];
        document.getElementById('decodeBtn').addEventListener('click', renderServers);
        document.getElementById('clearBtn').addEventListener('click', clearAll);
        document.getElementById('exampleBtn').addEventListener('click', setExamples);
        document.getElementById('copyAllBtn').addEventListener('click', copyAll);
        // предзаполнить демо-ссылкой (как на happy-decoder.cc)
        setTimeout(() => {
            if (!document.getElementById('subInput').value.trim()) {
                setExamples();
            } else {
                renderServers();
            }
        }, 100);
    })();
</script><!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Happy Decoder — расшифровка VPN-подписок Happ / VLESS / Sub</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: linear-gradient(135deg, #0b1120 0%, #0a0f1a 100%);
            font-family: 'Inter', system-ui, -apple-system, 'Segoe UI', 'Poppins', sans-serif;
            padding: 2rem 1rem;
            min-height: 100vh;
            color: #eef2ff;
        }

        /* main container */
        .decoder-container {
            max-width: 1400px;
            margin: 0 auto;
            background: rgba(15, 23, 42, 0.65);
            backdrop-filter: blur(16px);
            border-radius: 2rem;
            border: 1px solid rgba(56, 189, 248, 0.2);
            box-shadow: 0 25px 40px -15px rgba(0, 0, 0, 0.5);
            overflow: hidden;
        }

        /* header */
        .hero {
            padding: 1.8rem 2rem;
            background: rgba(2, 6, 23, 0.6);
            border-bottom: 1px solid rgba(56, 189, 248, 0.3);
        }
        .hero h1 {
            font-size: 2.2rem;
            font-weight: 700;
            background: linear-gradient(125deg, #ffffff, #7dd3fc, #38bdf8);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            display: inline-flex;
            align-items: center;
            gap: 12px;
        }
        .badge {
            background: #0f2f44;
            border-radius: 60px;
            padding: 4px 12px;
            font-size: 0.7rem;
            font-weight: 500;
            color: #a5f3fc;
            border: 1px solid #22d3ee60;
        }
        .subhead {
            margin-top: 10px;
            color: #9ab3d5;
            font-size: 0.85rem;
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            align-items: center;
        }

        /* main grid */
        .grid-main {
            display: flex;
            flex-wrap: wrap;
            gap: 1.8rem;
            padding: 2rem;
        }
        .input-panel {
            flex: 1.2;
            min-width: 320px;
        }
        .output-panel {
            flex: 2;
            min-width: 380px;
        }
        .glass-card {
            background: rgba(30, 41, 59, 0.55);
            backdrop-filter: blur(4px);
            border-radius: 1.5rem;
            border: 1px solid rgba(71, 85, 105, 0.5);
            padding: 1.4rem;
            transition: all 0.2s;
        }
        .card-title {
            font-weight: 500;
            margin-bottom: 1rem;
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 1rem;
            letter-spacing: 0.2px;
            color: #b9d0f8;
        }
        textarea {
            width: 100%;
            background: #0b1120;
            border: 1px solid #2d3a5e;
            border-radius: 1.2rem;
            padding: 1rem;
            color: #e2e8f0;
            font-family: 'JetBrains Mono', 'Fira Code', monospace;
            font-size: 0.8rem;
            resize: vertical;
            outline: none;
            transition: 0.2s;
        }
        textarea:focus {
            border-color: #38bdf8;
            box-shadow: 0 0 0 2px #38bdf830;
        }
        .btn-group {
            display: flex;
            gap: 12px;
            margin-top: 1.2rem;
            flex-wrap: wrap;
        }
        button {
            background: #1e2a44;
            border: none;
            padding: 0.5rem 1.2rem;
            border-radius: 2rem;
            font-weight: 500;
            font-size: 0.8rem;
            cursor: pointer;
            transition: 0.2s;
            color: #f0f4ff;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            border: 1px solid #3b4b6e;
        }
        button.primary {
            background: #0f2f44;
            border-color: #2c7da0;
            color: #b9f3ff;
            box-shadow: 0 0 8px rgba(0,180,255,0.2);
        }
        button.primary:hover {
            background: #1e4a6e;
            transform: translateY(-1px);
        }
        button:hover {
            background: #2f3d60;
            border-color: #5b7a9e;
        }

        /* result area */
        .result-header {
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            margin-bottom: 12px;
            flex-wrap: wrap;
        }
        .server-list {
            background: #050a15cc;
            border-radius: 1.2rem;
            padding: 0.8rem;
            max-height: 520px;
            overflow-y: auto;
            font-family: monospace;
        }
        .server-item {
            background: #0f1422b3;
            margin-bottom: 8px;
            padding: 10px 14px;
            border-radius: 1rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 8px;
            border-left: 3px solid #2dd4bf;
            transition: 0.1s;
        }
        .server-code {
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.75rem;
            word-break: break-all;
            color: #cbd5ff;
        }
        .copy-mini {
            background: #2d3b5a;
            padding: 4px 12px;
            border-radius: 30px;
            font-size: 0.7rem;
        }
        .footer-note {
            padding: 1rem 2rem;
            border-top: 1px solid rgba(56, 189, 248, 0.2);
            font-size: 0.7rem;
            color: #7e90b0;
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
        }
        .mode-tag {
            background: #0f212e;
            border-radius: 30px;
            padding: 2px 8px;
            font-size: 0.6rem;
            color: #7dd3fc;
        }
        ::-webkit-scrollbar {
            width: 5px;
        }
        ::-webkit-scrollbar-track {
            background: #101624;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb {
            background: #2f4b70;
            border-radius: 10px;
        }
        @media (max-width: 760px) {
            .grid-main { padding: 1rem; }
            .hero { padding: 1.2rem; }
        }
    </style>
</head>
<body>
<div class="decoder-container">
    <div class="hero">
        <div style="display: flex; justify-content: space-between; flex-wrap: wrap; gap: 12px;">
            <div>
                <h1>✨ Happy Decoder <span class="badge">happ://crypt* | VLESS | Sub</span></h1>
                <div class="subhead">
                    🔓 Расшифровка зашифрованных VPN-подписок Happ (crypt → RSA/ChaCha) + парсинг обычных sub-ссылок
                    <span class="mode-tag">автоопределение crypt → crypt5</span>
                </div>
            </div>
            <div id="statsLive" style="background:#0a0f1e; border-radius: 40px; padding: 6px 14px; font-size:0.75rem;">⚡ готов</div>
        </div>
    </div>

    <div class="grid-main">
        <!-- левая панель ввода -->
        <div class="input-panel">
            <div class="glass-card">
                <div class="card-title">
                    <span>📎 Ссылка подписки (happ://crypt* / sub:// / обычный URL)</span>
                </div>
                <textarea id="subInput" rows="6" placeholder="happ://crypt5:aHR0cHM6Ly9leGFtcGxlLmNvbS9zdWI/dG9rZW49WFhY
https://sub.whiteness.site/WK3QvAj_6qbG2Cmx
vless://... 
sub://https://vpn.example/sub">
                </textarea>
                <div class="btn-group">
                    <button id="decodeBtn" class="primary">🔓 Расшифровать & парсить серверы</button>
                    <button id="clearBtn">🗑 Очистить</button>
                    <button id="exampleBtn">📋 Примеры (happ + sub)</button>
                </div>
                <div style="margin-top: 14px; font-size:0.7rem; background:#00000030; border-radius: 1rem; padding: 8px;">
                    🧠 Поддерживаются: <strong>happ://crypt, crypt2...crypt5</strong> (base64 → расшифровка RSA/ChaCha симулирована, но реальное декодирование требует ключей — мы парсим прямую ссылку),
                    обычные <strong>sub://</strong> и любые ссылки <strong>vless://, vmess://, trojan://</strong>. Из подписки извлекаем VLESS-серверы и REALITY параметры.
                </div>
            </div>
        </div>

        <!-- правая панель результатов -->
        <div class="output-panel">
            <div class="glass-card" style="height: 100%; display: flex; flex-direction: column;">
                <div class="result-header">
                    <span>🌐 Извлечённые серверы (простой формат + VLESS-URI)</span>
                    <button id="copyAllBtn" style="background:#2d3b5a; padding: 4px 14px;">📋 Копировать всё</button>
                </div>
                <div id="serverListArea" class="server-list">
                    <div style="color:#8e9bb5; text-align: center; padding: 32px;">✨ Ожидание ссылки... вставьте happ://crypt* или sub://...</div>
                </div>
                <div id="infoCounter" style="margin-top: 8px; font-size: 0.7rem; text-align: right; color: #84a7d0;"></div>
            </div>
        </div>
    </div>
    <div class="footer-note">
        <span>⚡ Расшифровка happ://crypt* : извлекаем встроенную ссылку подписки (base64). Для полной дешифровки RSA/ChaCha потребовались бы приватные ключи, но сервис имитирует получение реальной конфигурации через прямой URL. <strong>Локальная работа, данные не отправляются.</strong></span>
        <span>🔹 поддерживаются sub. домены и любые VLESS/trojan строки</span>
    </div>
</div>

<script>
    (function() {
        // ---------- Универсальный парсер Happ / sub / VLESS ----------
        // Эмуляция расшифровки happ://crypt*: извлекаем зашифрованную часть, декодируем base64,
        // получаем URL реальной подписки. Потом загружаем данные через прокси? нет, CORS.
        // Но поскольку мы локальный инструмент, мы будем извлекать из happ-ссылки конечный url подписки
        // (в реальном Happy Decoder там происходит расшифровка, но для демонстрации мы покажем, как ссылка преобразуется в прямую подписку,
        // а также пользователь может вставить уже готовую raw sub-ссылку. Также мы сможем вытянуть серверы если пользователь вставит сам контент.
        // Дополнительно: сделаем парсинг уже готового текста подписки (например, если пользователь вставит конфиг).
        
        // Функция: из строки пытаемся извлечь все VLESS/VMess и другие прокси, а также прямые ссылки на sub.
        function extractAllServers(inputText) {
            let serversList = []; // уникальные строки серверов вида "host:port" или полные vless://
            if (!inputText || typeof inputText !== 'string') return serversList;
            
            const lines = inputText.split(/\r?\n/);
            for (let line of lines) {
                line = line.trim();
                if (!line) continue;
                
                // 1) если это happ://crypt* ссылка — расшифровываем до URL подписки
                if (line.match(/^happ:\/\/crypt\d*:/i)) {
                    let decodedSubUrl = decodeHappLink(line);
                    if (decodedSubUrl && decodedSubUrl.startsWith('http')) {
                        serversList.push(`[Subscription] ${decodedSubUrl}`);
                        // дополнительно: выводим как служебную строку, но также симулируем, что можно получить серверы, если загрузить
                        // Однако автоматически fetch нельзя из-за CORS. Но дадим пользователю подсказку.
                        serversList.push(`ℹ️ RAW подписка: ${decodedSubUrl} (скопируйте и вставьте содержимое в поле для парсинга VLESS)`);
                    }
                    continue;
                }
                
                // 2) поддержка sub:// ссылок (превращаем в реальный URL)
                if (line.startsWith('sub://')) {
                    let subUrl = line.slice(6);
                    if (subUrl.startsWith('http')) {
                        serversList.push(`[Sub-link] ${subUrl}`);
                    } else {
                        serversList.push(`[Sub] ${subUrl}`);
                    }
                    continue;
                }
                
                // 3) парсинг стандартных прокси-ссылок
                let parsed = parseProxyUri(line);
                if (parsed) {
                    if (!serversList.includes(parsed)) serversList.push(parsed);
                    continue;
                }
                
                // 4) если строка похожа на URL подписки (https://...) и не happ/sub, добавляем как ссылку на подписку
                if (line.startsWith('http://') || line.startsWith('https://')) {
                    serversList.push(`📡 Subscription source: ${line}`);
                    continue;
                }
                
                // 5) если строка является просто host:port
                if (/^[\w.-]+:\d{1,5}$/.test(line)) {
                    if (!serversList.includes(line)) serversList.push(line);
                }
            }
            return serversList;
        }
        
        // расшифровка happ://crypt* (эмуляция — извлечение base64 payload и декод)
        function decodeHappLink(happUrl) {
            try {
                // формат: happ://crypt5:base64data  или happ://crypt:base64data
                let match = happUrl.match(/^happ:\/\/crypt\d*:(.+)$/i);
                if (!match) return null;
                let b64payload = match[1];
                // удаляем возможные фрагменты #
                b64payload = b64payload.split('#')[0];
                let decoded = atob(b64payload);
                // в decoded может быть прямая ссылка подписки, либо конфиг JSON
                if (decoded.startsWith('http://') || decoded.startsWith('https://')) {
                    return decoded;
                } else {
                    // возможно внутри JSON. ищем url
                    try {
                        let json = JSON.parse(decoded);
                        if (json.url) return json.url;
                        if (json.subscription_url) return json.subscription_url;
                        return null;
                    } catch(e) {
                        // если внутри просто текст, который может быть ссылкой
                        let urlMatch = decoded.match(/(https?:\/\/[^\s]+)/);
                        if (urlMatch) return urlMatch[1];
                        return null;
                    }
                }
            } catch(e) { console.warn(e); return null; }
        }
        
        // универсальный парсер vless:// vmess:// trojan:// ss://
        function parseProxyUri(uri) {
            if (uri.startsWith('vless://')) {
                return parseVlessLike(uri);
            }
            if (uri.startsWith('trojan://')) {
                return parseTrojan(uri);
            }
            if (uri.startsWith('vmess://')) {
                return parseVmess(uri);
            }
            if (uri.startsWith('ss://')) {
                return parseShadowsocks(uri);
            }
            if (uri.startsWith('ssr://')) {
                return parseSSR(uri);
            }
            return null;
        }
        
        function parseVlessLike(url) {
            try {
                let raw = url.slice(8);
                let atIndex = raw.indexOf('@');
                if (atIndex === -1) return null;
                let afterAt = raw.substring(atIndex + 1);
                let hostPortPart = afterAt.split('?')[0].split('#')[0];
                let hp = hostPortPart.split(':');
                if (hp.length >= 2) {
                    let host = hp[0];
                    let port = hp[1];
                    let user = raw.substring(0, atIndex);
                    if (user && host && port) return `vless://${user}@${host}:${port}`;
                    return `${host}:${port}`;
                }
                return null;
            } catch(e) { return null; }
        }
        
        function parseTrojan(url) {
            try {
                let raw = url.slice(9);
                let atIndex = raw.indexOf('@');
                if (atIndex === -1) return null;
                let password = raw.substring(0, atIndex);
                let afterAt = raw.substring(atIndex + 1);
                let hostPort = afterAt.split('?')[0].split('#')[0];
                let [host, port] = hostPort.split(':');
                if (host && port) return `trojan://${password}@${host}:${port}`;
                return null;
            } catch(e) { return null; }
        }
        
        function parseVmess(url) {
            try {
                let b64part = url.slice(8);
                b64part = b64part.split('#')[0];
                let decoded = atob(b64part);
                let config = JSON.parse(decoded);
                let host = config.addr || config.host;
                let port = config.port;
                let id = config.id || '';
                if (host && port) return `vmess://${id}@${host}:${port}  (decoded)`;
                return null;
            } catch(e) { return null; }
        }
        
        function parseShadowsocks(url) {
            try {
                let b64part = url.slice(5);
                if (b64part.includes('@')) {
                    let [methodPass, hostPort] = b64part.split('@');
                    let [host, port] = hostPort.split(':');
                    if (host && port) return `ss://${methodPass}@${host}:${port}`;
                } else {
                    let clean = b64part.split('#')[0];
                    let decoded = atob(clean);
                    if (decoded.includes('@')) {
                        let [mp, hp] = decoded.split('@');
                        let [host, port] = hp.split(':');
                        if (host && port) return `ss://${mp}@${host}:${port}`;
                    }
                }
                return null;
            } catch(e) { return null; }
        }
        
        function parseSSR(url) {
            try {
                let b64 = url.slice(6);
                let decoded = atob(b64);
                let parts = decoded.split(':');
                if (parts.length >= 6) {
                    let server = parts[0];
                    let port = parts[1];
                    return `ssr://${server}:${port}`;
                }
                return null;
            } catch(e) { return null; }
        }
        
        // функция для рендеринга серверов (из ввода)
        function renderServers() {
            const rawText = document.getElementById('subInput').value;
            if (!rawText.trim()) {
                document.getElementById('serverListArea').innerHTML = '<div style="color:#8ba0c2; text-align:center; padding:36px;">💡 Введите happ://crypt*, sub:// или ссылку на подписку с VLESS</div>';
                document.getElementById('infoCounter').innerHTML = '';
                document.getElementById('statsLive').innerHTML = '⚡ ожидание ссылки';
                return;
            }
            
            let servers = extractAllServers(rawText);
            // фильтр пустых и дублей (сохраняем уникальность)
            servers = [...new Map(servers.map(s => [s, s])).values()];
            const container = document.getElementById('serverListArea');
            const counterSpan = document.getElementById('infoCounter');
            const statsSpan = document.getElementById('statsLive');
            
            if (servers.length === 0) {
                container.innerHTML = '<div style="color:#f28b82; text-align:center; padding:28px;">❌ Не удалось распознать серверы или ссылки. Проверьте формат (happ://crypt* / sub:// / vless://). Для подписок сперва получите конфигурационный текст.</div>';
                counterSpan.innerText = '🔍 не найдено элементов';
                statsSpan.innerHTML = '⚠️ 0 узлов';
                return;
            }
            
            let html = `<div style="margin-bottom: 12px;">✅ Найдено элементов: <strong>${servers.length}</strong> (серверы, ссылки, подписки)</div>`;
            servers.forEach((sv, idx) => {
                let escaped = escapeHtml(sv);
                html += `
                    <div class="server-item">
                        <div class="server-code" style="flex:1;">${idx+1}. ${escaped}</div>
                        <button class="copy-mini copy-single" data-value="${escapeAttr(sv)}">📋 Копировать</button>
                    </div>
                `;
            });
            container.innerHTML = html;
            counterSpan.innerText = `📡 итого уникальных записей: ${servers.length}`;
            statsSpan.innerHTML = `✨ распознано: ${servers.length}`;
            
            // привязать события кнопок
            document.querySelectorAll('.copy-single').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    let val = btn.getAttribute('data-value');
                    if (val) copyToClipboard(val);
                    showToast(`Скопировано: ${val.substring(0, 60)}`);
                });
            });
            window.currentServers = servers;
        }
        
        function escapeHtml(str) {
            return str.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;');
        }
        function escapeAttr(str) {
            return str.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;').replace(/"/g, '&quot;');
        }
        
        async function copyToClipboard(text) {
            try {
                await navigator.clipboard.writeText(text);
            } catch(err) {
                let ta = document.createElement('textarea');
                ta.value = text;
                document.body.appendChild(ta);
                ta.select();
                document.execCommand('copy');
                document.body.removeChild(ta);
            }
        }
        
        function showToast(msg) {
            let toast = document.getElementById('globalToast');
            if (!toast) {
                let div = document.createElement('div');
                div.id = 'globalToast';
                div.style.position = 'fixed';
                div.style.bottom = '25px';
                div.style.left = '50%';
                div.style.transform = 'translateX(-50%)';
                div.style.backgroundColor = '#0f172a';
                div.style.color = '#b9f3ff';
                div.style.padding = '10px 24px';
                div.style.borderRadius = '60px';
                div.style.fontSize = '0.8rem';
                div.style.zIndex = '9999';
                div.style.border = '1px solid #22d3ee';
                div.style.backdropFilter = 'blur(12px)';
                div.style.fontWeight = '500';
                document.body.appendChild(div);
                toast = div;
            }
            toast.innerText = msg;
            toast.style.opacity = '1';
            clearTimeout(window.toastTimeout);
            window.toastTimeout = setTimeout(() => {
                toast.style.opacity = '0';
            }, 2000);
        }
        
        function copyAll() {
            if (window.currentServers && window.currentServers.length) {
                let all = window.currentServers.join('\n');
                copyToClipboard(all);
                showToast(`📋 Скопировано ${window.currentServers.length} элементов`);
            } else {
                showToast("Нет данных для копирования");
            }
        }
        
        function clearAll() {
            document.getElementById('subInput').value = '';
            renderServers();
        }
        
        function setExamples() {
            document.getElementById('subInput').value = `happ://crypt5:aHR0cHM6Ly9zdWIud2hpdGVuZXNzLnNpdGUvV0szUXZBai82cWJHMkNteA==
https://sub.whiteness.site/WK3QvAj_6qbG2Cmx
vless://9f4e2d1c-3b2a-4c5d-8e7f-6a5b4c3d2e1f@lon.reality-server.com:443?encryption=none&security=reality&pbk=abcd#UK-REALITY
trojan://mySecurePass@trojan-vpn.xyz:443?security=tls
sub://https://vpn-example.com/sub/v1?token=abc123
vmess://eyJhZGQiOiJmci12bWVzcy5jbG91ZCIsInBvcnQiOjQ0MywiaWQiOiI2YzE5ZDU4OS0zMjE0LTQ3ODktYmRlZi0xMjM0NTY3ODkwIiwicHMiOiLms6LkuK3ml6AifQ==`;
            renderServers();
        }
        
        // инициализация
        window.currentServers = [];
        document.getElementById('decodeBtn').addEventListener('click', renderServers);
        document.getElementById('clearBtn').addEventListener('click', clearAll);
        document.getElementById('exampleBtn').addEventListener('click', setExamples);
        document.getElementById('copyAllBtn').addEventListener('click', copyAll);
        // предзаполнить демо-ссылкой (как на happy-decoder.cc)
        setTimeout(() => {
            if (!document.getElementById('subInput').value.trim()) {
                setExamples();
            } else {
                renderServers();
            }
        }, 100);
    })();
</script>
</body>
</html>
</body>
</html>
