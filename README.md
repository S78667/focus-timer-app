<!DOCTYPE html>
<html lang="ms">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Focus Timer | Produktiviti Maksimum</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .timer-bg-work { background-color: #ef4444; transition: background-color 0.5s ease; }
        .timer-bg-break { background-color: #10b981; transition: background-color 0.5s ease; }
    </style>
</head>
<body class="timer-bg-work min-h-screen flex items-center justify-center text-white">

    <div class="bg-white/10 backdrop-blur-md p-8 rounded-3xl shadow-2xl w-full max-w-md text-center border border-white/20">
        
        <div class="flex justify-center gap-4 mb-8">
            <button onclick="setMode('work')" id="btn-work" class="px-4 py-2 rounded-lg bg-white/20 font-bold">Focus</button>
            <button onclick="setMode('break')" id="btn-break" class="px-4 py-2 rounded-lg hover:bg-white/10">Break</button>
        </div>

        <h1 id="timer-display" class="text-8xl font-bold mb-8 tracking-tighter">25:00</h1>

        <div class="flex flex-col gap-4">
            <button id="start-btn" onclick="toggleTimer()" class="bg-white text-red-500 hover:bg-gray-100 text-xl font-bold py-4 rounded-2xl transition-all active:scale-95 shadow-lg">
                START
            </button>
            
            <button onclick="resetTimer()" class="text-white/70 hover:text-white uppercase tracking-widest text-sm font-semibold">
                Reset
            </button>
        </div>

        <div class="mt-8 pt-8 border-t border-white/10">
            <p id="status-text" class="text-lg opacity-90">Masa untuk fokus!</p>
        </div>
    </div>

    <script>
        let timeLeft = 25 * 60;
        let timerId = null;
        let isRunning = false;
        let currentMode = 'work';

        const display = document.getElementById('timer-display');
        const startBtn = document.getElementById('start-btn');
        const statusText = document.getElementById('status-text');
        const body = document.body;

        function updateDisplay() {
            const minutes = Math.floor(timeLeft / 60);
            const seconds = timeLeft % 60;
            display.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
            document.title = `${display.textContent} - Focus Timer`;
        }

        function toggleTimer() {
            if (isRunning) {
                clearInterval(timerId);
                startBtn.textContent = 'START';
                startBtn.classList.replace('text-gray-500', 'text-red-500');
            } else {
                timerId = setInterval(() => {
                    timeLeft--;
                    updateDisplay();
                    if (timeLeft <= 0) {
                        clearInterval(timerId);
                        alert("Masa tamat!");
                        resetTimer();
                    }
                }, 1000);
                startBtn.textContent = 'PAUSE';
            }
            isRunning = !isRunning;
        }

        function resetTimer() {
            clearInterval(timerId);
            isRunning = false;
            timeLeft = currentMode === 'work' ? 25 * 60 : 5 * 60;
            startBtn.textContent = 'START';
            updateDisplay();
        }

        function setMode(mode) {
            currentMode = mode;
            clearInterval(timerId);
            isRunning = false;
            startBtn.textContent = 'START';
            
            if (mode === 'work') {
                timeLeft = 25 * 60;
                body.className = 'timer-bg-work min-h-screen flex items-center justify-center text-white';
                statusText.textContent = "Masa untuk fokus!";
                document.getElementById('btn-work').classList.add('bg-white/20');
                document.getElementById('btn-break').classList.remove('bg-white/20');
            } else {
                timeLeft = 5 * 60;
                body.className = 'timer-bg-break min-h-screen flex items-center justify-center text-white';
                statusText.textContent = "Rehat sebentar...";
                document.getElementById('btn-break').classList.add('bg-white/20');
                document.getElementById('btn-work').classList.remove('bg-white/20');
            }
            updateDisplay();
        }

        updateDisplay();
    </script>
</body>
</html>
