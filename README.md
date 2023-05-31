<script>
    // ...

    function login() {
        const name = nameInput.value.trim();

        if (name === '' || player.emoji === '') {
            showNotification('Please enter your name and select an emoji.');
            return;
        }

        player.name = name;

        login.style.display = 'none';
        gameCanvas.style.display = 'block';
        gameStarted = true;

        // Start game logic
        startGame();
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
