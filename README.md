<!DOCTYPE html>
<html>
<head>
    <title>Ping Pong Game</title>
    <style>
        canvas {
            background-color: #000;
            display: block;
            margin: 0 auto;
        }
    </style>
</head>
<body>
    <canvas id="pong" width="800" height="400"></canvas>

    <script>
        const canvas = document.getElementById('pong');
        const context = canvas.getContext('2d');

        // Objek raket
        const paddleWidth = 10;
        const paddleHeight = 100;

        const player = {
            x: 0,
            y: canvas.height / 2 - paddleHeight / 2,
            width: paddleWidth,
            height: paddleHeight,
            color: '#fff',
            dy: 8 // Kecepatan pergerakan raket
        };

        const computer = {
            x: canvas.width - paddleWidth,
            y: canvas.height / 2 - paddleHeight / 2,
            width: paddleWidth,
            height: paddleHeight,
            color: '#fff',
            dy: 4 // Kecepatan pergerakan raket komputer
        };

        // Objek bola
        const ball = {
            x: canvas.width / 2,
            y: canvas.height / 2,
            radius: 10,
            speed: 4,
            dx: 4,
            dy: 4,
            color: '#fff'
        };

        // Fungsi menggambar objek pada canvas
        function drawRect(x, y, width, height, color) {
            context.fillStyle = color;
            context.fillRect(x, y, width, height);
        }

        function drawCircle(x, y, radius, color) {
            context.fillStyle = color;
            context.beginPath();
            context.arc(x, y, radius, 0, Math.PI * 2, false);
            context.closePath();
            context.fill();
        }

        function drawText(text, x, y, color) {
            context.fillStyle = color;
            context.font = '45px fantasy';
            context.fillText(text, x, y);
        }

        // Fungsi menggambar net pada tengah lapangan
        function drawNet() {
            for (let i = 0; i <= canvas.height; i += 15) {
                drawRect(canvas.width / 2 - 1, i, 2, 10, '#fff');
            }
        }

        // Fungsi menggambar seluruh objek pada canvas
        function draw() {
            drawRect(0, 0, canvas.width, canvas.height, '#000');

            drawText(player.score, canvas.width / 4, canvas.height / 5, '#fff');
            drawText(computer.score, 3 * canvas.width / 4, canvas.height / 5, '#fff');

            drawNet();

            drawRect(player.x, player.y, player.width, player.height, player.color);
            drawRect(computer.x, computer.y, computer.width, computer.height, computer.color);

            drawCircle(ball.x, ball.y, ball.radius, ball.color);
        }

        // Fungsi menggerakkan raket pemain
        function movePaddle() {
            document.addEventListener('keydown', function(event) {
                switch(event.keyCode) {
                    case 38: // Tombol panah atas
                        if (player.y - player.dy > 0) {
                            player.y -= player.dy;
                        }
                        break;
                    case 40: // Tombol panah bawah
                        if (player.y + player.dy + player.height < canvas.height) {
                            player.y += player.dy;
                        }
                        break;
                }
            });
        }

        // Fungsi menggerakkan raket komputer secara otomatis
        function moveComputer() {
            if (computer.y < ball.y && computer.y + computer.height < canvas.height) {
                computer.y += computer.dy;
            }

            if (computer.y > ball.y && computer.y > 0) {
                computer.y -= computer.dy;
            }
        }

        // Fungsi menggerakkan bola
        function moveBall() {
            ball.x += ball.dx;
            ball.y += ball.dy;

            // Deteksi tabrakan dengan dinding atas/bawah
            if (ball.y + ball.radius > canvas.height || ball.y - ball.radius < 0) {
                ball.dy *= -1;
            }

            // Deteksi tabrakan dengan raket pemain
            if (
                ball.x - ball.radius < player.x + player.width &&
                ball.y > player.y &&
                ball.y < player.y + player.height
            ) {
                ball.dx *= -1;
            }

            // Deteksi tabrakan dengan raket komputer
            if (
                ball.x + ball.radius > computer.x &&
                ball.y > computer.y &&
                ball.y < computer.y + computer.height
            ) {
                ball.dx *= -1;
            }

            // Deteksi bola keluar dari layar
            if (ball.x - ball.radius < 0) {
                computer.score++;
                resetBall();
            } else if (ball.x + ball.radius > canvas.width) {
                player.score++;
                resetBall();
            }
        }

        // Fungsi mengatur posisi awal bola
        function resetBall() {
            ball.x = canvas.width / 2;
            ball.y = canvas.height / 2;
            ball.dx = -ball.dx;
        }

        // Fungsi utama permainan
        function game() {
            draw();
            movePaddle();
            moveComputer();
            moveBall();
        }

        // Loop permainan
        setInterval(game, 1000 / 60); // 60 FPS
    </script>
</body>
</html>
