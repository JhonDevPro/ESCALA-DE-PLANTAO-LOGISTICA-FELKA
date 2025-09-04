<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Controle de Plantões - Calendário</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 10px;
            transition: background-color 0.3s, color 0.3s;
        }
        .light-theme {
            background-color: #e6f0ff;
            color: #000000;
        }
        .dark-theme {
            background-color: #0a1127;
            color: #000000;
        }
        h1 {
            text-align: center;
            font-size: 1em;
            margin-bottom: 10px;
            text-transform: uppercase;
            padding: 8px;
            border-radius: 5px;
        }
        .light-theme h1 {
            background-color: #4b6cb7;
            color: white;
        }
        .dark-theme h1 {
            background-color: #1e3a8a;
            color: #e6f0ff;
        }
        .calendars {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
        }
        .month-calendar {
            margin: 10px;
            border-radius: 8px;
            overflow: hidden;
        }
        .calendar {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 1px;
            max-width: 200px;
            margin: 0 auto;
        }
        .calendar-header {
            text-align: center;
            font-weight: bold;
            padding: 3px;
            font-size: 0.7em;
            background-color: #4b6cb7;
            color: white;
        }
        .dark-theme .calendar-header {
            background-color: #1e3a8a;
        }
        .calendar-day {
            padding: 3px;
            text-align: center;
            border: 1px solid #ccc;
            background-color: #fff;
            min-height: 20px;
            font-size: 0.6em;
            cursor: pointer;
            color: #000000;
            border-radius: 4px;
            position: relative;
        }
        .dark-theme .calendar-day {
            background-color: #e6f0ff;
            border-color: #4b6cb7;
            color: #000000;
        }
        .calendar-day.empty {
            background-color: #f0f0f0;
            cursor: default;
        }
        .dark-theme .calendar-day.empty {
            background-color: #d4e2ff;
        }
        .calendar-day.current {
            background-color: #b3c7ff;
            font-weight: bold;
        }
        .dark-theme .calendar-day.current {
            background-color: #93c5fd;
        }
        .calendar-day.holiday {
            background-color: #fffacd;
        }
        .dark-theme .calendar-day.holiday {
            background-color: #b8860b;
        }
        .calendar-day.selected {
            background-color: #ffff00 !important;
        }
        .dark-theme .calendar-day.selected {
            background-color: #ffff00 !important;
        }
        .calendar-day span {
            display: block;
            margin-top: 1px;
            font-size: 0.55em;
            color: #000000;
        }
        .calendar-day.note::after {
            content: '📝';
            position: absolute;
            top: 2px;
            right: 2px;
            font-size: 0.5em;
        }
        .calendar-day.note:hover::before {
            content: attr(data-note);
            position: absolute;
            top: 100%;
            left: 50%;
            transform: translateX(-50%);
            background: #333;
            color: white;
            padding: 5px;
            border-radius: 3px;
            font-size: 0.7em;
            max-width: 150px;
            z-index: 10;
            white-space: pre-wrap;
        }
        #theme-toggle {
            position: fixed;
            top: 10px;
            right: 10px;
            padding: 5px 10px;
            border: none;
            border-radius: 3px;
            cursor: pointer;
            background-color: #4b6cb7;
            color: white;
            font-size: 1em;
            transition: background-color 0.3s;
        }
        .theme-toggle:hover {
            background-color: #3b82f6;
        }
        .dark-theme #theme-toggle {
            background-color: #60a5fa;
        }
        .dark-theme #theme-toggle:hover {
            background-color: #93c5fd;
        }
        .month-label {
            grid-column: span 7;
            text-align: center;
            font-size: 0.8em;
            padding: 3px;
            background-color: #4b6cb7;
            color: white;
        }
        .dark-theme .month-label {
            background-color: #1e3a8a;
        }
        #swap-controls, #filter-controls, #export-controls, #backup-controls {
            text-align: center;
            margin: 10px 0;
        }
        #swap-controls button, #filter-controls button, #export-controls button, #backup-controls button {
            padding: 5px 10px;
            margin: 5px;
            background-color: #4b6cb7;
            color: white;
            border: none;
            border-radius: 3px;
            cursor: pointer;
        }
        #swap-controls button:hover, #filter-controls button:hover, #export-controls button:hover, #backup-controls button:hover {
            background-color: #3b82f6;
        }
        .dark-theme #swap-controls button, .dark-theme #filter-controls button, .dark-theme #export-controls button, .dark-theme #backup-controls button {
            background-color: #60a5fa;
        }
        .dark-theme #swap-controls button:hover, .dark-theme #filter-controls button:hover, .dark-theme #export-controls button:hover, .dark-theme #backup-controls button:hover {
            background-color: #93c5fd;
        }
        #filter-controls button.active {
            background-color: #3b82f6;
        }
        .dark-theme #filter-controls button.active {
            background-color: #93c5fd;
        }
        #next-shift {
            text-align: center;
            margin: 10px 0;
            font-size: 0.9em;
            padding: 10px;
            background-color: #f0f0f0;
            border-radius: 5px;
        }
        .dark-theme #next-shift {
            background-color: #d4e2ff;
        }
        #modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }
        #modal-content {
            padding: 20px;
            border-radius: 5px;
            text-align: center;
            width: 300px;
        }
        .light-theme #modal-content {
            background: white;
            color: #000000;
        }
        .dark-theme #modal-content {
            background: #e6f0ff;
            color: #000000;
        }
        #modal-date {
            font-size: 1em;
            margin-bottom: 10px;
        }
        #modal-name {
            font-size: 1.2em;
            margin-bottom: 10px;
        }
        #modal-note {
            width: 100%;
            height: 100px;
            margin-bottom: 10px;
            resize: none;
            font-size: 0.9em;
        }
        #modal-save, #modal-delete, #modal-close {
            padding: 5px 10px;
            margin: 5px;
            background-color: #4b6cb7;
            color: white;
            border: none;
            border-radius: 3px;
            cursor: pointer;
        }
        #modal-save:hover, #modal-delete:hover, #modal-close:hover {
            background-color: #3b82f6;
        }
        .dark-theme #modal-save, .dark-theme #modal-delete, .dark-theme #modal-close {
            background-color: #60a5fa;
        }
        .dark-theme #modal-save:hover, .dark-theme #modal-delete:hover, .dark-theme #modal-close:hover {
            background-color: #93c5fd;
        }
        #backup-file {
            display: none;
        }
    </style>
