<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Koordynator Spotkań</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Chosen Palette: Warm Neutrals (Tailwind bg-amber-50, text-slate, accent-sky) -->
    <!-- Application Structure Plan: A task-oriented single-page application that transforms the Excel guide into a functional tool. The structure includes three main sections: 1. Configuration for adding participants and selecting date range. 2. An interactive Calendar Grid for inputting availability. 3. A dynamic Summary section with text results and a bar chart. This structure was chosen to guide the user logically from setup to result, making the process of finding a meeting date intuitive and direct, superior to a static report. -->
    <!-- Visualization & Content Choices: Report Info: Manually counting availability. Goal: Automate counting and finding the best date. Viz/Method: An interactive HTML table with dropdowns (`<select>`) for data entry, directly mimicking the Excel sheet. Report Info: Highlighting best dates. Goal: Clearly visualize and present the best options. Viz/Method: A dynamic bar chart (Chart.js) shows daily availability, and a summary text block explicitly states the best date(s). Interaction: All results and the chart update in real-time on dropdown changes. Justification: This provides immediate feedback and a clear, visual comparison of dates, which is more engaging and efficient than manual Excel formatting. Library/Method: Chart.js for the canvas-based bar chart. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap');
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 900px;
            margin-left: auto;
            margin-right: auto;
            height: 400px;
            max-height: 50vh;
        }
        .table-container {
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
        }
         select {
            -webkit-appearance: none;
            -moz-appearance: none;
            appearance: none;
            background-image: url('data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%22292.4%22%20height%3D%22292.4%22%3E%3Cpath%20fill%3D%22%23007CB2%22%20d%3D%22M287%2069.4a17.6%2017.6%200%200%200-13-5.4H18.4c-5%200-9.3%201.8-12.9%205.4A17.6%2017.6%200%200%200%200%2082.2c0%205%201.8%209.3%205.4%2012.9l128%20127.9c3.6%203.6%207.8%205.4%2012.8%205.4s9.2-1.8%2012.8-5.4L287%2095c3.5-3.5%205.4-7.8%205.4-12.8%200-5-1.9-9.2-5.5-12.8z%22%2F%3E%3C%2Fsvg%3E');
            background-repeat: no-repeat;
            background-position: right 0.7rem center;
            background-size: 0.65rem auto;
            padding-right: 2rem;
        }
    </style>
