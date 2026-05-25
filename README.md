<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Bouncing Text Box with Cursor Image and Guestbook</title>
    <style>
        h1, header, .site-header, .page-header, .project-name { display: none !important; }

        @keyframes cartoonSpinIn {
            0% { transform: scale(0) rotate(0deg) translate(-50%, -50%); opacity: 0; }
            60% { transform: scale(1.2) rotate(720deg) translate(-50%, -50%); opacity: 1; }
            100% { transform: scale(1) rotate(720deg) translate(-50%, -50%); opacity: 1; }
        }

        .cartoon-animate { animation: cartoonSpinIn 0.8s ease-out; }

        body {
            background-image: url("https://i.imgur.com/1d9tmQg.png");
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
            margin: 0;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            height: 100vh;
            cursor: none;
            position: relative;
        }

        .bouncing-text {
            font-family: "Comic Sans MS", cursive, sans-serif;
            font-size: 16px;
            white-space: nowrap;
            padding: 5px 10px;
            background-color: pink;
            color: black;
            position: absolute;
            border: 2px solid black;
            cursor: pointer;
        }

        .cursor-image {
            position: absolute;
            width: 50px;
            height: auto;
            pointer-events: none;
            background-color: transparent !important;
            border: none !important;
            user-select: none;
            -webkit-user-drag: none;
            z-index: 9999;
        }

        .bottom-buttons {
            position: fixed;
            bottom: 10px;
            left: 0;
            right: 0;
            display: flex;
            justify-content: center;
            gap: 20px;
            pointer-events: auto;
            z-index: 100;
        }

        .guestbook-box, .link-box, .gif-button {
            background-color: rgba(255, 255, 255, 0.8);
            border: 2px solid black;
            padding: 10px;
            cursor: pointer;
            text-align: center;
            font-size: 18px;
            width: 150px;
            user-select: none;
            box-sizing: border-box;
            font-family: "Comic Sans MS", cursive, sans-serif;
        }

        .guestbook-box:hover { background-color: lightcoral; }
        .link-box { background-color: lightblue; }
        .link-box:hover { background-color: skyblue; }
        .gif-button { background-color: #eb9819; color: black; font-weight: bold; }
        .gif-button:hover { background-color: #d17712; }

        #tic-tac-toe-container {
            position: fixed;
            right: 10px;
            top: 50%;
            transform: translateY(-50%);
            background: rgba(255, 255, 255, 0.9);
            border: 3px solid black;
            padding: 10px;
            z-index: 1000;
            font-family: 'Comic Sans MS', cursive;
        }

        #tic-tac-toe-board {
            display: grid;
            grid-template-columns: repeat(3, 60px);
            grid-template-rows: repeat(3, 60px);
            gap: 5px;
            margin-bottom: 10px;
            justify-content: center;
        }

        .tic-cell {
            width: 60px;
            height: 60px;
            background-color: white;
            border: 2px solid black;
            font-size: 30px;
            text-align: center;
            line-height: 60px;
            cursor: pointer;
            user-select: none;
        }

        #reset-button {
            width: 100%;
            padding: 5px;
            cursor: pointer;
            font-weight: bold;
            background-color: lightgreen;
            border: 2px solid black;
        }

        #coin-flip-container {
            position: fixed;
            left: 10px;
            top: 50%;
            transform: translateY(-50%);
            background: rgba(255, 255, 255, 0.9);
            border: 3px solid black;
            padding: 10px;
            z-index: 1000;
            font-family: 'Comic Sans MS', cursive;
            width: 150px;
            text-align: center;
        }

        #coin-flip-result { font-size: 24px; margin: 10px 0; }
        #flip-button { padding: 5px; cursor: pointer; font-weight: bold; background-color: gold; border: 2px solid black; }

        #trains-button {
            position: fixed;
            right: 12px;
            bottom: 12px;
            z-index: 2500;
            background: #5b2f16;
            color: #fff1cf;
            border: 3px solid black;
            padding: 10px 14px;
            font-family: 'Comic Sans MS', cursive;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 3px 3px 0 black;
        }

        #trains-panel {
            display: none;
            position: fixed;
            right: 12px;
            bottom: 62px;
            width: min(430px, calc(100vw - 24px));
            max-height: 82vh;
            overflow-y: auto;
            z-index: 2400;
            background: rgba(255, 244, 213, 0.96);
            border: 4px solid black;
            padding: 12px;
            font-family: 'Comic Sans MS', cursive;
            color: black;
            box-sizing: border-box;
            cursor: auto;
        }

        #trains-panel h2 { margin: 6px 0; text-align: center; }
        #trains-panel h3 { margin: 0; }

        .train-mission-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 8px;
            margin-bottom: 6px;
        }

        .train-runs-badge {
            background: #5b2f16;
            color: #fff1cf;
            border: 2px solid black;
            padding: 3px 7px;
            font-size: 13px;
            font-weight: bold;
            white-space: nowrap;
        }

        .train-close {
            float: right;
            border: 2px solid black;
            background: lightcoral;
            cursor: pointer;
            font-weight: bold;
        }

        .train-mission {
            border: 2px solid black;
            background: rgba(255,255,255,0.55);
            padding: 8px;
            margin-top: 8px;
        }

        .train-label-row, .train-row {
            display: grid;
            grid-template-columns: 1fr 70px 70px;
            gap: 6px;
            align-items: center;
            margin: 4px 0;
        }

        .train-label-row { font-size: 12px; font-weight: bold; }

        .train-row input {
            width: 100%;
            box-sizing: border-box;
            padding: 4px;
            border: 2px solid black;
            font-family: inherit;
        }

        .train-mission button {
            width: 100%;
            margin-top: 6px;
            padding: 6px;
            border: 2px solid black;
            background: #c28339;
            font-family: inherit;
            font-weight: bold;
            cursor: pointer;
        }

        #train-message {
            margin-top: 8px;
            text-align: center;
            font-weight: bold;
            min-height: 22px;
        }
    </style>
