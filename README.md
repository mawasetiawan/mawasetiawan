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

        canvas {
            border: 1px solid #000;
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

        #login {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: #f0f0f0;
            padding: 20px;
            border-radius: 5px;
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
        }

        #profile-selection {
            display: flex;
            margin-bottom: 20px;
        }

        .profile-image {
            width: 60px;
            height: 60px;
            border: 2px solid #000;
            margin-right: 10px;
            cursor: pointer;
        }

        .profile-image.active {
            border-color: #ff0000;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas"></canvas>

    <div id="notification"></div>

    <div id="login">
        <h2>Welcome to the Game</h2>
        <input type="text" id="nameInput" placeholder="Your Name">
        <div id="profile-selection">
            <div class="profile-image" onclick="selectProfile(0)"></div>
            <div class="profile-image" onclick="selectProfile(1)"></div>
            <div class="profile-image" onclick="selectProfile(2)"></div>
            <div class="profile-image" onclick="selectProfile(3)"></div>
            <div class="profile-image" onclick="selectProfile(4)"></div>
        </div>
        <button onclick="startGame()">Start</button>
    </div>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const context = canvas.getContext('2d');

        const notification = document.getElementById('notification');
        const login = document.getElementById('login');
        const nameInput = document.getElementById('nameInput');
        const profileImages = document.querySelectorAll('.profile-image');

        let player = {
            name: '',
            profileImage: ''
        };

        let gameStarted = false;

        function selectProfile(index) {
            profileImages.forEach((image, i) => {
                if (i === index) {
                    image.classList.add('active');
                } else {
                    image.classList.remove('active');
                }
            });

            player.profileImage = `https://this-person-does-not-exist.com/image${index}.jpg`;
        }

        function startGame() {
            const name = nameInput.value.trim();
            if (name === '' || player.profileImage === '') {
                return;
            }

            player.name = name;
            login.style.display = 'none';
            canvas.style.display = 'block';
            gameStarted = true;

            drawPlayer();
            startAnimation();
        }

        function showNotification(message) {
            notification.innerText = message;
            notification.style.display = 'block';
            setTimeout(() => {
                notification.style.display = 'none';
            }, 2000);
        }

        function drawPlayer() {
            const img = new Image();
            img.src = player.profileImage;

            img.onload = () => {
                context.clearRect(0, 0, canvas.width, canvas.height);
                context.drawImage(img, canvas.width / 2 - 30, canvas.height / 2 - 30, 60, 60);
                context.fillStyle = '#000';
                context.font = '12px Arial';
                context.fillText(player.name, canvas.width / 2 - (player.name.length * 2.5), canvas.height / 2 + 40);
            };
        }

        function startAnimation() {
            if (!gameStarted) {
                return;
            }

            context.clearRect(0, 0, canvas.width, canvas.height);
            context.fillStyle = '#ff0000';
            context.fillRect(Math.random() * (canvas.width - 20), Math.random() * (canvas.height - 20), 20, 20);

            requestAnimationFrame(startAnimation);
        }

        function handleKeyPress(event) {
            if (!gameStarted) {
                return;
            }

            const key = event.key;

            // Handle movement logic based on key press
            // ...

            // Check collision with zombie and perform action
            // ...

            // Check if player is dead and show notification
            // ...
        }

        document.addEventListener('keypress', handleKeyPress);
    </script>
</body>
</html>
