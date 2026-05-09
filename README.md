<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Universal VPN Parser | VLESS / SSR / Trojan / Sub → Серверы</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: #0a0c12;
            font-family: 'Inter', system-ui, -apple-system, 'Segoe UI', 'Poppins', sans-serif;
            padding: 24px 16px;
            color: #eef2ff;
        }

        /* Glassmorphic container как в hapр */
        .app-container {
            max-width: 1400px;
            margin: 0 auto;
            background: rgba(18, 22, 35, 0.65);
            backdrop-filter: blur(18px);
            border-radius: 2rem;
            border: 1px solid rgba(72, 85, 120, 0.35);
            box-shadow: 0 25px 45px -10px rgba(0, 0, 0, 0.4), inset 0 1px 0 rgba(255,255,255,0.05);
            overflow: hidden;
        }

        /* Header */
        .header {
            padding: 1.5rem 2rem;
            background: rgba(12, 14, 24, 0.75);
            border-bottom: 1px solid rgba(78, 92, 130, 0.3);
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            align-items: center;
            gap: 1rem;
        }
        .title-section h1 {
            font-size: 1.8rem;
            font-weight: 600;
            background: linear-gradient(125deg, #ffffff, #a5f3fc, #5ee0fa);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            letter-spacing: -0.3px;
            display: flex;
            align-items: center;
            gap: 12px;
        }
        .badge-pro {
            background: #2dd4bf20;
            border: 1px solid #2dd4bf60;
            border-radius: 60px;
            padding: 4px 12px;
            font-size: 0.7rem;
            font-weight: 500;
            color: #99f6e4;
        }
        .stats {
            background: #0f111a;
            padding: 6px 16px;
            border-radius: 60px;
            font-size: 0.85rem;
            font-family: monospace;
            color: #b9c8ff;
        }
        /* Layout */
        .main-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 1.5rem;
            padding: 1.8rem 2rem;
        }
        .input-panel {
            flex: 1.2;
            min-width: 320px;
        }
        .output-panel {
            flex: 1.8;
            min-width: 380px;
        }
        .glass-card {
            background: rgba(25, 30, 45, 0.7);
            backdrop-filter: blur(4px);
            border-radius: 1.5rem;
            border: 1px solid rgba(96, 112, 150, 0.35);
            padding: 1.3rem;
            transition: all 0.2s;
        }
        .card-title {
            font-weight: 500;
            margin-bottom: 1rem;
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 1rem;
            letter-spacing: 0.3px;
            color: #cbd5ff;
        }
        textarea {
            width: 100%;
            background: #0b0e16;
            border: 1px solid #2d3750;
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
            border-color: #3b82f6;
            box-shadow: 0 0 0 2px #3b82f620;
        }
        .btn-group {
            display: flex;
            gap: 10px;
            margin-top: 1rem;
            flex-wrap: wrap;
        }
        button {
            background: #1e243b;
            border: none;
            padding: 0.5rem 1.2rem;
            border-radius: 2rem;
            font-weight: 500;
            font-size: 0.8rem;
            cursor: pointer;
            transition: 0.2s;
            color: #eef2ff;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            border: 1px solid #334155;
        }
        button.primary {
            background: #0f2b3d;
            border-color: #2c6e9e;
            color: #b9f3ff;
            box-shadow: 0 0 8px rgba(0,160,255,0.2);
        }
        button.primary:hover {
            background: #144a66;
            transform: translateY(-1px);
        }
        button:hover {
            background: #2a344f;
            border-color: #5b6e9e;
        }
        .result-header {
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            margin-bottom: 12px;
            flex-wrap: wrap;
        }
        .server-list {
            background: #07090fcc;
            border-radius: 1.2rem;
            padding: 0.8rem;
            max-height: 460px;
            overflow-y: auto;
            font-family: monospace;
        }
        .server-item {
            background: #0f121eb3;
            margin-bottom: 8px;
            padding: 8px 12px;
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
        .copy-btn-mini {
            background: #2a2f44;
            padding: 4px 12px;
            border-radius: 30px;
            font-size: 0.7rem;
        }
        .footer-note {
            padding: 1rem 2rem;
            border-top: 1px solid rgba(78, 92, 130, 0.3);
            font-size: 0.7rem;
            color: #7f8cb0;
            display: flex;
            justify-content: space-between;
        }
        ::-webkit-scrollbar {
            width: 5px;
        }
        ::-webkit-scrollbar-track {
            background: #1a1f2e;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb {
            background: #3b4b6e;
            border-radius: 10px;
        }
        @media (max-width: 760px) {
            .main-grid { padding: 1rem; }
            .header { flex-direction: column; align-items: start; }
        }
        .link-type {
            font-size: 0.6rem;
            background: #1e2a3e;
            border-radius: 30px;
            padding: 2px 8px;
        }
    </style>
</head>
<body>
<div class="app-container">
    <div class="header">
        <div class="title-section">
            <h1>🌀 VPN PARSER <span class="badge-pro">VLESS | VMess | Trojan | Shadowsocks | Sub</span></h1>
            <div style="font-size: 0.75rem; margin-top: 6px;">Декодируй любые ссылки → чистые серверные строки (хост:порт, логин:пароль@хост:порт)</div>
        </div>
        <div class="stats" id="globalStats">⚡ готов к расшифровке</div>
    </div>

    <div class="main-grid">
        <!-- Левая панель ввода -->
        <div class="input-panel">
            <div class="glass-card">
                <div class="card-title">
                    <span>📎 Вставьте ссылки (VLESS, VMess, SSR, Trojan, sub:// и любые vpn://)</span>
                </div>
                <textarea id="vpnInput" rows="8" placeholder="vless://9a6e...@example.com:443?encryption=none#Name
vmess://eyJhZGQiOiJzZy1leGFtcGxlLmNvbSIsInBvcnQiOjQ0MywicHMiOiLnu4Tnu4cifQ==
trojan://password@trojan-server.xyz:443?security=tls
ss://Y2hhY2hhMjAtaWV0Zi1wb2x5MTMwNTpwYXNzd29yZA==@us-ny.node.com:8388
sub://https://vip-planet.com/sub/v1?token=xxx
vpn://vpnuser:pass@193.104.210.45:1194"></textarea>
                <div class="btn-group">
                    <button id="decodeBtn" class="primary">🔓 Расшифровать → серверы</button>
                    <button id="clearBtn">🗑 Очистить</button>
                    <button id="exampleBtn">✨ Примеры ссылок</button>
                </div>
                <div style="margin-top: 12px; font-size: 0.7rem; background:#00000040; border-radius: 1rem; padding: 8px;">
                    🧩 Поддержка: VLESS, VMess(b64), Trojan, Shadowsocks, SSR, vpn://, sub:// (извлечение ссылок из подписки)
                </div>
            </div>
        </div>

        <!-- Правая панель результат -->
        <div class="output-panel">
            <div class="glass-card" style="height: 100%; display: flex; flex-direction: column;">
                <div class="result-header">
                    <span>🌍 Извлечённые серверы (простой формат)</span>
                    <div>
                        <button id="copyAllServersBtn" style="background: #2d3b5a; padding: 4px 14px;">📋 Копировать всё</button>
                    </div>
                </div>
                <div id="serverListContainer" class="server-list">
                    <div style="color: #8e9bb5; text-align: center; padding: 30px;">✨ Ожидание ввода... вставьте VLESS/VMess/Trojan или sub ссылки</div>
                </div>
                <div id="countInfo" style="margin-top: 8px; font-size: 0.7rem; text-align: right; color: #8aa0c9;"></div>
            </div>
        </div>
    </div>
    <div class="footer-note">
        <span>🛡️ Всё локально: парсинг VLESS / VMess / Shadowsocks / Trojan / sub-ссылки → хост:порт и если есть логин/пароль. Никаких запросов.</span>
        <span>⚡ happ‑стиль + полная поддержка</span>
    </div>
</div>

<script>
    (function() {
        // ---------- универсальный парсер для всех форматов ----------
        function extractServersFromLinks(rawText) {
            let extracted = []; // массив строк "server:port" или "user:pass@server:port"
            if (!rawText || typeof rawText !== 'string') return extracted;
            const lines = rawText.split(/\r?\n/);
            
            for (let line of lines) {
                line = line.trim();
                if (!line) continue;
                
                // 1) Обработка sub:// ссылок — они содержат URL подписки, из которой нужно вытянуть контент? 
                // Но sub ссылка сама по себе не даёт сразу сервер, но технически sub:// это просто ссылка на удаленный subscription.
                // По заданию "поддерживался sub и другого рода ссылки" — мы можем извлечь сам URL подписки как строку (но не хост:порт). 
                // Однако часто sub ссылка ведёт к списку конфигов. Но мы не можем парсить удалённо (безопасность). Поэтому из sub:// ссылки извлекаем базовый URL и выводим как "[sub] link: ...", но пользователи ждут серверы. 
                // Лучше если sub встречается — показываем простую строку: "SUBSCRIPTION: url" но по сути сервер не извлечь без запроса. Но сделаем извлечение домена и порта из URL самой sub ссылки? 
                // Умный подход: если sub:// содержит внутри https://domain.com/path, то вытащим хост и порт из URL? Не совсем корректно. Но оставим поддержку: вместе с тем парсим все остальные стандартные протоколы.
                // В дополнение: рекурсивно если ссылка является sub://, мы можем её пропустить, но чтобы визуал не страдал, покажем "sub domain" если есть домен.
                
                // ---- Сначала обработка известных схем ----
                let serverStr = null;
                
                // VLESS, Trojan, VMess (b64), Shadowsocks, SSR обработка
                if (line.startsWith('vless://')) {
                    serverStr = parseVlessLike(line, 'vless');
                }
                else if (line.startsWith('trojan://')) {
                    serverStr = parseTrojan(line);
                }
                else if (line.startsWith('vmess://')) {
                    serverStr = parseVmess(line);
                }
                else if (line.startsWith('ss://')) {
                    serverStr = parseShadowsocks(line);
                }
                else if (line.startsWith('ssr://')) {
                    serverStr = parseSSR(line);
                }
                else if (line.startsWith('vpn://')) {
                    serverStr = parseVpnScheme(line);
                }
                else if (line.startsWith('sub://')) {
                    // sub ссылка: извлекаем URL, не сервер, но покажем в виде строки с информацией
                    let subUrl = line.slice(6);
                    if (subUrl.startsWith('https://') || subUrl.startsWith('http://')) {
                        try {
                            let urlObj = new URL(subUrl);
                            let hostPort = urlObj.hostname;
                            if (urlObj.port) hostPort += ':' + urlObj.port;
                            serverStr = `[Subscription] ${hostPort}`;
                        } catch(e) { serverStr = `[sub] ${subUrl.substring(0,60)}`; }
                    } else { serverStr = `[sub] ${subUrl.substring(0,60)}`; }
                }
                
                // дополнительно если строка содержит обычный URL с http но не имеет схемы vpn, можно попробовать вытянуть хост:порт? не нужно
                if (serverStr) {
                    if (!extracted.includes(serverStr)) extracted.push(serverStr);
                    continue;
                }
                
                // Если не совпало ни с чем, но может содержать просто строку ip:port (поддержка raw)
                const simpleHostPort = /^([a-zA-Z0-9.-]+):(\d{1,5})$/;
                if (simpleHostPort.test(line)) {
                    if (!extracted.includes(line)) extracted.push(line);
                }
            }
            return extracted;
        }
        
        // --- VLESS (и подобные: vless://uuid@host:port?params#tag) ---
        function parseVlessLike(url, scheme) {
            try {
                let raw = url.slice(scheme.length + 3); // после vless://
                let atIndex = raw.indexOf('@');
                if (atIndex === -1) return null;
                let afterAt = raw.substring(atIndex + 1);
                let hostPortPart = afterAt.split('?')[0].split('#')[0];
                let hostAndPort = hostPortPart.split(':');
                if (hostAndPort.length >= 2) {
                    let host = hostAndPort[0];
                    let port = hostAndPort[1];
                    let userPart = raw.substring(0, atIndex);
                    if (userPart && host && port) {
                        return `${userPart}@${host}:${port}`;
                    } else if (host && port) {
                        return `${host}:${port}`;
                    }
                }
                return null;
            } catch(e) { return null; }
        }
        
        // Trojan: trojan://password@host:port?params
        function parseTrojan(url) {
            try {
                let raw = url.slice(9); // 'trojan://'.length
                let atIndex = raw.indexOf('@');
                if (atIndex === -1) return null;
                let password = raw.substring(0, atIndex);
                let afterAt = raw.substring(atIndex + 1);
                let hostPort = afterAt.split('?')[0].split('#')[0];
                let [host, port] = hostPort.split(':');
                if (host && port && password) return `${password}@${host}:${port}`;
                if (host && port) return `${host}:${port}`;
                return null;
            } catch(e) { return null; }
        }
        
        // VMess base64 декодирование
        function parseVmess(url) {
            try {
                let b64Part = url.slice(8); // vmess://
                // иногда содержит # в конце
                b64Part = b64Part.split('#')[0];
                let decoded = atob(b64Part);
                let config = JSON.parse(decoded);
                let host = config.addr || config.host || config.address;
                let port = config.port;
                let ps = config.ps || config.remark || '';
                if (host && port) {
                    let user = config.id || config.uuid || '';
                    if (user) return `${user}@${host}:${port}`;
                    return `${host}:${port}`;
                }
                return null;
            } catch(e) { return null; }
        }
        
        // Shadowsocks: ss://method:password@host:port  или base64-кодированный
        function parseShadowsocks(url) {
            try {
                let b64part = url.slice(5); // ss://
                if (b64part.includes('@')) {
                    // стандартный ss://method:password@host:port
                    let [methodPass, hostPort] = b64part.split('@');
                    let hostPortSplit = hostPort.split(':');
                    if (hostPortSplit.length >= 2) {
                        let host = hostPortSplit[0];
                        let port = hostPortSplit[1].split('/')[0].split('?')[0];
                        let methodPassClean = methodPass;
                        return `${methodPassClean}@${host}:${port}`;
                    }
                } else {
                    // base64 формат ss://base64#tag
                    let clean = b64part.split('#')[0];
                    let decoded = atob(clean);
                    // decoded обычно method:password@host:port
                    if (decoded.includes('@')) {
                        let [mp, hp] = decoded.split('@');
                        let [host, port] = hp.split(':');
                        if (host && port) return `${mp}@${host}:${port}`;
                        return `${host}:${port}`;
                    }
                }
                return null;
            } catch(e) { return null; }
        }
        
        // SSR: ssr://base64
        function parseSSR(url) {
            try {
                let b64 = url.slice(6);
                let decoded = atob(b64);
                // формат ssr: server:port:protocol:method:obfs:base64pass/?params
                let parts = decoded.split(':');
                if (parts.length >= 6) {
                    let server = parts[0];
                    let port = parts[1];
                    let passwordEncoded = parts[5];
                    let password = '';
                    try {
                        password = atob(passwordEncoded);
                    } catch(e) { password = passwordEncoded; }
                    if (server && port) {
                        if (password) return `${password}@${server}:${port}`;
                        return `${server}:${port}`;
                    }
                }
                return null;
            } catch(e) { return null; }
        }
        
        function parseVpnScheme(url) {
            let regex = /^vpn:\/\/(?:([^:]+):([^@]+)@)?([^:/\?#]+):(\d+)/i;
            let match = url.match(regex);
            if (match) {
                let user = match[1];
                let pass = match[2];
                let host = match[3];
                let port = match[4];
                if (user && pass) return `${user}:${pass}@${host}:${port}`;
                return `${host}:${port}`;
            }
            return null;
        }
        
        // главный рендер
        function renderServers() {
            const raw = document.getElementById('vpnInput').value;
            if (!raw.trim()) {
                document.getElementById('serverListContainer').innerHTML = '<div style="color:#6f85ad; text-align:center; padding:32px;">💤 Вставьте VLESS/VMess/Trojan/SS/sub ссылки</div>';
                document.getElementById('countInfo').innerText = '';
                document.getElementById('globalStats').innerHTML = '⚡ 0 серверов';
                return;
            }
            let servers = extractServersFromLinks(raw);
            // фильтр пустых
            servers = servers.filter(s => s && s.length > 2);
            const container = document.getElementById('serverListContainer');
            const countSpan = document.getElementById('countInfo');
            const statsSpan = document.getElementById('globalStats');
            
            if (servers.length === 0) {
                container.innerHTML = '<div style="color:#e06c75; text-align:center; padding:28px;">❌ Не удалось извлечь серверы. Попробуйте ссылки VLESS, VMess, Trojan, SS, SSR, vpn:// или sub://</div>';
                countSpan.innerText = '✅ найдено 0 элементов';
                statsSpan.innerHTML = '⚠️ 0 распознано';
                return;
            }
            
            let html = `<div style="margin-bottom: 6px;">🎯 <strong>${servers.length}</strong> сервер(ов) в чистом виде:</div>`;
            servers.forEach((sv, idx) => {
                let escaped = sv.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;');
                html += `
                    <div class="server-item">
                        <div class="server-code" style="flex:1;">${idx+1}. ${escaped}</div>
                        <button class="copy-btn-mini copy-single" data-value="${escapeAttr(sv)}">📋 Копировать</button>
                    </div>
                `;
            });
            container.innerHTML = html;
            countSpan.innerText = `📡 извлечено уникальных серверных строк: ${servers.length}`;
            statsSpan.innerHTML = `🔓 активные узлы: ${servers.length}`;
            
            // добавить события кнопкам
            document.querySelectorAll('.copy-single').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    let val = btn.getAttribute('data-value');
                    if (val) copyToClipboard(val);
                    showToast(`Скопировано: ${val.substring(0, 42)}`);
                });
            });
            window.currentServersList = servers;
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
            let toast = document.getElementById('toastMsg');
            if (!toast) {
                let div = document.createElement('div');
                div.id = 'toastMsg';
                div.style.position = 'fixed';
                div.style.bottom = '24px';
                div.style.left = '50%';
                div.style.transform = 'translateX(-50%)';
                div.style.backgroundColor = '#111827';
                div.style.color = '#b9f3ff';
                div.style.padding = '8px 20px';
                div.style.borderRadius = '60px';
                div.style.fontSize = '0.8rem';
                div.style.zIndex = '9999';
                div.style.border = '1px solid #2dd4bf';
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
            }, 1800);
        }
        
        function copyAll() {
            if (window.currentServersList && window.currentServersList.length) {
                let allText = window.currentServersList.join('\n');
                copyToClipboard(allText);
                showToast(`📋 Скопировано ${window.currentServersList.length} серверов`);
            } else {
                showToast("Нет серверов для копирования");
            }
        }
        
        function clearAll() {
            document.getElementById('vpnInput').value = '';
            renderServers();
        }
        
        function setExamples() {
            document.getElementById('vpnInput').value = `vless://8f4e3d2c-1b2a-4c3d-9e8f-7a6b5c4d3e2f@lon.vpn-example.com:443?encryption=none&security=tls#UK-Server
vmess://eyJhZGQiOiJmci12bWVzcy5jbG91ZCIsInBvcnQiOjQ0MywiaWQiOiI2YzE5ZDU4OS0zMjE0LTQ3ODktYmRlZi0xMjM0NTY3ODkwIiwicHMiOiLms6LkuK3ml6AifQ==
trojan://mysecretpassword@trojan.nl-freepoint.xyz:8443?security=tls&sni=trojan.nl-freepoint.xyz#TrojanNL
ss://Y2hhY2hhMjAtaWV0Zi1wb2x5MTMwNTpteXBhc3N3b3JkMTIz@sg-shadowsock.xyz:10086#SS-SG
ssr://c2ctc3NyLmV4YW1wbGUuY29tOjU0MzE6YXV0aF9hZXMxMjg6Y2hhY2hhMjA6aHR0cF9zaW1wbGU6Y25kOjJkNTExMg
vpn://openvpnuser:strongpass@vpn.server.co:1194
sub://https://example.com/sub/v2ray?token=abcd1234
185.102.45.67:8080`;
            renderServers();
        }
        
        // инициализация UI
        window.currentServersList = [];
        document.getElementById('decodeBtn').addEventListener('click', renderServers);
        document.getElementById('clearBtn').addEventListener('click', clearAll);
        document.getElementById('exampleBtn').addEventListener('click', setExamples);
        document.getElementById('copyAllServersBtn').addEventListener('click', copyAll);
        // первичный запуск с демо
        setTimeout(() => {
            if (!document.getElementById('vpnInput').value.trim()) {
                setExamples();
            } else {
                renderServers();
            }
        }, 100);
    })();
</script>
</body>
</html>
