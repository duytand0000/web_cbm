<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Catch The Ball</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    background: #111827;
    color: white;
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
}

.game {
    width: 420px;
    max-width: 95vw;
    text-align: center;
}

h1 {
    margin-bottom: 10px;
}

.info {
    display: flex;
    justify-content: space-between;
    background: #1f2937;
    padding: 12px 18px;
    border-radius: 10px 10px 0 0;
}

canvas {
    width: 100%;
    background: #0f172a;
    border: 2px solid #374151;
    display: block;
}

button {
    margin-top: 15px;
    padding: 12px 25px;
    border: none;
    border-radius: 8px;
    background: #6366f1;
    color: white;
    font-size: 16px;
    cursor: pointer;
}

button:hover {
    background: #4f46e5;
}

#message {
    margin-top: 10px;
    color: #9ca3af;
}
</style>
</head>

<body>

<div class="game">

    <h1>🎯 Catch The Ball</h1>

    <div class="info">
        <span>Điểm: <b id="score">0</b></span>
        <span>Mạng: <b id="lives">3</b></span>
    </div>

    <canvas id="gameCanvas" width="400" height="500"></canvas>

    <button onclick="restartGame()">Chơi lại</button>

    <p id="message">
        Dùng ← → hoặc A/D để di chuyển
    </p>

</div>

<script>

const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

const scoreText = document.getElementById("score");
const livesText = document.getElementById("lives");
const message = document.getElementById("message");

let score = 0;
let lives = 3;
let gameOver = false;

const player = {
    x: 170,
    y: 450,
    width: 60,
    height: 20,
    speed: 7
};

const ball = {
    x: Math.random() * 380,
    y: 0,
    radius: 10,
    speed: 3
};

let keys = {};

document.addEventListener("keydown", function(e) {

    keys[e.key.toLowerCase()] = true;

    if (e.code === "Space" && gameOver) {
        restartGame();
    }

});

document.addEventListener("keyup", function(e) {
    keys[e.key.toLowerCase()] = false;
});


function drawPlayer() {

    ctx.fillStyle = "#6366f1";

    ctx.fillRect(
        player.x,
        player.y,
        player.width,
        player.height
    );
}


function drawBall() {

    ctx.beginPath();

    ctx.arc(
        ball.x,
        ball.y,
        ball.radius,
        0,
        Math.PI * 2
    );

    ctx.fillStyle = "#f43f5e";

    ctx.fill();

    ctx.closePath();
}


function movePlayer() {

    if (keys["arrowleft"] || keys["a"]) {
        player.x -= player.speed;
    }

    if (keys["arrowright"] || keys["d"]) {
        player.x += player.speed;
    }

    if (player.x < 0) {
        player.x = 0;
    }

    if (player.x + player.width > canvas.width) {
        player.x = canvas.width - player.width;
    }
}function moveBall() {

    ball.y += ball.speed;

    // Kiểm tra bắt bóng

    if (
        ball.y + ball.radius >= player.y &&
        ball.x >= player.x &&
        ball.x <= player.x + player.width
    ) {

        score += 10;

        scoreText.textContent = score;

        resetBall();

        // Tăng tốc độ

        ball.speed += 0.2;
    }


    // Bóng rơi khỏi màn hình

    if (ball.y > canvas.height) {

        lives--;

        livesText.textContent = lives;

        resetBall();

        if (lives <= 0) {
            endGame();
        }
    }
}


function resetBall() {

    ball.x =
        ball.radius +
        Math.random() *
        (canvas.width - ball.radius * 2);

    ball.y = 0;
}


function endGame() {

    gameOver = true;

    message.innerHTML =
        "💀 GAME OVER — Nhấn SPACE để chơi lại";

}


function restartGame() {

    score = 0;

    lives = 3;

    gameOver = false;

    ball.speed = 3;

    player.x = 170;

    scoreText.textContent = score;

    livesText.textContent = lives;

    message.textContent =
        "Dùng ← → hoặc A/D để di chuyển";

    resetBall();

    gameLoop();
}


function gameLoop() {

    if (gameOver) {
        return;
    }

    ctx.clearRect(
        0,
        0,
        canvas.width,
        canvas.height
    );

    movePlayer();

    moveBall();

    drawPlayer();

    drawBall();

    requestAnimationFrame(gameLoop);
}


// Bắt đầu game

gameLoop();

</script>

</body>
</html>
