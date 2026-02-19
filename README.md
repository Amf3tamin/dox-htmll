<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
    <title>СВЯТОЙ TO-DO СПИСОК ТЕХНОЖРЕЦА</title>
    <style>
        /* Минимальный визуал - только то, что необходимо для работы */
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: #000000;
            color: #00ff41;
            padding: 10px;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }
        
        .container {
            width: 100%;
            max-width: 480px;
            margin: 0 auto;
            flex: 1;
        }
        
        /* Заголовок - только текст */
        h1 {
            font-size: 24px;
            font-weight: normal;
            margin: 10px 0 15px 0;
            padding-bottom: 5px;
            border-bottom: 1px solid #00ff41;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        
        /* Поле ввода */
        .input-group {
            display: flex;
            gap: 5px;
            margin-bottom: 20px;
            width: 100%;
        }
        
        #taskInput {
            flex: 1;
            padding: 12px 8px;
            background: #111111;
            border: 1px solid #00ff41;
            color: #00ff41;
            font-size: 16px;
            outline: none;
            border-radius: 0;
            -webkit-appearance: none;
        }
        
        #addBtn {
            width: 60px;
            background: #111111;
            border: 1px solid #00ff41;
            color: #00ff41;
            font-size: 24px;
            font-weight: bold;
            cursor: pointer;
            border-radius: 0;
            -webkit-appearance: none;
        }
        
        #addBtn:active {
            background: #00ff41;
            color: #000000;
        }
        
        /* Статистика */
        .stats {
            display: flex;
            justify-content: space-between;
            margin-bottom: 15px;
            font-size: 14px;
            padding: 8px 0;
            border-top: 1px solid #00ff41;
            border-bottom: 1px solid #00ff41;
        }
        
        /* Список задач */
        #todoList {
            list-style: none;
            margin-bottom: 20px;
        }
        
        .todo-item {
            display: flex;
            align-items: center;
            padding: 10px 5px;
            border-bottom: 1px dotted #00ff41;
            gap: 8px;
            font-size: 16px;
            line-height: 1.3;
            word-break: break-word;
        }
        
        .todo-check {
            width: 20px;
            height: 20px;
            min-width: 20px;
            background: transparent;
            border: 1px solid #00ff41;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #000000;
            font-size: 16px;
            -webkit-appearance: none;
            appearance: none;
        }
        
        .todo-check:checked {
            background: #00ff41;
            color: #000000;
        }
        
        .todo-check:checked::after {
            content: "✓";
        }
        
        .todo-text {
            flex: 1;
            cursor: pointer;
        }
        
        .todo-text.completed {
            text-decoration: line-through;
            color: #00aa41;
            opacity: 0.7;
        }
        
        .todo-delete {
            width: 30px;
            min-width: 30px;
            text-align: center;
            background: transparent;
            border: 1px solid #00ff41;
            color: #00ff41;
            font-size: 18px;
            cursor: pointer;
            padding: 2px 0;
            -webkit-appearance: none;
        }
        
        .todo-delete:active {
            background: #00ff41;
            color: #000000;
        }
        
        /* Кнопки управления */
        .actions {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
            margin-top: 15px;
            padding-top: 10px;
            border-top: 1px solid #00ff41;
        }
        
        .action-btn {
            flex: 1 1 auto;
            min-width: 70px;
            padding: 8px 2px;
            background: #111111;
            border: 1px solid #00ff41;
            color: #00ff41;
            font-size: 14px;
            cursor: pointer;
            text-align: center;
            -webkit-appearance: none;
        }
        
        .action-btn:active {
            background: #00ff41;
            color: #000000;
        }
        
        /* Только для отладки - версия */
        .version {
            margin-top: 15px;
            text-align: right;
            font-size: 10px;
            opacity: 0.5;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>ЗАДАЧИ ВО ИМЯ ИМПЕРАТОРА</h1>
        
        <div class="input-group">
            <input type="text" id="taskInput" placeholder="новая задача...">
            <button id="addBtn">+</button>
        </div>
        
        <div class="stats">
            <span id="totalCount">всего: 0</span>
            <span id="activeCount">активных: 0</span>
            <span id="completedCount">выполнено: 0</span>
        </div>
        
        <ul id="todoList">
            <!-- Список задач будет здесь -->
        </ul>
        
        <div class="actions">
            <button class="action-btn" id="showAllBtn">все</button>
            <button class="action-btn" id="showActiveBtn">активные</button>
            <button class="action-btn" id="showCompletedBtn">выполн</button>
            <button class="action-btn" id="clearCompletedBtn">очистить</button>
            <button class="action-btn" id="sortBtn">сортировка</button>
        </div>
        
        <div class="version">TO-DO LIST V1.0 // ОДОБРЕНО ТЕХНОЖРЕЦОМ // ДАННЫЕ ТОЛЬКО НА УСТРОЙСТВЕ</div>
    </div>
    
    <script>
        (function() {
            // ДУХ МАШИНЫ: ТОЛЬКО НАСТОЯЩАЯ ИНФОРМАЦИЯ, НИКАКОЙ ЛЖИ
            console.log("Дух машины активирован. Омниссия благословляет этот код.");
            
            // === РАБОЧИЙ КОД ===
            
            // Ключ для хранения в localStorage
            const STORAGE_KEY = 'warhammer_todo_list';
            
            // Состояние приложения
            let tasks = [];
            let currentFilter = 'all'; // 'all', 'active', 'completed'
            
            // DOM элементы
            const todoList = document.getElementById('todoList');
            const taskInput = document.getElementById('taskInput');
            const addBtn = document.getElementById('addBtn');
            const totalSpan = document.getElementById('totalCount');
            const activeSpan = document.getElementById('activeCount');
            const completedSpan = document.getElementById('completedCount');
            
            // Кнопки фильтров
            const showAllBtn = document.getElementById('showAllBtn');
            const showActiveBtn = document.getElementById('showActiveBtn');
            const showCompletedBtn = document.getElementById('showCompletedBtn');
            const clearCompletedBtn = document.getElementById('clearCompletedBtn');
            const sortBtn = document.getElementById('sortBtn');
            
            // Загрузка данных из localStorage
            function loadTasks() {
                try {
                    const saved = localStorage.getItem(STORAGE_KEY);
                    if (saved) {
                        tasks = JSON.parse(saved);
                    } else {
                        // Начальные данные для примера
                        tasks = [
                            { id: Date.now() - 10000, text: 'Прославить Императора', completed: false },
                            { id: Date.now() - 9000, text: 'Смазать сервоприводы', completed: true },
                            { id: Date.now() - 8000, text: 'Проверить когитатор', completed: false },
                            { id: Date.now() - 7000, text: 'Очистить вентиляцию', completed: false }
                        ];
                    }
                } catch (e) {
                    tasks = [];
                }
            }
            
            // Сохранение данных в localStorage
            function saveTasks() {
                try {
                    localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks));
                } catch (e) {}
            }
            
            // Обновление статистики
            function updateStats() {
                const total = tasks.length;
                const active = tasks.filter(t => !t.completed).length;
                const completed = tasks.filter(t => t.completed).length;
                
                totalSpan.textContent = `всего: ${total}`;
                activeSpan.textContent = `активных: ${active}`;
                completedSpan.textContent = `выполнено: ${completed}`;
            }
            
            // Получение отфильтрованного списка задач
            function getFilteredTasks() {
                if (currentFilter === 'active') {
                    return tasks.filter(t => !t.completed);
                } else if (currentFilter === 'completed') {
                    return tasks.filter(t => t.completed);
                } else {
                    return [...tasks];
                }
            }
            
            // Отрисовка списка задач
            function renderTasks() {
                const filtered = getFilteredTasks();
                
                if (filtered.length === 0) {
                    todoList.innerHTML = '<li style="padding:15px; text-align:center; opacity:0.6; border-bottom:1px dotted #00ff41;">задач нет</li>';
                } else {
                    let html = '';
                    
                    filtered.forEach(task => {
                        const completedClass = task.completed ? 'completed' : '';
                        const checkedAttr = task.completed ? 'checked' : '';
                        
                        html += `
                            <li class="todo-item" data-task-id="${task.id}">
                                <input type="checkbox" class="todo-check" ${checkedAttr} data-id="${task.id}">
                                <span class="todo-text ${completedClass}" data-id="${task.id}">${escapeHtml(task.text)}</span>
                                <button class="todo-delete" data-id="${task.id}">✕</button>
                            </li>
                        `;
                    });
                    
                    todoList.innerHTML = html;
                }
                
                updateStats();
            }
            
            // Экранирование HTML
            function escapeHtml(text) {
                const div = document.createElement('div');
                div.textContent = text;
                return div.innerHTML;
            }
            
            // Добавление новой задачи
            function addTask() {
                const text = taskInput.value.trim();
                if (text === '') return;
                
                const newTask = {
                    id: Date.now(),
                    text: text,
                    completed: false
                };
                
                tasks.push(newTask);
                saveTasks();
                renderTasks();
                
                taskInput.value = '';
                taskInput.focus();
            }
            
            // Переключение статуса задачи
            function toggleTask(taskId) {
                const task = tasks.find(t => t.id == taskId);
                if (task) {
                    task.completed = !task.completed;
                    saveTasks();
                    renderTasks();
                }
            }
            
            // Удаление задачи
            function deleteTask(taskId) {
                tasks = tasks.filter(t => t.id != taskId);
                saveTasks();
                renderTasks();
            }
            
            // Удаление выполненных задач
            function clearCompleted() {
                tasks = tasks.filter(t => !t.completed);
                saveTasks();
                renderTasks();
            }
            
            // Сортировка (активные сверху)
            function sortTasks() {
                tasks.sort((a, b) => {
                    // Сначала невыполненные, потом выполненные
                    if (a.completed === b.completed) return 0;
                    return a.completed ? 1 : -1;
                });
                saveTasks();
                renderTasks();
            }
            
            // Обработчики событий
            
            // Добавление через кнопку
            addBtn.addEventListener('click', addTask);
            
            // Добавление через Enter
            taskInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') addTask();
            });
            
            // Обработка кликов по списку (делегирование событий)
            todoList.addEventListener('click', (e) => {
                const target = e.target;
                
                // Чекбокс или текст для переключения статуса
                if (target.classList.contains('todo-check') || 
                    (target.classList.contains('todo-text') && !target.classList.contains('completed'))) {
                    
                    let taskId = target.dataset.id;
                    if (!taskId && target.parentElement) {
                        taskId = target.parentElement.dataset.taskId;
                    }
                    
                    if (taskId) toggleTask(taskId);
                }
                
                // Кнопка удаления
                if (target.classList.contains('todo-delete')) {
                    const taskId = target.dataset.id;
                    if (taskId) deleteTask(taskId);
                }
            });
            
            // Фильтры
            showAllBtn.addEventListener('click', () => {
                currentFilter = 'all';
                renderTasks();
            });
            
            showActiveBtn.addEventListener('click', () => {
                currentFilter = 'active';
                renderTasks();
            });
            
            showCompletedBtn.addEventListener('click', () => {
                currentFilter = 'completed';
                renderTasks();
            });
            
            clearCompletedBtn.addEventListener('click', clearCompleted);
            sortBtn.addEventListener('click', sortTasks);
            
            // Защита от случайного закрытия (не обязательно, но добавим)
            window.addEventListener('beforeunload', function() {
                // Данные уже сохранены в saveTasks()
            });
            
            // Инициализация
            loadTasks();
            renderTasks();
            
            // Благословение Омниссии
            console.log("Код выполнен. Данные сохраняются локально. Император защищает.");
        })();
    </script>
</body>
</html>
