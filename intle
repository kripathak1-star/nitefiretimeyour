<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Mini Fire Game</title>
  <style>
    body {
      margin: 0;
      background: black;
      color: white;
      text-align: center;
      font-family: Arial;
    }
    canvas {
      background: #222;
      display: block;
      margin: 10px auto;
      border: 2px solid white;
    }
  </style>
</head>
<body>

<h2>🔥 Mini Fire 🔫</h2>
<p>Move: ⬅️ ➡️ | Shoot: SPACE</p>

<canvas id="game" width="360" height="500"></canvas>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

// player
let player = { x: 160, y: 440, w: 40, h: 40 };

// game objects
let bullets = [];
let enemies = [];
let score = 0;
let gameOver = false;

// controls
document.addEventListener("keydown", e => {
  if (e.key === "ArrowLeft" && player.x > 0) {
    player.x -= 20;
  }
  if (e.key === "ArrowRight" && player.x < canvas.width - player.w) {
    player.x += 20;
  }
  if (e.code === "Space" && !gameOver) {
    bullets.push({ x: player.x + 18, y: player.y });
  }
});

// enemies
setInterval(() => {
  if (!gameOver) {
    enemies.push({
      x: Math.random() * (canvas.width - 40),
      y: -40,
      w: 40,
      h: 40
    });
  }
}, 1000);

// game loop
function gameLoop() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  if (gameOver) {
    ctx.fillStyle = "white";
    ctx.font = "30px Arial";
    ctx.fillText("GAME OVER", 90, 220);
    ctx.font = "20px Arial";
    ctx.fillText("Score: " + score, 130, 260);
    return;
  }

  // draw player
  ctx.fillStyle = "#00ff99";
  ctx.fillRect(player.x, player.y, player.w, player.h);

  // bullets
  ctx.fillStyle = "yellow";
  bullets.forEach((b, i) => {
    b.y -= 8;
    ctx.fillRect(b.x, b.y, 4, 10);
    if (b.y < 0) bullets.splice(i, 1);
  });

  // enemies
  ctx.fillStyle = "red";
  enemies.forEach((e, ei) => {
    e.y += 3;
    ctx.fillRect(e.x, e.y, e.w, e.h);

    // collision with player
    if (
      player.x < e.x + e.w &&
      player.x + player.w > e.x &&
      player.y < e.y + e.h &&
      player.y + player.h > e.y
    ) {
      gameOver = true;
    }

    // collision with bullet
    bullets.forEach((b, bi) => {
      if (
        b.x < e.x + e.w &&
        b.x + 4 > e.x &&
        b.y < e.y + e.h &&
        b.y + 10 > e.y
      ) {
        enemies.splice(ei, 1);
        bullets.splice(bi, 1);
        score += 10;
      }
    });
  });

  // score
  ctx.fillStyle = "white";
  ctx.font = "16px Arial";
  ctx.fillText("Score: " + score, 10, 20);

  requestAnimationFrame(gameLoop);
}

gameLoop();
</script>

</body>
</html>
