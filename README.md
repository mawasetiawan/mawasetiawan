<!DOCTYPE html>
<html>
<head>
    <title>Game</title>
    <style>
        body {
            margin: 0;
            padding: 0;
        }

        #gameCanvas {
            width: 100vw;
            height: 100vh;
            background-color: #000;
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
    </style>
</head>
<body>
    <canvas id="gameCanvas"></canvas>

    <div id="notification"></div>

    <script>
        const gameCanvas = document.getElementById('gameCanvas');
        const notification = document.getElementById('notification');

        let player = {
            name: 'Player',
            x: 0,
            y: 0,
            speed: 5
        };

        let gameStarted = false;

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

            function drawPlayer() {
                context.clearRect(0, 0, canvas.width, canvas.height);
                context.fillStyle = 'red';
                context.fillRect(player.x, player.y, playerSize, playerSize);
                context.font = '20px Arial';
                context.fillStyle = 'white';
                context.fillText(player.name, player.x, player.y - 10);
            }

            function updatePlayerPosition() {
                document.addEventListener('keydown', (event) => {
                    const key = event.key.toLowerCase();
                    if (key === 'w') {
                        player.y -= player.speed;
                    } else if (key === 'a') {
                        player.x -= player.speed;
                    } else if (key === 's') {
                        player.y += player.speed;
                    } else if (key === 'd') {
                        player.x += player.speed;
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

        // Prompt user for their name
        player.name = prompt('Please enter your name') || 'Player';

        // Start the game
        gameCanvas.style.display = 'block';
        gameStarted = true;
        startGame();
    </script>
</body>
</html>
