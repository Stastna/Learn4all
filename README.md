# Learn4all
<!DOCTYPE html>
<html lang="cs">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Learn4all | Výuková aplikace</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="learn4all-root">Načítám aplikaci...</div>
    <script src="app.js"></script>
</body>
</html>
body {
    background-color: #f0f2f5;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    margin: 0;
}

.l4a-card {
    background: white;
    padding: 2rem;
    border-radius: 20px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.1);
    max-width: 500px;
    width: 90%;
    text-align: center;
}

.l4a-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 15px;
    margin-top: 20px;
}

.l4a-btn {
    background: #4CAF50;
    color: white;
    border: none;
    padding: 15px;
    border-radius: 12px;
    font-weight: bold;
    cursor: pointer;
    transition: transform 0.1s;
}

.l4a-btn:active { transform: scale(0.95); }

.l4a-opt-btn {
    display: block;
    width: 100%;
    padding: 15px;
    margin: 10px 0;
    border: 2px solid #eee;
    background: white;
    border-radius: 10px;
    font-size: 1.1rem;
    cursor: pointer;
}

.l4a-opt-btn:hover { border-color: #4CAF50; background: #f9fff9; }
const tasks = [
    { id: 1, grade: "1", q: "Kolik je 5 + 2?", a: ["6", "7", "8"], c: "7" },
    { id: 2, grade: "5", q: "Které číslo je prvočíslo?", a: ["9", "13", "15"], c: "13" },
    { id: 3, grade: "Cermat 9", q: "Vypočítej stranu čtverce, je-li obsah 64 cm².", a: ["4 cm", "8 cm", "16 cm"], c: "8 cm" }
];

let state = { view: 'menu', filteredTasks: [], currentIdx: 0, score: 0 };

function render() {
    const root = document.getElementById('learn4all-root');
    if (state.view === 'menu') {
        root.innerHTML = `
            <div class="l4a-card">
                <h1>Learn4all</h1>
                <p>Vyber si úroveň:</p>
                <div class="l4a-grid">
                    <button class="l4a-btn" onclick="start('1')">1. třída</button>
                    <button class="l4a-btn" onclick="start('5')">5. třída</button>
                    <button class="l4a-btn" onclick="start('Cermat 9')">Cermat 9</button>
                </div>
            </div>`;
    } else if (state.view === 'quiz') {
        const t = state.filteredTasks[state.currentIdx];
        root.innerHTML = `
            <div class="l4a-card">
                <h3>Otázka ${state.currentIdx + 1} / ${state.filteredTasks.length}</h3>
                <h2>${t.q}</h2>
                ${t.a.map(opt => `<button class="l4a-opt-btn" onclick="check('${opt}')">${opt}</button>`).join('')}
            </div>`;
    } else {
        root.innerHTML = `
            <div class="l4a-card">
                <h2>Výsledek: ${state.score} / ${state.filteredTasks.length}</h2>
                <button class="l4a-btn" onclick="location.reload()">Zkusit znovu</button>
            </div>`;
    }
}

window.start = (g) => {
    state.filteredTasks = tasks.filter(t => t.grade === g);
    state.view = 'quiz';
    render();
};

window.check = (ans) => {
    if (ans === state.filteredTasks[state.currentIdx].c) state.score++;
    if (state.currentIdx + 1 < state.filteredTasks.length) {
        state.currentIdx++;
    } else {
        state.view = 'result';
    }
    render();
};

render();
