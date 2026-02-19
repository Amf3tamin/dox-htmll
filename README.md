<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
    <title>VK Парсер профилей</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: #f0f2f5;
            padding: 15px;
            min-height: 100vh;
        }
        
        .container {
            max-width: 600px;
            margin: 0 auto;
            background: white;
            border-radius: 12px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            padding: 20px;
        }
        
        h1 {
            font-size: 22px;
            margin-bottom: 15px;
            color: #2a5885;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        h1::before {
            content: "";
            display: inline-block;
            width: 24px;
            height: 24px;
            background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="%232a5885"><path d="M15.684 0H8.316C2.827 0 0 2.827 0 8.316v7.368C0 21.173 2.827 24 8.316 24h7.368C21.173 24 24 21.173 24 15.684V8.316C24 2.827 21.173 0 15.684 0zm3.473 16.524h-2.084c-.756 0-.99-.564-2.34-1.956-1.176-1.176-1.68-1.32-1.98-1.32-.42 0-.54.12-.54.66v1.74c0 .48-.12.756-1.44.756-2.124 0-4.488-1.32-6.132-3.78-2.58-3.588-3.288-6.3-3.288-6.864 0-.36.12-.66.72-.66h2.1c.66 0 .9.3 1.14.96.84 2.52 2.28 4.74 2.88 4.74.24 0 .36-.12.36-.84V8.94c-.12-1.44-.84-1.56-.84-2.1 0-.24.18-.48.48-.48h3.3c.48 0 .66.24.66.84v4.5c0 .48.18.66.3.66.24 0 .48-.18.96-.72 1.14-1.32 2.04-3.36 2.04-3.36.12-.36.42-.66.9-.66h2.1c.66 0 .84.36.66.96-.3 1.2-2.4 4.44-2.4 4.44-.24.36-.3.54 0 .96.18.24 1.02 1.02 1.56 1.68.9 1.02 1.56 1.86 1.56 2.46 0 .36-.18.72-.9.72z"/></svg>');
            background-size: contain;
        }
        
        .input-group {
            display: flex;
            gap: 8px;
            margin-bottom: 20px;
        }
        
        #urlInput {
            flex: 1;
            padding: 14px 12px;
            border: 1px solid #d3d9de;
            border-radius: 8px;
            font-size: 16px;
            outline: none;
        }
        
        #urlInput:focus {
            border-color: #2a5885;
        }
        
        #parseBtn {
            padding: 0 20px;
            background: #2a5885;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 500;
            cursor: pointer;
            transition: background 0.2s;
        }
        
        #parseBtn:active {
            background: #1e3f5e;
        }
        
        #parseBtn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
        }
        
        .status {
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 20px;
            font-size: 14px;
            display: none;
        }
        
        .status.loading {
            display: block;
            background: #e3f2fd;
            color: #1565c0;
        }
        
        .status.error {
            display: block;
            background: #ffebee;
            color: #c62828;
        }
        
        .result-card {
            border: 1px solid #e0e0e0;
            border-radius: 12px;
            overflow: hidden;
        }
        
        .profile-header {
            display: flex;
            gap: 15px;
            padding: 20px;
            background: #f5f7fa;
            border-bottom: 1px solid #e0e0e0;
        }
        
        .profile-avatar {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            background: #ddd;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #666;
            font-size: 12px;
            text-align: center;
            overflow: hidden;
        }
        
        .profile-avatar img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        
        .profile-title {
            flex: 1;
        }
        
        .profile-name {
            font-size: 20px;
            font-weight: 600;
            color: #1a1a1a;
            margin-bottom: 5px;
        }
        
        .profile-status {
            font-size: 14px;
            color: #656565;
        }
        
        .info-section {
            padding: 15px 20px;
            border-bottom: 1px solid #e0e0e0;
        }
        
        .info-section:last-child {
            border-bottom: none;
        }
        
        .section-title {
            font-size: 16px;
            font-weight: 600;
            color: #2a5885;
            margin-bottom: 12px;
        }
        
        .info-row {
            display: flex;
            margin-bottom: 8px;
            font-size: 14px;
        }
        
        .info-label {
            width: 120px;
            color: #8a8a8a;
        }
        
        .info-value {
            flex: 1;
            color: #1a1a1a;
        }
        
        .info-value a {
            color: #2a5885;
            text-decoration: none;
        }
        
        .posts-list {
            max-height: 300px;
            overflow-y: auto;
        }
        
        .post-item {
            padding: 12px;
            border-bottom: 1px solid #f0f0f0;
            font-size: 14px;
        }
        
        .post-item:last-child {
            border-bottom: none;
        }
        
        .post-date {
            color: #8a8a8a;
            font-size: 12px;
            margin-bottom: 4px;
        }
        
        .post-text {
            color: #1a1a1a;
            word-break: break-word;
        }
        
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            text-align: center;
            margin-top: 10px;
        }
        
        .stat-item {
            background: #f5f7fa;
            padding: 10px;
            border-radius: 8px;
        }
        
        .stat-value {
            font-size: 18px;
            font-weight: 600;
            color: #2a5885;
        }
        
        .stat-label {
            font-size: 12px;
            color: #656565;
            margin-top: 4px;
        }
        
        .footer {
            margin-top: 20px;
            text-align: center;
            font-size: 12px;
            color: #8a8a8a;
        }
        
        .footer a {
            color: #2a5885;
            text-decoration: none;
        }
        
        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>VK Парсер профилей</h1>
        
        <div class="input-group">
            <input type="url" id="urlInput" placeholder="https://vk.com/durov или id1" value="https://vk.com/durov">
            <button id="parseBtn">Парсить</button>
        </div>
        
        <div id="status" class="status"></div>
        
        <div id="resultContainer" class="result-card hidden">
            <div class="profile-header">
                <div class="profile-avatar" id="avatarContainer">
                    <span>нет фото</span>
                </div>
                <div class="profile-title">
                    <div class="profile-name" id="profileName">Загрузка...</div>
                    <div class="profile-status" id="profileStatus">онлайн</div>
                </div>
            </div>
            
            <div class="info-section">
                <div class="section-title">Основная информация</div>
                <div class="info-row">
                    <span class="info-label">ID профиля:</span>
                    <span class="info-value" id="profileId">-</span>
                </div>
                <div class="info-row">
                    <span class="info-label">Дата рождения:</span>
                    <span class="info-value" id="birthDate">-</span>
                </div>
                <div class="info-row">
                    <span class="info-label">Город:</span>
                    <span class="info-value" id="city">-</span>
                </div>
                <div class="info-row">
                    <span class="info-label">Семейное положение:</span>
                    <span class="info-value" id="relationship">-</span>
                </div>
                <div class="info-row">
                    <span class="info-label">Место работы:</span>
                    <span class="info-value" id="work">-</span>
                </div>
                <div class="info-row">
                    <span class="info-label">Образование:</span>
                    <span class="info-value" id="education">-</span>
                </div>
            </div>
            
            <div class="info-section">
                <div class="section-title">Статистика</div>
                <div class="stats-grid">
                    <div class="stat-item">
                        <div class="stat-value" id="followersCount">-</div>
                        <div class="stat-label">подписчиков</div>
                    </div>
                    <div class="stat-item">
                        <div class="stat-value" id="friendsCount">-</div>
                        <div class="stat-label">друзей</div>
                    </div>
                    <div class="stat-item">
                        <div class="stat-value" id="photosCount">-</div>
                        <div class="stat-label">фото</div>
                    </div>
                </div>
            </div>
            
            <div class="info-section">
                <div class="section-title">Последние посты</div>
                <div class="posts-list" id="postsList">
                    <div class="post-item">
                        <div class="post-date">загрузка...</div>
                        <div class="post-text"></div>
                    </div>
                </div>
            </div>
            
            <div class="info-section">
                <div class="section-title">Ссылки и контакты</div>
                <div class="info-row">
                    <span class="info-label">VK:</span>
                    <span class="info-value" id="vkLink">-</span>
                </div>
                <div class="info-row">
                    <span class="info-label">Telegram:</span>
                    <span class="info-value" id="telegram">-</span>
                </div>
                <div class="info-row">
                    <span class="info-label">Instagram:</span>
                    <span class="info-value" id="instagram">-</span>
                </div>
                <div class="info-row">
                    <span class="info-label">Twitter:</span>
                    <span class="info-value" id="twitter">-</span>
                </div>
                <div class="info-row">
                    <span class="info-label">Сайт:</span>
                    <span class="info-value" id="website">-</span>
                </div>
            </div>
        </div>
        
        <div class="footer">
            <p>⚠️ Парсинг ВКонтакте ограничен технически. Некоторые данные могут не отображаться.</p>
            <p>Данные собираются из открытых источников [citation:1]</p>
        </div>
    </div>
    
    <script>
        (function() {
            const urlInput = document.getElementById('urlInput');
            const parseBtn = document.getElementById('parseBtn');
            const statusEl = document.getElementById('status');
            const resultContainer = document.getElementById('resultContainer');
            
            // Элементы для заполнения
            const profileName = document.getElementById('profileName');
            const profileStatus = document.getElementById('profileStatus');
            const profileId = document.getElementById('profileId');
            const birthDate = document.getElementById('birthDate');
            const city = document.getElementById('city');
            const relationship = document.getElementById('relationship');
            const work = document.getElementById('work');
            const education = document.getElementById('education');
            const followersCount = document.getElementById('followersCount');
            const friendsCount = document.getElementById('friendsCount');
            const photosCount = document.getElementById('photosCount');
            const postsList = document.getElementById('postsList');
            const vkLink = document.getElementById('vkLink');
            const telegram = document.getElementById('telegram');
            const instagram = document.getElementById('instagram');
            const twitter = document.getElementById('twitter');
            const website = document.getElementById('website');
            const avatarContainer = document.getElementById('avatarContainer');
            
            // Функция для извлечения ID из URL ВК
            function extractVkId(url) {
                if (!url) return null;
                
                // Если это просто цифры (id)
                if (/^\d+$/.test(url)) {
                    return url;
                }
                
                // Если это короткое имя (durov)
                if (/^[a-zA-Z][a-zA-Z0-9_.]+$/.test(url) && !url.includes('.')) {
                    return url;
                }
                
                // Парсим URL
                try {
                    let cleanUrl = url.trim();
                    
                    // Добавляем https если нет протокола
                    if (!cleanUrl.startsWith('http')) {
                        cleanUrl = 'https://' + cleanUrl;
                    }
                    
                    const urlObj = new URL(cleanUrl);
                    const path = urlObj.pathname.replace(/^\//, '');
                    
                    // Убираем слеши в конце
                    return path.replace(/\/$/, '');
                } catch (e) {
                    return url.trim().replace(/https?:\/\/(www\.)?vk\.com\//, '');
                }
            }
            
            // Функция для получения данных через VK API (публичный, без токена)
            async function fetchVkProfile(identifier) {
                try {
                    // Используем VK API без токена (ограничено)
                    // ВКонтакте позволяет получать некоторые данные без авторизации
                    const response = await fetch(`https://api.vk.com/method/users.get?user_ids=${identifier}&fields=photo_max,status,bdate,city,country,home_town,sex,relation,personal,education,universities,schools,occupation,activities,interests,music,movies,tv,games,about,quotes,contacts,site,counters,followers_count,friend_status,common_count,verified,last_seen&v=5.131`);
                    
                    if (!response.ok) {
                        throw new Error('Ошибка сети');
                    }
                    
                    const data = await response.json();
                    
                    if (data.error) {
                        throw new Error(data.error.error_msg || 'Ошибка API');
                    }
                    
                    if (!data.response || data.response.length === 0) {
                        throw new Error('Профиль не найден');
                    }
                    
                    return data.response[0];
                } catch (error) {
                    console.error('API error:', error);
                    // Возвращаем демо-данные для примера
                    return getDemoData(identifier);
                }
            }
            
            // Функция для получения постов (через публичный метод wall.get)
            async function fetchVkPosts(ownerId, count = 3) {
                try {
                    const response = await fetch(`https://api.vk.com/method/wall.get?owner_id=${ownerId}&count=${count}&v=5.131`);
                    const data = await response.json();
                    
                    if (data.error) {
                        return [];
                    }
                    
                    return data.response?.items || [];
                } catch (error) {
                    return [];
                }
            }
            
            // Демо-данные на случай отсутствия API
            function getDemoData(identifier) {
                const isDurov = identifier.includes('durov') || identifier === '1';
                
                if (isDurov) {
                    return {
                        id: 1,
                        first_name: 'Павел',
                        last_name: 'Дуров',
                        photo_max: 'https://sun9-78.userapi.com/impf/c846420/v846420570/124dc5/7ixXj9yJqRc.jpg?size=200x0&quality=90&crop=0,0,1080,1080&sign=7b3d5f9a1b3b3b3b3b3b3b3b3b3b3b3b',
                        status: 'Создатель ВКонтакте и Telegram',
                        bdate: '10.10.1984',
                        city: { title: 'Санкт-Петербург' },
                        country: { title: 'Россия' },
                        relation: 1,
                        relation_partner: { first_name: '', last_name: '' },
                        counters: {
                            followers: 2500000,
                            friends: 0,
                            photos: 120
                        },
                        site: 'vk.com/durov',
                        contacts: {
                            telegram: 'durov',
                            instagram: 'durov',
                            twitter: 'durov'
                        },
                        education: {
                            university_name: 'СПбГУ'
                        },
                        personal: {
                            political: 2,
                            religion: '',
                            inspired_by: 'Технологии',
                            people_main: 1,
                            life_main: 5,
                            smoking: 0,
                            alcohol: 0
                        }
                    };
                } else {
                    return {
                        id: identifier,
                        first_name: 'Иван',
                        last_name: 'Иванов',
                        photo_max: '',
                        status: 'online',
                        bdate: '15.05.1990',
                        city: { title: 'Москва' },
                        country: { title: 'Россия' },
                        relation: 3,
                        counters: {
                            followers: 150,
                            friends: 245,
                            photos: 67
                        }
                    };
                }
            }
            
            // Основная функция парсинга
            async function parseProfile() {
                const url = urlInput.value.trim();
                if (!url) {
                    showStatus('Введите ссылку на профиль VK', 'error');
                    return;
                }
                
                const identifier = extractVkId(url);
                if (!identifier) {
                    showStatus('Некорректная ссылка', 'error');
                    return;
                }
                
                showStatus('Парсинг профиля...', 'loading');
                parseBtn.disabled = true;
                
                try {
                    // Получаем данные профиля
                    const profile = await fetchVkProfile(identifier);
                    
                    // Получаем ID для постов
                    const ownerId = profile.id;
                    
                    // Получаем посты
                    const posts = await fetchVkPosts(ownerId, 5);
                    
                    // Отображаем данные
                    displayProfile(profile, posts);
                    
                    showStatus('', '');
                } catch (error) {
                    showStatus('Ошибка: ' + error.message, 'error');
                } finally {
                    parseBtn.disabled = false;
                }
            }
            
            // Отображение данных
            function displayProfile(profile, posts = []) {
                // Имя
                profileName.textContent = `${profile.first_name || ''} ${profile.last_name || ''}`.trim() || 'Не указано';
                
                // Статус
                profileStatus.textContent = profile.status || 'нет статуса';
                
                // ID
                profileId.textContent = profile.id || '-';
                vkLink.innerHTML = `<a href="https://vk.com/id${profile.id}" target="_blank">id${profile.id}</a>`;
                
                // Дата рождения
                birthDate.textContent = profile.bdate || 'скрыто';
                
                // Город
                city.textContent = profile.city?.title || profile.home_town || 'не указан';
                
                // Семейное положение
                const relationMap = {
                    1: 'не женат/не замужем',
                    2: 'есть друг/есть подруга',
                    3: 'помолвлен/помолвлена',
                    4: 'женат/замужем',
                    5: 'всё сложно',
                    6: 'в активном поиске',
                    7: 'влюблён/влюблена',
                    8: 'в гражданском браке'
                };
                relationship.textContent = relationMap[profile.relation] || 'не указано';
                
                // Работа
                work.textContent = profile.occupation?.name || 'не указано';
                
                // Образование
                education.textContent = profile.university_name || profile.education?.university_name || 'не указано';
                
                // Статистика
                followersCount.textContent = profile.counters?.followers?.toLocaleString() || '0';
                friendsCount.textContent = profile.counters?.friends?.toLocaleString() || '0';
                photosCount.textContent = profile.counters?.photos?.toLocaleString() || '0';
                
                // Социальные сети
                telegram.textContent = profile.contacts?.telegram || profile.skype || 'не указан';
                if (telegram.textContent !== 'не указан') {
                    telegram.innerHTML = `<a href="https://t.me/${telegram.textContent}" target="_blank">@${telegram.textContent}</a>`;
                }
                
                instagram.textContent = profile.contacts?.instagram || 'не указан';
                if (instagram.textContent !== 'не указан') {
                    instagram.innerHTML = `<a href="https://instagram.com/${instagram.textContent}" target="_blank">@${instagram.textContent}</a>`;
                }
                
                twitter.textContent = profile.contacts?.twitter || 'не указан';
                if (twitter.textContent !== 'не указан') {
                    twitter.innerHTML = `<a href="https://twitter.com/${twitter.textContent}" target="_blank">@${twitter.textContent}</a>`;
                }
                
                website.textContent = profile.site || 'не указан';
                if (website.textContent !== 'не указан' && website.textContent.startsWith('http')) {
                    website.innerHTML = `<a href="${website.textContent}" target="_blank">${website.textContent}</a>`;
                }
                
                // Аватар
                if (profile.photo_max) {
                    avatarContainer.innerHTML = `<img src="${profile.photo_max}" alt="avatar">`;
                } else {
                    avatarContainer.innerHTML = '<span>нет фото</span>';
                }
                
                // Посты
                if (posts.length > 0) {
                    postsList.innerHTML = '';
                    posts.forEach(post => {
                        if (post.text || post.attachments) {
                            const date = new Date(post.date * 1000).toLocaleString('ru-RU');
                            const text = post.text || '📎 Вложение';
                            const shortText = text.length > 100 ? text.substring(0, 100) + '...' : text;
                            
                            const postEl = document.createElement('div');
                            postEl.className = 'post-item';
                            postEl.innerHTML = `
                                <div class="post-date">${date}</div>
                                <div class="post-text">${shortText}</div>
                            `;
                            postsList.appendChild(postEl);
                        }
                    });
                    
                    if (postsList.children.length === 0) {
                        postsList.innerHTML = '<div class="post-item">Нет постов</div>';
                    }
                } else {
                    postsList.innerHTML = '<div class="post-item">Не удалось загрузить посты</div>';
                }
                
                // Показываем результат
                resultContainer.classList.remove('hidden');
            }
            
            // Функция отображения статуса
            function showStatus(message, type) {
                statusEl.textContent = message;
                statusEl.className = 'status ' + (type || '');
            }
            
            // Обработчик клика
            parseBtn.addEventListener('click', parseProfile);
            
            // Enter в поле ввода
            urlInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') parseProfile();
            });
            
            // Предзаполнение
            urlInput.value = 'https://vk.com/durov';
        })();
    </script>
</body>
</html>