</head>
<body class="light-theme">
    <button id="theme-toggle">🌙</button>
    <h1>ESCALA DE PLANTÃO LOGÍSTICA FELKA</h1>
    <div id="next-shift"></div>
    <div id="filter-controls">
        <button onclick="filterBy('all')" class="active">Todos</button>
        <button onclick="filterBy('Kauan')">Kauan</button>
        <button onclick="filterBy('Luan')">Luan</button>
        <button onclick="filterBy('Jhony')">Jhony</button>
    </div>
    <div id="swap-controls">
        <button onclick="swapWith('Kauan')">Trocar com Kauan</button>
        <button onclick="swapWith('Luan')">Trocar com Luan</button>
        <button onclick="swapWith('Jhony')">Trocar com Jhony</button>
    </div>
    <div id="export-controls">
        <button onclick="exportToPDF()">Exportar Escala (PDF)</button>
    </div>
    <div id="backup-controls">
        <button onclick="backupState()">Exportar Backup</button>
        <label for="backup-file">Importar Backup</label>
        <input type="file" id="backup-file" accept=".json" onchange="restoreState(event)">
    </div>
    <div class="calendars" id="calendars"></div>

    <div id="modal">
        <div id="modal-content">
            <div id="modal-date"></div>
            <div id="modal-name"></div>
            <textarea id="modal-note" placeholder="Digite sua nota aqui..."></textarea>
            <button id="modal-save">Salvar</button>
            <button id="modal-delete">Apagar</button>
            <button id="modal-close" onclick="closeModal()">Fechar</button>
        </div>
    </div>

    <script>
        const { jsPDF } = window.jspdf;
        const calendarsContainer = document.getElementById('calendars');
        const themeToggle = document.getElementById('theme-toggle');
        const body = document.body;
        const modal = document.getElementById('modal');
        const modalDate = document.getElementById('modal-date');
        const modalName = document.getElementById('modal-name');
        const modalNote = document.getElementById('modal-note');
        const modalSave = document.getElementById('modal-save');
        const modalDelete = document.getElementById('modal-delete');
        const nextShift = document.getElementById('next-shift');

        // Toggle theme between light and dark
        themeToggle.addEventListener('click', () => {
            if (body.classList.contains('light-theme')) {
                body.classList.remove('light-theme');
                body.classList.add('dark-theme');
                themeToggle.textContent = '☀️';
            } else {
                body.classList.remove('dark-theme');
                body.classList.add('light-theme');
                themeToggle.textContent = '🌙';
            }
        });

        function closeModal() {
            modal.style.display = 'none';
            modalNote.value = '';
            modalDate.textContent = '';
            modalName.textContent = '';
        }

        // Holidays list
        const holidays = [
            '2025-01-01', '2025-03-03', '2025-03-04', '2025-04-18', '2025-04-21', '2025-05-01', '2025-06-19',
            '2025-09-07', '2025-10-12', '2025-11-02', '2025-11-15', '2025-11-20', '2025-12-25',
            '2026-01-01', '2026-02-16', '2026-02-17', '2026-04-03', '2026-04-21', '2026-05-01', '2026-06-04',
            '2026-09-07', '2026-10-12', '2026-11-02', '2026-11-15', '2026-11-20', '2026-12-25'
        ];

        // Function to get the Saturday of the week
        function getSaturday(date) {
            const d = new Date(date);
            const day = d.getDay();
            if (day === 0) {
                d.setDate(d.getDate() - 1);
            } else {
                d.setDate(d.getDate() + (6 - day));
            }
            return d;
        }

        // Load saved state from localStorage
        let names = ['Kauan', 'Jhony', 'Luan'];
        let breakpoint = Infinity;
        let newPhase = 0;
        let selected_week = null;
        let notes = {};
        let currentFilter = 'all';

        function loadState() {
            const savedState = localStorage.getItem('rosterState');
            if (savedState) {
                const state = JSON.parse(savedState);
                names = state.names || ['Kauan', 'Jhony', 'Luan'];
                breakpoint = state.breakpoint !== undefined ? state.breakpoint : Infinity;
                newPhase = state.newPhase || 0;
                notes = state.notes || {};
            }
        }

        // Save state to localStorage
        function saveState() {
            const state = {
                names: names,
                breakpoint: breakpoint,
                newPhase: newPhase,
                notes: notes
            };
            localStorage.setItem('rosterState', JSON.stringify(state));
        }

        // Generate calendar starting from the upcoming weekend relative to reference date
        const referenceDate = new Date('2025-09-03');
        const refDay = referenceDate.getDay();
        const daysUntilSaturday = (6 - refDay + 7) % 7 || 7;
        const startDate = new Date(referenceDate);
        startDate.setDate(referenceDate.getDate() + daysUntilSaturday); // Fixed start: September 6, 2025

        const today = new Date(); // Dynamic current date for highlighting and starting month

        const monthsToShow = 16;
        const daysOfWeek = ['Dom', 'Seg', 'Ter', 'Qua', 'Qui', 'Sex', 'Sáb'];

        let currentDate = new Date(today);
        currentDate.setDate(1);

        function renderNextShift() {
            let nextShiftDate = null;
            let nextShiftName = '';
            let nextHolidayDate = null;

            const checkDate = new Date(today);
            for (let i = 0; i < 365; i++) {
                const dateStr = checkDate.toISOString().slice(0, 10);
                if (holidays.includes(dateStr) && !nextHolidayDate) {
                    nextHolidayDate = checkDate;
                }

                const sat = getSaturday(checkDate);
                const weeksDiff = Math.floor((sat - startDate) / (7 * 24 * 60 * 60 * 1000));
                if (weeksDiff >= 0 && (checkDate.getDay() === 6 || checkDate.getDay() === 0) && !nextShiftDate) {
                    const mod = weeksDiff < breakpoint ? weeksDiff % 3 : (weeksDiff + newPhase) % 3;
                    nextShiftDate = checkDate;
                    nextShiftName = names[mod];
                }

                if (nextShiftDate && nextHolidayDate) break;
                checkDate.setDate(checkDate.getDate() + 1);
            }

            let message = '';
            if (nextShiftDate) {
                message += `Próximo plantão: ${nextShiftName} em ${nextShiftDate.toLocaleDateString('pt-BR')}`;
            }
            if (nextHolidayDate) {
                message += `${message ? '<br>' : ''}Próximo feriado: ${nextHolidayDate.toLocaleDateString('pt-BR')}`;
            }
            nextShift.innerHTML = message || 'Nenhum plantão ou feriado próximo.';
        }

        function renderCalendar() {
            calendarsContainer.innerHTML = '';

            for (let month = 0; month < monthsToShow; month++) {
                const monthContainer = document.createElement('div');
                monthContainer.className = 'month-calendar';

                const monthLabel = document.createElement('div');
                monthLabel.className = 'month-label';
                monthLabel.textContent = currentDate.toLocaleDateString('pt-BR', { month: 'long', year: 'numeric' }).replace(/^\w/, c => c.toUpperCase());
                monthContainer.appendChild(monthLabel);

                const calendarGrid = document.createElement('div');
                calendarGrid.className = 'calendar';

                // Add day headers
                daysOfWeek.forEach(day => {
                    const header = document.createElement('div');
                    header.className = 'calendar-header';
                    header.textContent = day;
                    calendarGrid.appendChild(header);
                });

                // Calculate first day of the month
                const firstDayOfMonth = new Date(currentDate.getFullYear(), currentDate.getMonth(), 1).getDay();
                const daysInMonth = new Date(currentDate.getFullYear(), currentDate.getMonth() + 1, 0).getDate();

                // Add empty days before the first day
                for (let i = 0; i < firstDayOfMonth; i++) {
                    const emptyDay = document.createElement('div');
                    emptyDay.className = 'calendar-day empty';
                    calendarGrid.appendChild(emptyDay);
                }

                // Add days of the month
                for (let day = 1; day <= daysInMonth; day++) {
                    const date = new Date(currentDate.getFullYear(), currentDate.getMonth(), day);
                    const dayDiv = document.createElement('div');
                    dayDiv.className = 'calendar-day';
                    if (date.toDateString() === today.toDateString()) {
                        dayDiv.classList.add('current');
                    }
                    let content = day;
                    const dateStr = date.toISOString().slice(0, 10);

                    let sat = null;
                    let weeksDiff = null;
                    if (date.getDay() === 6 || date.getDay() === 0) {
                        sat = date.getDay() === 6 ? new Date(date) : new Date(date.getFullYear(), date.getMonth(), day - 1);
                        weeksDiff = Math.floor((sat - startDate) / (7 * 24 * 60 * 60 * 1000));
                    }

                    if (holidays.includes(dateStr)) {
                        dayDiv.classList.add('holiday');
                        if (sat === null) {
                            sat = getSaturday(date);
                            weeksDiff = Math.floor((sat - startDate) / (7 * 24 * 60 * 60 * 1000));
                        }
                    }

                    if (sat !== null && weeksDiff >= 0) {
                        const mod = weeksDiff < breakpoint ? weeksDiff % 3 : (weeksDiff + newPhase) % 3;
                        const name = names[mod];
                        if (currentFilter === 'all' || currentFilter === name) {
                            content += `<span>${name}</span>`;
                            dayDiv.dataset.weeksDiff = weeksDiff;
                        } else {
                            content = day;
                            dayDiv.style.opacity = '0.3';
                        }
                    }

                    if (notes[dateStr]) {
                        dayDiv.classList.add('note');
                        dayDiv.dataset.note = notes[dateStr];
                    }

                    dayDiv.innerHTML = content;
                    dayDiv.dataset.date = dateStr;
                    calendarGrid.appendChild(dayDiv);

                    // Add click event for selection
                    dayDiv.addEventListener('click', () => {
                        const span = dayDiv.querySelector('span');
                        if (span) {
                            const wd = parseInt(dayDiv.dataset.weeksDiff);
                            if (selected_week === wd) {
                                selected_week = null;
                                dayDiv.classList.remove('selected');
                            } else {
                                document.querySelectorAll('.calendar-day.selected').forEach(el => el.classList.remove('selected'));
                                selected_week = wd;
                                dayDiv.classList.add('selected');
                            }
                        }
                    });

                    // Add double-click event for pop-up
                    dayDiv.addEventListener('dblclick', () => {
                        modalDate.textContent = date.toLocaleDateString('pt-BR');
                        const span = dayDiv.querySelector('span');
                        modalName.textContent = span ? span.textContent : '';
                        modalNote.value = notes[dateStr] || '';
                        modal.style.display = 'flex';
                        modal.dataset.date = dateStr;
                    });
                }

                monthContainer.appendChild(calendarGrid);
                calendarsContainer.appendChild(monthContainer);

                // Move to next month
                currentDate.setMonth(currentDate.getMonth() + 1);
            }

            // Reset currentDate after rendering
            currentDate = new Date(today);
            currentDate.setDate(1);
            renderNextShift();
        }

        function filterBy(name) {
            currentFilter = name;
            document.querySelectorAll('#filter-controls button').forEach(btn => {
                btn.classList.toggle('active', btn.textContent === name);
            });
            renderCalendar();
        }

        function swapWith(colleague) {
            if (selected_week === null) {
                return;
            }
            if (colleague === names[selected_week % 3]) return;
            const targetIndex = names.indexOf(colleague);
            if (targetIndex === -1) return;

            const currentMod = selected_week % 3;
            newPhase = (targetIndex - currentMod + 3) % 3;
            breakpoint = selected_week;
            selected_week = null;
            saveState();
            renderCalendar();
        }

        function exportToPDF() {
            const doc = new jsPDF();
            doc.setFontSize(16);
            doc.text('Escala de Plantão Logística Felka', 10, 10);
            doc.setFontSize(12);
            let y = 20;

            const start = new Date(today.getFullYear(), today.getMonth(), 1);
            const end = new Date(today.getFullYear() + 1, today.getMonth(), 0);

            doc.text('Data\tPlantonista\tNota', 10, y);
            y += 10;
            doc.setLineWidth(0.5);
            doc.line(10, y, 200, y);
            y += 5;

            for (let d = new Date(start); d <= end; d.setDate(d.getDate() + 1)) {
                const dateStr = d.toISOString().slice(0, 10);
                let name = '';
                let sat = null;
                let weeksDiff = null;

                if (d.getDay() === 6 || d.getDay() === 0) {
                    sat = d.getDay() === 6 ? new Date(d) : new Date(d.getFullYear(), d.getMonth(), d.getDate() - 1);
                    weeksDiff = Math.floor((sat - startDate) / (7 * 24 * 60 * 60 * 1000));
                }
                if (holidays.includes(dateStr) && !sat) {
                    sat = getSaturday(d);
                    weeksDiff = Math.floor((sat - startDate) / (7 * 24 * 60 * 60 * 1000));
                }

                if (sat !== null && weeksDiff >= 0) {
                    const mod = weeksDiff < breakpoint ? weeksDiff % 3 : (weeksDiff + newPhase) % 3;
                    name = names[mod];
                }

                const note = notes[dateStr] || '';
                if (name || note) {
                    const dateText = d.toLocaleDateString('pt-BR');
                    const line = `${dateText}\t${name}\t${note}`;
                    const lines = doc.splitTextToSize(line, 180);
                    doc.text(lines, 10, y);
                    y += lines.length * 7;
                    if (y > 270) {
                        doc.addPage();
                        y = 10;
                    }
                }
            }

            doc.save('escala_plantao.pdf');
        }

        function backupState() {
            const state = {
                names: names,
                breakpoint: breakpoint,
                newPhase: newPhase,
                notes: notes
            };
            const blob = new Blob([JSON.stringify(state)], { type: 'application/json' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = 'backup_escala.json';
            link.click();
        }

        function restoreState(event) {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    try {
                        const state = JSON.parse(e.target.result);
                        names = state.names || ['Kauan', 'Jhony', 'Luan'];
                        breakpoint = state.breakpoint !== undefined ? state.breakpoint : Infinity;
                        newPhase = state.newPhase || 0;
                        notes = state.notes || {};
                        saveState();
                        renderCalendar();
                    } catch (error) {
                        alert('Erro ao importar o backup. Verifique o arquivo.');
                    }
                };
                reader.readAsText(file);
            }
        }

        // Save note
        modalSave.addEventListener('click', () => {
            const dateStr = modal.dataset.date;
            const noteText = modalNote.value.trim();
            if (noteText) {
                notes[dateStr] = noteText;
            } else {
                delete notes[dateStr];
            }
            saveState();
            renderCalendar();
            closeModal();
        });

        // Delete note
        modalDelete.addEventListener('click', () => {
            const dateStr = modal.dataset.date;
            delete notes[dateStr];
            saveState();
            renderCalendar();
            closeModal();
        });

        // Load state and render on page load
        loadState();
        renderCalendar();
    </script>
</body>
</html>
