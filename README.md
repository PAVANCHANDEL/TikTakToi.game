# TikTakToi.game
TikTakToi
<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TIK TAK TOE</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
        }

        .container {
            text-align: center;
        }

        #game-title {
            font-size: 3rem;
            margin-bottom: 20px;
        }

        #board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 5px;
            margin-bottom: 20px;
        }

        .cell {
            width: 100px;
            height: 100px;
            background-color: #fff;
            border: 2px solid #000;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2rem;
            cursor: pointer;
        }

        #result {
            font-size: 2rem;
            margin-bottom: 20px;
        }

        #restart {
            padding: 10px 20px;
            font-size: 1rem;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1 id="game-title">TIK TAK TOE</h1>
        <div id="board">
            <div class="cell" data-index="0"></div>
            <div class="cell" data-index="1"></div>
            <div class="cell" data-index="2"></div>
            <div class="cell" data-index="3"></div>
            <div class="cell" data-index="4"></div>
            <div class="cell" data-index="5"></div>
            <div class="cell" data-index="6"></div>
            <div class="cell" data-index="7"></div>
            <div class="cell" data-index="8"></div>
        </div>
        <div id="result"></div>
        <button id="restart">रीस्टार्ट</button>
    </div>
    <script>
        const board = document.getElementById('board');
        const cells = document.querySelectorAll('.cell');
        const resultDisplay = document.getElementById('result');
        const restartButton = document.getElementById('restart');
        let currentPlayer = 'X';
        let gameActive = true;
        let gameState = ['', '', '', '', '', '', '', '', ''];

        const winningConditions = [
            [0, 1, 2],
            [3, 4, 5],
            [6, 7, 8],
            [0, 3, 6],
            [1, 4, 7],
            [2, 5, 8],
            [0, 4, 8],
            [2, 4, 6]
        ];

        function handleCellClick(event) {
            const clickedCell = event.target;
            const clickedCellIndex = parseInt(clickedCell.getAttribute('data-index'));

            if (gameState[clickedCellIndex] !== '' || !gameActive) {
                return;
            }

            gameState[clickedCellIndex] = currentPlayer;
            clickedCell.textContent = currentPlayer;

            checkForWinner();
        }

        function checkForWinner() {
            let roundWon = false;

            for (let i = 0; i < winningConditions.length; i++) {
                const winCondition = winningConditions[i];
                const a = gameState[winCondition[0]];
                const b = gameState[winCondition[1]];
                const c = gameState[winCondition[2]];

                if (a === '' || b === '' || c === '') {
                    continue;
                }

                if (a === b && b === c) {
                    roundWon = true;
                    break;
                }
            }

            if (roundWon) {
                resultDisplay.innerHTML = `<span style="color: green;">${currentPlayer}</span> <span style="color: red;">WIN</span>`;
                gameActive = false;
                return;
            }

            const roundDraw = !gameState.includes('');
            if (roundDraw) {
                resultDisplay.textContent = 'DRAW!';
                gameActive = false;
                return;
            }

            currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
        }

        function restartGame() {
            gameState = ['', '', '', '', '', '', '', '', ''];
            gameActive = true;
            currentPlayer = 'X';
            resultDisplay.textContent = '';
            cells.forEach(cell => cell.textContent = '');
        }

        cells.forEach(cell => cell.addEventListener('click', handleCellClick));
        restartButton.addEventListener('click', restartGame);
    </script>
</body>
</html>
