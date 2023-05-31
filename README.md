<!DOCTYPE html>
<html>
<head>
    <title>Game</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        #login {
            background-color: #f0f0f0;
            padding: 20px;
            border-radius: 5px;
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
        }

        #nameInput {
            margin-bottom: 10px;
        }

        #notification {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #f0f0f0;
            padding: 10px;
            border-radius: 5px;
            display: none;
        }

        #gameCanvas {
            width: 100%;
            height: 100%;
            background-color: #000;
            display: none;
        }
    </style>
</head>
<body>
    <div id="login">
        <h2>Login</h2>
        <input type="text" id="nameInput" placeholder="Your Name">
        <button onclick="login()">Login</button>
    </div>

    <canvas id="gameCanvas"></canvas>

    <div id="notification"></div>

    <script>
        const login = document.getElementById('login');
        const nameInput = document.getElementById('nameInput');
        const gameCanvas = document.getElementById('gameCanvas');
        const notification = document.getElementById('notification');

        let player = {
            name: ''
        };

        let gameStarted = false;

        function login() {
            const name = nameInput.value.trim();

            if (name === '') {
                showNotification('Please enter your name.');
                return;
            }

            player.name = name;

            login.style.display = 'none';
            gameCanvas.style.display = 'block';
            gameStarted = true;

            // Start game logic
            startGame();
        }

        function showNotification(message) {
            notification.innerText = message;
            notification.style.display = 'block';
            setTimeout(() => {
                notification.style.display = 'none';
            }, 2000);
        }

        function startGame() {
            const canvas = gameCanvas;
            const context = canvas.getContext('2d');
            const playerSize = 40;
            let playerX = canvas.width / 2 - playerSize / 2;
            let playerY = canvas.height / 2 - playerSize / 2;
            let playerSpeed = 5;

            function drawPlayer() {
                context.clearRect(0, 0, canvas.width, canvas.height);
                context.fillStyle = 'red';
                context.fillRect(playerX, playerY, playerSize, playerSize);
                context.font = '20px Arial';
                context.fillStyle = 'white';
                context.fillText(player.name, playerX, playerY - 10);
            }

            function updatePlayerPosition() {
                document.addEventListener('keydown', (event) => {
                    const key = event.key.toLowerCase();
                    if (key === 'w') {
                        playerY -= playerSpeed;
                    } else if (key === 'a') {
                        playerX -= playerSpeed;
                    } else if (key === 's') {
                        playerY += playerSpeed;
                    } else if (key === 'd') {
                        playerX += playerSpeed;
                    }
                });
            }

            function gameLoop() {
                drawPlayer();
                updatePlayerPosition();
                requestAnimationFrame(gameLoop);
            }

            gameLoop();
        }
    </script>
</body>
</html>