</head>
<body>
    <div class="bouncing-text">Click for good luck!</div>
    <img src="Monkeys/monkeycursor.png" alt="Cursor Image" class="cursor-image" />

    <div class="bottom-buttons">
        <div class="guestbook-box" onclick="openGuestbook()">Guestbook</div>
        <div class="gif-button" onclick="showRandomGif()">Monkey</div>
        <div class="link-box" onclick="openLink()">Cat</div>
    </div>

    <img id="cat-image" src="https://i.imgur.com/IIM6kpY.png" alt="Cat Image" style="display: none; max-width: 100%; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); border: 4px solid black; z-index: 1000;" />

    <div id="gif-container" style="display:none; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); border: 4px solid black; background: white; padding: 10px; z-index: 1000;">
        <button onclick="hideGif()" style="display: block; margin-bottom: 8px; cursor:pointer;">Close GIF</button>
        <img id="random-gif" src="" alt="Random GIF" style="max-width: 90vw; max-height: 80vh; display: block;" />
    </div>

    <div id="tic-tac-toe-container">
        <div id="tic-tac-toe-board"></div>
        <button id="reset-button">Reset</button>
    </div>

    <div id="coin-flip-container">
        <div id="coin-flip-result">Flip me!</div>
        <button id="flip-button">Flip Coin</button>
    </div>

    <button id="trains-button" onclick="toggleTrainsPanel()">Trains</button>

    <div id="trains-panel">
        <button class="train-close" onclick="toggleTrainsPanel()">X</button>
        <h2>Train Missions</h2>
        <div id="train-message"></div>
        <div id="train-missions"></div>
    </div>

    <script>
        const box = document.querySelector('.bouncing-text');
        const cursorImage = document.querySelector('.cursor-image');
        const speed = 2;
        let posX = 0, posY = 0, velX = 0, velY = 0, isBouncing = false;
        const boxWidth = box.offsetWidth;
        const boxHeight = box.offsetHeight;

        function startBouncing() {
            if (!isBouncing) {
                isBouncing = true;
                velX = speed;
                velY = speed;
                update();
            }
        }

        function update() {
            if (!isBouncing) return;
            posX += velX;
            posY += velY;
            if (posX <= 0 || posX + boxWidth >= window.innerWidth) velX = -velX;
            if (posY <= 0 || posY + boxHeight >= window.innerHeight) velY = -velY;
            box.style.left = posX + 'px';
            box.style.top = posY + 'px';
            requestAnimationFrame(update);
        }

        posX = Math.random() * (window.innerWidth - boxWidth);
        posY = Math.random() * (window.innerHeight - boxHeight);
        box.style.left = posX + 'px';
        box.style.top = posY + 'px';

        box.addEventListener('click', () => {
            startBouncing();
            const msg = new SpeechSynthesisUtterance("Good luck, bastard!");
            window.speechSynthesis.speak(msg);
        });

        document.addEventListener('mousemove', (event) => {
            cursorImage.style.left = (event.clientX - cursorImage.width / 2) + 'px';
            cursorImage.style.top = (event.clientY - cursorImage.height / 2) + 'px';
        });

        function openGuestbook() { window.location.href = 'guestbook.html'; }

        function openLink() {
            const catImage = document.getElementById('cat-image');
            if (catImage.style.display === 'none' || catImage.style.display === '') {
                catImage.style.display = 'block';
                catImage.classList.remove('cartoon-animate');
                void catImage.offsetWidth;
                catImage.classList.add('cartoon-animate');
            } else {
                catImage.style.display = 'none';
                catImage.classList.remove('cartoon-animate');
            }
        }

        const gifs = [
            'Monkeys/spinning-monkey.gif',
            'Monkeys/monky-monkey.gif',
            'Monkeys/monkey-spinning-444hobi.gif',
            'Monkeys/monkey-spinning.gif',
            'Monkeys/mongy-monke.gif',
            'Monkeys/fat-fat-monkey.gif',
            'Monkeys/spinpool.gif'
        ];

        function showRandomGif() {
            const container = document.getElementById('gif-container');
            const gifImage = document.getElementById('random-gif');
            gifImage.src = gifs[Math.floor(Math.random() * gifs.length)];
            container.style.display = 'block';
        }

        function hideGif() {
            document.getElementById('gif-container').style.display = 'none';
            document.getElementById('random-gif').src = '';
        }

        const board = document.getElementById('tic-tac-toe-board');
        const resetButton = document.getElementById('reset-button');
        let currentPlayer = 'X';

        function createBoard() {
            board.innerHTML = '';
            for (let i = 0; i < 9; i++) {
                const cell = document.createElement('div');
                cell.classList.add('tic-cell');
                cell.addEventListener('click', () => {
                    if (!cell.textContent) {
                        cell.textContent = currentPlayer;
                        currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
                    }
                });
                board.appendChild(cell);
            }
        }
        resetButton.addEventListener('click', createBoard);
        createBoard();

        document.getElementById('flip-button').addEventListener('click', () => {
            document.getElementById('coin-flip-result').textContent = Math.random() < 0.5 ? 'Heads' : 'Tails';
        });

        const TRAIN_MISSIONS = {
            "Mission 1": { "Sugar Sacks": 25, "Strong Stims": 25, "Fiber": 25, "Wagon Repair": 2, "Rocks": 200 },
            "Mission 2": { "Carcano": 1, "Volcanic": 1, "Winchester": 1, "Navy": 1, "Scholfield": 1, "Semi Auto Shot": 1 },
            "Mission 3": { "Iron Bar": 25, "Copper Bar": 25, "Gold Bar": 25, "Tin Tent": 10, "Beer Boxes": 10 },
            "Mission 4": { "Copper Bar": 25, "Shell Casing": 175, "Fertilizer": 10, "Bison Pelt": 1, "Med Box": 1 },
            "Mission 5": { "Acid": 20, "Lucky Coin": 20, "Flower Coffin": 5, "Blue Paint": 20, "Firewood": 20 }
        };

        const DEFAULT_TRAIN_INVENTORY = {};
        Object.values(TRAIN_MISSIONS).forEach(mission => {
            Object.keys(mission).forEach(item => {
                if (DEFAULT_TRAIN_INVENTORY[item] === undefined) DEFAULT_TRAIN_INVENTORY[item] = 0;
            });
        });

        let trainInventory = loadJson('trainInventory', DEFAULT_TRAIN_INVENTORY);
        let trainMissionCosts = loadJson('trainMissionCosts', TRAIN_MISSIONS);

        function loadJson(key, fallback) {
            try {
                const saved = localStorage.getItem(key);
                return saved ? JSON.parse(saved) : JSON.parse(JSON.stringify(fallback));
            } catch (err) {
                return JSON.parse(JSON.stringify(fallback));
            }
        }

        function saveTrainData() {
            localStorage.setItem('trainInventory', JSON.stringify(trainInventory));
            localStorage.setItem('trainMissionCosts', JSON.stringify(trainMissionCosts));
        }

        function toggleTrainsPanel() {
            const panel = document.getElementById('trains-panel');
            panel.style.display = panel.style.display === 'block' ? 'none' : 'block';
            renderTrainTracker();
        }

        function renderTrainTracker() {
            const missionsBox = document.getElementById('train-missions');
            missionsBox.innerHTML = '';

            Object.keys(TRAIN_MISSIONS).forEach(missionName => {
                if (!trainMissionCosts[missionName]) trainMissionCosts[missionName] = JSON.parse(JSON.stringify(TRAIN_MISSIONS[missionName]));

                const missionBox = document.createElement('div');
                missionBox.className = 'train-mission';

                const header = document.createElement('div');
                header.className = 'train-mission-header';

                const title = document.createElement('h3');
                const missionLabels = {
                    'Mission 3': 'Mission 3 - FN to DR',
                    'Mission 4': 'Mission 4 - ER to SD',
                    'Mission 5': 'Mission 5 - Rigg to Wall'
                };

                title.textContent = missionLabels[missionName] || missionName;

                const runsBadge = document.createElement('div');
                runsBadge.className = 'train-runs-badge';
                runsBadge.textContent = 'Runs: ' + calculatePossibleRuns(missionName);

                header.appendChild(title);
                header.appendChild(runsBadge);
                missionBox.appendChild(header);

                const labelRow = document.createElement('div');
                labelRow.className = 'train-label-row';
                labelRow.innerHTML = '<div>Item</div><div>Have</div><div>Uses</div>';
                missionBox.appendChild(labelRow);

                Object.keys(TRAIN_MISSIONS[missionName]).forEach(item => {
                    if (trainInventory[item] === undefined) trainInventory[item] = 0;
                    if (trainMissionCosts[missionName][item] === undefined) trainMissionCosts[missionName][item] = TRAIN_MISSIONS[missionName][item];

                    const row = document.createElement('div');
                    row.className = 'train-row';

                    const label = document.createElement('div');
                    label.textContent = item;

                    const haveInput = document.createElement('input');
                    haveInput.type = 'number';
                    haveInput.value = trainInventory[item];
                    haveInput.dataset.item = item;
                    haveInput.className = 'train-have-input';
                    haveInput.addEventListener('input', syncTrainHaveInputs);

                    const usesInput = document.createElement('input');
                    usesInput.type = 'number';
                    usesInput.value = trainMissionCosts[missionName][item];
                    usesInput.dataset.mission = missionName;
                    usesInput.dataset.item = item;
                    usesInput.className = 'train-uses-input';

                    row.appendChild(label);
                    row.appendChild(haveInput);
                    row.appendChild(usesInput);
                    missionBox.appendChild(row);
                });

                const saveButton = document.createElement('button');
                saveButton.textContent = 'Save ' + missionName + ' Values';
                saveButton.onclick = saveTrainValues;
                missionBox.appendChild(saveButton);

                const runButton = document.createElement('button');
                runButton.textContent = 'Complete ' + missionName + ' Run';
                runButton.onclick = () => completeTrainMission(missionName);
                missionBox.appendChild(runButton);

                missionsBox.appendChild(missionBox);
            });
        }

        function saveTrainValues() {
            const newestValues = {};

            document.querySelectorAll('.train-have-input').forEach(input => {
                newestValues[input.dataset.item] = Number(input.value) || 0;
            });

            Object.keys(newestValues).forEach(item => {
                trainInventory[item] = newestValues[item];
            });

            document.querySelectorAll('.train-uses-input').forEach(input => {
                if (!trainMissionCosts[input.dataset.mission]) trainMissionCosts[input.dataset.mission] = {};
                trainMissionCosts[input.dataset.mission][input.dataset.item] = Number(input.value) || 0;
            });

            saveTrainData();
            renderTrainTracker();
            showTrainMessage('Values saved.');
        }

        function syncTrainHaveInputs(event) {
            const changedInput = event.target;
            const item = changedInput.dataset.item;
            const value = changedInput.value;

            document.querySelectorAll('.train-have-input').forEach(input => {
                if (input.dataset.item === item && input !== changedInput) {
                    input.value = value;
                }
            });
        }

        function calculatePossibleRuns(missionName) {
            const mission = trainMissionCosts[missionName] || TRAIN_MISSIONS[missionName];
            let possibleRuns = Infinity;

            Object.keys(mission).forEach(item => {
                const have = Number(trainInventory[item]) || 0;
                const uses = Number(mission[item]) || 0;

                if (uses > 0) {
                    possibleRuns = Math.min(possibleRuns, Math.floor(have / uses));
                }
            });

            if (possibleRuns === Infinity) return 0;
            return possibleRuns;
        }

        function completeTrainMission(missionName) {
            saveTrainValues();

            Object.keys(trainMissionCosts[missionName]).forEach(item => {
                const current = Number(trainInventory[item]) || 0;
                const used = Number(trainMissionCosts[missionName][item]) || 0;
                trainInventory[item] = Math.max(0, current - used);
            });

            saveTrainData();
            renderTrainTracker();
            showTrainMessage(missionName + ' run completed.');
        }

        function showTrainMessage(message) {
            document.getElementById('train-message').textContent = message;
        }

        renderTrainTracker();
    </script>
</body>
</html>
