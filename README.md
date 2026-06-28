<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Daily Journal & Habit Tracker</title>
    <style>
        :root {
            --primary: #7952b3;
            --primary-light: #efe9f7;
            --bg: #f3f0ff; 
            --card-bg: #ffffff;
            --text: #332941;
            --border: #e2d9f3;
            --success: #2ecc71;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg);
            color: var(--text);
            margin: 0;
            padding: 40px 20px;
            display: flex;
            justify-content: center;
            min-height: 100vh;
            box-sizing: border-box;
        }

        .container {
            width: 100%;
            max-width: 950px;
            display: flex;
            flex-direction: column;
            gap: 25px;
        }

        header {
            text-align: center;
        }

        h1 {
            color: var(--primary);
            margin: 0;
            font-size: 2.2rem;
            letter-spacing: -0.5px;
        }

        /* Two-Column Grid Setup for Balance */
        .main-layout {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
        }

        @media (max-width: 768px) {
            .main-layout {
                grid-template-columns: 1fr;
            }
        }

        .column {
            display: flex;
            flex-direction: column;
            gap: 25px;
        }

        .journal-card {
            background: var(--card-bg);
            padding: 25px;
            border-radius: 16px;
            box-shadow: 0 10px 25px rgba(121, 82, 179, 0.03);
            border: 1px solid var(--border);
        }

        .card-title {
            margin-top: 0;
            margin-bottom: 20px;
            font-size: 1.2rem;
            color: var(--primary);
            border-bottom: 2px solid var(--primary-light);
            padding-bottom: 8px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            font-weight: 600;
            margin-bottom: 8px;
            font-size: 14px;
        }

        textarea {
            width: 100%;
            padding: 12px;
            border: 1px solid var(--border);
            border-radius: 10px;
            font-size: 14px;
            box-sizing: border-box;
            resize: vertical;
            font-family: inherit;
            color: var(--text);
            background-color: #faf9fe;
        }

        textarea:focus {
            outline: none;
            border-color: var(--primary);
            background-color: #fff;
        }

        /* Todo Checklist styling */
        .todo-item {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 10px 14px;
            background: #faf9fe;
            border: 1px solid var(--border);
            border-radius: 10px;
            margin-bottom: 8px;
            cursor: pointer;
            transition: all 0.2s;
        }

        .todo-item input[type="checkbox"] {
            width: 18px;
            height: 18px;
            accent-color: var(--primary);
            cursor: pointer;
        }

        .todo-item.done {
            background: #fdfdfd;
            border-color: #e2e2e2;
            opacity: 0.6;
        }

        .todo-item.done span {
            text-decoration: line-through;
        }

        /* Emoji Selection */
        .emoji-group {
            display: flex;
            justify-content: space-between;
            max-width: 280px;
            font-size: 2rem;
            background: #faf9fe;
            padding: 6px 12px;
            border-radius: 10px;
            border: 1px solid var(--border);
        }

        .emoji-btn {
            cursor: pointer;
            transition: transform 0.2s;
            user-select: none;
            opacity: 0.4;
        }

        .emoji-btn.selected, .emoji-btn:hover {
            opacity: 1;
            transform: scale(1.15);
        }

        button.submit-btn {
            background-color: var(--primary);
            color: white;
            border: none;
            padding: 14px;
            font-size: 16px;
            font-weight: 600;
            border-radius: 10px;
            cursor: pointer;
            width: 100%;
            box-shadow: 0 4px 12px rgba(121, 82, 179, 0.15);
        }

        /* Calendar UI Element */
        .calendar-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 8px;
            text-align: center;
        }

        .calendar-day-name {
            font-weight: 600;
            font-size: 12px;
            color: #a395b8;
        }

        .calendar-day {
            padding: 10px 0;
            border-radius: 8px;
            cursor: pointer;
            font-size: 14px;
            font-weight: 500;
            background: #faf9fe;
            border: 1px solid transparent;
            position: relative;
            transition: all 0.2s;
        }

        .calendar-day:hover {
            background: var(--primary-light);
            color: var(--primary);
        }

        .calendar-day.has-entry::after {
            content: '';
            position: absolute;
            bottom: 4px;
            left: 50%;
            transform: translateX(-50%);
            width: 5px;
            height: 5px;
            background-color: var(--primary);
            border-radius: 50%;
        }

        .calendar-day.selected-day {
            background: var(--primary) !important;
            color: white !important;
        }

        .cal-btn {
            background: none;
            border: 1px solid var(--border);
            border-radius: 6px;
            cursor: pointer;
            padding: 4px 10px;
            color: var(--primary);
            font-weight: bold;
        }

        /* Secure Entry Output */
        .entry-display-box {
            background: #fff;
            border-radius: 16px;
            padding: 25px;
            border: 1px solid var(--border);
            min-height: 150px;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        .entry-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px dashed var(--border);
            padding-bottom: 10px;
            margin-bottom: 15px;
        }

        .entry-date { font-weight: bold; color: var(--primary); }
        .entry-emoji { font-size: 1.8rem; }
        .section-title { font-size: 11px; text-transform: uppercase; color: #a395b8; margin: 12px 0 4px 0; font-weight: 700; }
        .entry-text { margin: 0; white-space: pre-wrap; line-height: 1.5; font-size: 14px;}

        /* Built-in checklist view inside entry */
        .logged-todo {
            font-size: 13.5px;
            margin: 4px 0;
            color: #555;
        }
    </style>
</head>
<body>

<div class="container">
    <header>
        <h1>My Daily Journal</h1>
    </header>

    <div class="main-layout">
        
        <div class="column">
            <div class="journal-card">
                <h3 class="card-title">Daily Checklist</h3>
                <div id="todoContainer">
                    <div class="todo-item" onclick="toggleTodoCheck(this)"><input type="checkbox" id="td1"> <span>📖 Reading</span></div>
                    <div class="todo-item" onclick="toggleTodoCheck(this)"><input type="checkbox" id="td2"> <span>🎸 Play Guitar</span></div>
                    <div class="todo-item" onclick="toggleTodoCheck(this)"><input type="checkbox" id="td3"> <span>💻 Learn Coding</span></div>
                    <div class="todo-item" onclick="toggleTodoCheck(this)"><input type="checkbox" id="td4"> <span>💰 Record Expenses</span></div>
                </div>
            </div>

            <div class="journal-card">
                <h3 class="card-title">New Journal Entry</h3>
                <form id="journalForm">
                    <div class="form-group">
                        <label for="routine">1. My Daily Routine</label>
                        <textarea id="routine" rows="4" placeholder="What did you do today?" required></textarea>
                    </div>

                    <div class="form-group">
                        <label>2. How are you feeling?</label>
                        <div class="emoji-group">
                            <span class="emoji-btn" onclick="selectEmoji('😊', this)">😊</span>
                            <span class="emoji-btn" onclick="selectEmoji('😎', this)">😎</span>
                            <span class="emoji-btn" onclick="selectEmoji('😴', this)">😴</span>
                            <span class="emoji-btn" onclick="selectEmoji('😔', this)">😔</span>
                            <span class="emoji-btn" onclick="selectEmoji('😡', this)">😡</span>
                        </div>
                        <input type="hidden" id="selectedEmotion" value="😊">
                    </div>

                    <div class="form-group">
                        <label for="proudThings">3. Proud Moments / Good Things / Firsts</label>
                        <textarea id="proudThings" rows="3" placeholder="What made you proud today?" required></textarea>
                    </div>

                    <button type="submit" class="submit-btn">Save Today's Entry</button>
                </form>
            </div>
        </div>

        <div class="column">
            <div class="journal-card">
                <h3 class="card-title">Journal Archive Calendar</h3>
                <div class="calendar-header">
                    <button class="cal-btn" onclick="changeMonth(-1)">◀</button>
                    <span id="calendarMonthYear" style="font-weight: bold;"></span>
                    <button class="cal-btn" onclick="changeMonth(1)">▶</button>
                </div>
                <div class="calendar-grid" id="calendarGrid"></div>
            </div>

            <div class="entry-display-box" id="entryDisplay">
                <p style="text-align: center; color: #a395b8; margin: 0;">Select a date on the calendar with a purple dot to safely unlock and read your memory.</p>
            </div>
        </div>

    </div>
</div>

<script>
    const PASSWORD = "fionliew1018";
    let currentNavDate = new Date();
    let loadedEntries = {};

    // Load initial structures
    document.querySelector('.emoji-btn').classList.add('selected');
    loadDatabase();
    generateCalendar();

    function loadDatabase() {
        const localData = JSON.parse(localStorage.getItem('calJournalEntries')) || [];
        loadedEntries = {};
        localData.forEach(entry => {
            loadedEntries[entry.idString] = entry; // Key mapping by "YYYY-MM-DD"
        });
    }

    function toggleTodoCheck(element) {
        const checkbox = element.querySelector('input');
        checkbox.checked = !checkbox.checked;
        element.classList.toggle('done', checkbox.checked);
    }

    function selectEmoji(emoji, element) {
        document.querySelectorAll('.emoji-btn').forEach(btn => btn.classList.remove('selected'));
        element.classList.add('selected');
        document.getElementById('selectedEmotion').value = emoji;
    }

    // Submission Logic
    document.getElementById('journalForm').addEventListener('submit', function(e) {
        e.preventDefault();
        
        const today = new Date();
        const idString = formatDateKey(today);

        // Fetch Checklist status
        const tasks = [];
        document.querySelectorAll('.todo-item').forEach(item => {
            tasks.push({
                text: item.querySelector('span').innerText,
                done: item.querySelector('input').checked
            });
        });

        const entry = {
            idString: idString,
            dateLabel: today.toLocaleDateString('en-US', { weekday: 'short', year: 'numeric', month: 'short', day: 'numeric' }),
            routine: document.getElementById('routine').value,
            emotion: document.getElementById('selectedEmotion').value,
            proudThings: document.getElementById('proudThings').value,
            todos: tasks
        };

        // Update local memory
        let dbList = JSON.parse(localStorage.getItem('calJournalEntries')) || [];
        dbList = dbList.filter(item => item.idString !== idString); // overwrite if entry exists
        dbList.unshift(entry);
        localStorage.setItem('calJournalEntries', JSON.stringify(dbList));

        // Reset inputs
        document.getElementById('journalForm').reset();
        document.querySelectorAll('.todo-item').forEach(el => el.classList.remove('done'));
        selectEmoji('😊', document.querySelector('.emoji-btn'));

        alert("Today's journal successfully locked away in your archives!");
        loadDatabase();
        generateCalendar();
    });

    // Calendar Generation
    function generateCalendar() {
        const grid = document.getElementById('calendarGrid');
        const monthYearLabel = document.getElementById('calendarMonthYear');
        grid.innerHTML = '';

        const year = currentNavDate.getFullYear();
        const month = currentNavDate.getMonth();

        monthYearLabel.innerText = currentNavDate.toLocaleDateString('en-US', { month: 'long', year: 'numeric' });

        // Weekday Titles
        const daysOfWeek = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
        daysOfWeek.forEach(d => {
            const div = document.createElement('div');
            div.className = 'calendar-day-name';
            div.innerText = d;
            grid.appendChild(div);
        });

        const firstDayIndex = new Date(year, month, 1).getDay();
        const totalDays = new Date(year, month + 1, 0).getDate();

        // Empty block paddings
        for (let i = 0; i < firstDayIndex; i++) {
            grid.appendChild(document.createElement('div'));
        }

        // Days execution mapping
        for (let day = 1; day <= totalDays; day++) {
            const dayDiv = document.createElement('div');
            dayDiv.className = 'calendar-day';
            dayDiv.innerText = day;

            const dateKey = `${year}-${String(month + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
            
            if (loadedEntries[dateKey]) {
                dayDiv.classList.add('has-entry');
            }

            dayDiv.onclick = () => viewDayDetails(dateKey, dayDiv);
            grid.appendChild(dayDiv);
        }
    }

    function changeMonth(direction) {
        currentNavDate.setMonth(currentNavDate.getMonth() + direction);
        generateCalendar();
    }

    // Security Gate and Dynamic Rendering 
    function viewDayDetails(dateKey, element) {
        document.querySelectorAll('.calendar-day').forEach(d => d.classList.remove('selected-day'));
        element.classList.add('selected-day');

        const displayBox = document.getElementById('entryDisplay');
        const data = loadedEntries[dateKey];

        if (!data) {
            displayBox.innerHTML = `<p style="text-align: center; color: #a395b8; margin:0;">No entry recorded on this date.</p>`;
            return;
        }

        const verification = prompt("Enter password to unlock past memories:");
        if (verification === PASSWORD) {
            
            // Format checklist items for rendering
            let checklistHTML = data.todos ? data.todos.map(t => `
                <div class="logged-todo">${t.done ? '✅' : '❌'} ${t.text}</div>
            `).join('') : '<p class="entry-text" style="color:gray">No tracking records.</p>';

            displayBox.innerHTML = `
                <div class="entry-header">
                    <div class="entry-date">${data.dateLabel}</div>
                    <div class="entry-emoji">${data.emotion}</div>
                </div>
                <div>
                    <div class="section-title">Habit Performance</div>
                    ${checklistHTML}
                </div>
                <div style="margin-top:10px">
                    <div class="section-title">Daily Routine</div>
                    <p class="entry-text">${data.routine}</p>
                </div>
                <div style="margin-top:10px">
                    <div class="section-title">Proud Moments & Wins</div>
                    <p class="entry-text">${data.proudThings}</p>
                </div>
            `;
        } else if (verification !== null) {
            alert("Incorrect password. Secure block protocol kept active.");
            displayBox.innerHTML = `<p style="text-align: center; color: #e53e3e; margin: 0;">❌ Authentication Failed.</p>`;
        }
    }

    function formatDateKey(dateObj) {
        const y = dateObj.getFullYear();
        const m = String(dateObj.getMonth() + 1).padStart(2, '0');
        const d = String(dateObj.getDate()).padStart(2, '0');
        return `${y}-${m}-${d}`;
    }
</script>

</body>
</html>