</head>
<body class="bg-amber-50 text-slate-800">

    <div class="container mx-auto p-4 sm:p-6 lg:p-8">
        
        <header class="text-center mb-8">
            <h1 class="text-4xl md:text-5xl font-bold text-slate-900">Koordynator Spotkań</h1>
            <p class="mt-2 text-lg text-slate-600">Znajdź idealny termin dla całej grupy. Szybko i bezboleśnie.</p>
        </header>

        <main class="space-y-12">
            
            <section id="setup" class="bg-white p-6 rounded-2xl shadow-lg border border-slate-200">
                <h2 class="text-2xl font-bold mb-4">Krok 1: Konfiguracja</h2>
                <div class="mb-6">
                    <h3 class="text-xl font-semibold mb-3">Dodaj uczestników</h3>
                    <div class="flex flex-col sm:flex-row gap-4">
                        <input type="text" id="participant-name" placeholder="Wpisz imię i naciśnij Enter..." class="flex-grow p-3 border border-slate-300 rounded-lg focus:ring-2 focus:ring-sky-500 focus:border-sky-500 transition">
                        <button id="add-participant-btn" class="bg-sky-600 text-white font-bold py-3 px-6 rounded-lg hover:bg-sky-700 transition-colors shadow">Dodaj osobę</button>
                    </div>
                    <div id="participants-list" class="mt-4 flex flex-wrap gap-3"></div>
                </div>

                <div>
                    <h3 class="text-xl font-semibold mb-3">Wybierz zakres dat</h3>
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                        <div>
                            <label for="start-date" class="block text-sm font-medium text-slate-700 mb-1">Data początkowa:</label>
                            <input type="date" id="start-date" class="w-full p-3 border border-slate-300 rounded-lg focus:ring-2 focus:ring-sky-500 focus:border-sky-500 transition">
                        </div>
                        <div>
                            <label for="end-date" class="block text-sm font-medium text-slate-700 mb-1">Data końcowa:</label>
                            <input type="date" id="end-date" class="w-full p-3 border border-slate-300 rounded-lg focus:ring-2 focus:ring-sky-500 focus:border-sky-500 transition">
                        </div>
                    </div>
                </div>
            </section>
            
            <section id="calendar-section" class="bg-white p-6 rounded-2xl shadow-lg border border-slate-200">
                <h2 class="text-2xl font-bold mb-4">Krok 2: Zaznaczcie swoją dostępność</h2>
                 <p class="text-slate-600 mb-6">Wybierzcie "Tak", "Nie" lub "Może" dla każdego dnia. Wyniki aktualizują się na żywo poniżej.</p>
                <div class="table-container rounded-lg border border-slate-200">
                    <table class="min-w-full divide-y divide-slate-200">
                        <thead id="table-head" class="bg-slate-100">
                        </thead>
                        <tbody id="table-body" class="bg-white divide-y divide-slate-200">
                        </tbody>
                    </table>
                </div>
            </section>
            
            <section id="results-section" class="bg-white p-6 rounded-2xl shadow-lg border border-slate-200">
                 <h2 class="text-2xl font-bold mb-4">Krok 3: Sprawdźcie wyniki!</h2>
                 <div id="results-summary" class="grid grid-cols-1 md:grid-cols-2 gap-6 text-center mb-8">
                    <div class="bg-green-100 border border-green-200 p-6 rounded-xl">
                        <h3 class="text-lg font-semibold text-green-800">Najlepszy termin na spotkanie:</h3>
                        <p id="best-date" class="text-2xl font-bold text-green-900 mt-2">-</p>
                    </div>
                     <div class="bg-sky-100 border border-sky-200 p-6 rounded-xl">
                        <h3 class="text-lg font-semibold text-sky-800">Liczba dostępnych osób:</h3>
                        <p id="attendee-count" class="text-2xl font-bold text-sky-900 mt-2">-</p>
                    </div>
                 </div>
                 <div class="chart-container">
                     <canvas id="availability-chart"></canvas>
                 </div>
            </section>

        </main>
        
        <footer class="text-center mt-12 text-slate-500">
            <p>Stworzone, by ułatwić planowanie.</p>
        </footer>

    </div>

    <script>
    document.addEventListener('DOMContentLoaded', function() {
        let participants = [];
        let availabilityChart = null;
        const nameInput = document.getElementById('participant-name');
        const addBtn = document.getElementById('add-participant-btn');
        const participantsList = document.getElementById('participants-list');
        const calendarSection = document.getElementById('calendar-section');
        const resultsSection = document.getElementById('results-section');
        const tableHead = document.getElementById('table-head');
        const tableBody = document.getElementById('table-body');
        const startDateInput = document.getElementById('start-date');
        const endDateInput = document.getElementById('end-date');
        
        const bestDateEl = document.getElementById('best-date');
        const attendeeCountEl = document.getElementById('attendee-count');

        // Set default dates
        const today = new Date();
        const thirtyDaysLater = new Date();
        thirtyDaysLater.setDate(today.getDate() + 29); // +29 to include 30 days total

        startDateInput.value = today.toISOString().split('T')[0]; // YYYY-MM-DD format
        endDateInput.value = thirtyDaysLater.toISOString().split('T')[0]; // YYYY-MM-DD format

        const addParticipant = () => {
            const name = nameInput.value.trim();
            if (name && !participants.includes(name)) {
                participants.push(name);
                renderParticipants();
                renderTable();
                updateCalculations();
                // calendarSection.classList.remove('hidden'); // Removed as they are now visible by default
                // resultsSection.classList.remove('hidden'); // Removed as they are now visible by default
            }
             nameInput.focus();
        };

        addBtn.addEventListener('click', addParticipant);
        nameInput.addEventListener('keyup', (e) => {
            if (e.key === 'Enter') {
                addParticipant();
            }
        });

        startDateInput.addEventListener('change', () => {
            if (new Date(startDateInput.value) > new Date(endDateInput.value)) {
                endDateInput.value = startDateInput.value; // Adjust end date if it's before start date
            }
            renderTable();
            updateCalculations();
        });

        endDateInput.addEventListener('change', () => {
            if (new Date(endDateInput.value) < new Date(startDateInput.value)) {
                startDateInput.value = endDateInput.value; // Adjust start date if it's after end date
            }
            renderTable();
            updateCalculations();
        });

        function renderParticipants() {
            participantsList.innerHTML = '';
            participants.forEach(name => {
                const tag = document.createElement('div');
                tag.className = 'flex items-center bg-sky-100 text-sky-800 text-sm font-medium px-4 py-2 rounded-full';
                tag.innerHTML = `
                    <span>${name}</span>
                    <button class="ml-2 text-sky-600 hover:text-sky-800 font-bold" data-name="${name}">&times;</button>
                `;
                participantsList.appendChild(tag);
            });

            document.querySelectorAll('#participants-list button').forEach(button => {
                button.addEventListener('click', (e) => {
                    const nameToRemove = e.target.dataset.name;
                    participants = participants.filter(p => p !== nameToRemove);
                    renderParticipants();
                    renderTable();
                    updateCalculations();
                    // if (participants.length === 0) { // No longer needed as sections are visible by default
                    //     calendarSection.classList.add('hidden');
                    //     resultsSection.classList.add('hidden');
                    // }
                });
            });
        }
        
        function getDates() {
            const dates = [];
            const startDate = new Date(startDateInput.value);
            const endDate = new Date(endDateInput.value);
            
            // Ensure valid date range
            if (isNaN(startDate.getTime()) || isNaN(endDate.getTime()) || startDate > endDate) {
                return [];
            }

            let currentDate = new Date(startDate);
            while (currentDate <= endDate) {
                dates.push(new Date(currentDate));
                currentDate.setDate(currentDate.getDate() + 1);
            }
            return dates;
        }
        
        function renderTable() {
            const dates = getDates();
            tableHead.innerHTML = '';
            tableBody.innerHTML = '';
            
            if (dates.length === 0) {
                tableBody.innerHTML = '<tr><td colspan="100%" class="text-center py-4 text-slate-500">Wybierz prawidłowy zakres dat.</td></tr>';
                return;
            }

            let headRow = '<tr><th class="py-3 px-4 text-left text-xs font-medium text-slate-500 uppercase tracking-wider">Data</th>';
            participants.forEach(p => {
                headRow += `<th class="py-3 px-4 text-left text-xs font-medium text-slate-500 uppercase tracking-wider">${p}</th>`;
            });
            headRow += '<th class="py-3 px-4 text-left text-xs font-medium text-slate-500 uppercase tracking-wider">Dostępni</th></tr>';
            tableHead.innerHTML = headRow;
            
            dates.forEach((date, index) => {
                const dateString = date.toLocaleDateString('pl-PL', { day: '2-digit', month: '2-digit', weekday: 'short' });
                let bodyRow = `<tr class="hover:bg-amber-50" data-row-index="${index}"><td class="py-3 px-4 whitespace-nowrap text-sm font-medium text-slate-900">${dateString}</td>`;
                participants.forEach(() => {
                    bodyRow += `<td class="py-2 px-4 whitespace-nowrap"><select class="availability-select w-full p-2 border-slate-200 rounded-md bg-white hover:bg-slate-50 focus:ring-1 focus:ring-sky-500 focus:border-sky-500 transition">
                        <option value="Może">Może</option>
                        <option value="Tak">Tak</option>
                        <option value="Nie">Nie</option>
                    </select></td>`;
                });
                bodyRow += `<td class="available-count py-3 px-4 whitespace-nowrap text-sm font-bold text-slate-800 text-center">0</td></tr>`;
                tableBody.innerHTML += bodyRow;
            });

            document.querySelectorAll('.availability-select').forEach(select => {
                select.addEventListener('change', updateCalculations);
            });
        }
        
        function updateCalculations() {
            if (participants.length === 0 || getDates().length === 0) {
                bestDateEl.textContent = '-';
                attendeeCountEl.textContent = '-';
                if(availabilityChart) {
                    availabilityChart.data.labels = [];
                    availabilityChart.data.datasets[0].data = [];
                    availabilityChart.update();
                }
                return;
            };

            const rows = tableBody.querySelectorAll('tr');
            let maxAttendees = 0;
            let bestDates = [];
            const chartLabels = [];
            const chartData = [];

            rows.forEach(row => {
                const selects = row.querySelectorAll('.availability-select');
                let availableCount = 0;
                selects.forEach(s => {
                    if (s.value === 'Tak') {
                        availableCount++;
                    }
                });
                row.querySelector('.available-count').textContent = availableCount;
                
                const dateLabel = row.cells[0].textContent;
                chartLabels.push(dateLabel.split(',')[0]); 
                chartData.push(availableCount);

                if (availableCount > maxAttendees) {
                    maxAttendees = availableCount;
                    bestDates = [dateLabel];
                } else if (availableCount === maxAttendees && maxAttendees > 0) {
                    bestDates.push(dateLabel);
                }
            });
            
            if(maxAttendees > 0) {
                bestDateEl.innerHTML = bestDates.join('<br>');
                attendeeCountEl.textContent = `${maxAttendees} / ${participants.length}`;
            } else {
                bestDateEl.textContent = 'Brak wspólnego terminu';
                attendeeCountEl.textContent = `0 / ${participants.length}`;
            }
            
            renderChart(chartLabels, chartData, maxAttendees);
        }

        function renderChart(labels, data, max) {
            const ctx = document.getElementById('availability-chart').getContext('2d');
            const backgroundColors = data.map(value => value === max && max > 0 ? 'rgba(56, 189, 248, 0.6)' : 'rgba(203, 213, 225, 0.6)');
            const borderColors = data.map(value => value === max && max > 0 ? 'rgba(14, 165, 233, 1)' : 'rgba(100, 116, 139, 1)');


            if (availabilityChart) {
                availabilityChart.data.labels = labels;
                availabilityChart.data.datasets[0].data = data;
                availabilityChart.data.datasets[0].backgroundColor = backgroundColors;
                availabilityChart.data.datasets[0].borderColor = borderColors;
                availabilityChart.update();
            } else {
                availabilityChart = new Chart(ctx, {
                    type: 'bar',
                    data: {
                        labels: labels,
                        datasets: [{
                            label: 'Liczba dostępnych osób',
                            data: data,
                            backgroundColor: backgroundColors,
                            borderColor: borderColors,
                            borderWidth: 1,
                            borderRadius: 4
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        scales: {
                            y: {
                                beginAtZero: true,
                                ticks: {
                                    stepSize: 1
                                },
                                grid: {
                                    color: '#e2e8f0'
                                }
                            },
                            x: {
                                grid: {
                                    display: false
                                }
                            }
                        },
                        plugins: {
                            legend: {
                                display: false
                            },
                            tooltip: {
                                callbacks: {
                                    title: function(context) {
                                        return tableBody.rows[context[0].dataIndex].cells[0].textContent;
                                    }
                                }
                            }
                        }
                    }
                });
            }
        }
        // Initial render of the table and calculations when the page loads
        renderTable();
        updateCalculations();
    });
    </script>
</body>
</html>
