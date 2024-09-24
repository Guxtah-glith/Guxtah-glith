```javascript
let canvas = document.getElementById("canvas");
let ctx = canvas.getContext("2d");

let player1 = {
  x: 50,
  y: canvas.height / 2 - 50,
  width: 10,
  height: 100,
  color: "blue",
  score: 0,
  speed: 5
};

let player2 = {
  x: canvas.width - 60,
  y: canvas.height / 2 - 50,
  width: 10,
  height: 100,
  color: "red",
  score: 0,
  speed: 5
};

let ball = {
  x: canvas.width / 2,
  y: canvas.height / 2,
  radius: 10,
  speedX: 5,
  speedY: 5,
  color: "green"
};

function drawRect(x, y, width, height, color) {
  ctx.fillStyle = color;
  ctx.fillRect(x, y, width, height);
}

function drawCircle(x, y, radius, color) {
  ctx.fillStyle = color;
  ctx.beginPath();
  ctx.arc(x, y, radius, 0, Math.PI * 2, true);
  ctx.closePath();
  ctx.fill();
}

function drawText(text, x, y, color) {
  ctx.fillStyle = color;
  ctx.font = "30px Arial";
  ctx.fillText(text, x, y);
}

function drawScore() {
  drawText(player1.score, canvas.width / 4, 50, "blue");
  drawText(player2.score, (3 * canvas.width) / 4, 50, "red");
}

function update() {
  ball.x += ball.speedX;
  ball.y += ball.speedY;

  // Colisão com as bordas superiores e inferiores
  if (ball.y - ball.radius < 0 || ball.y + ball.radius > canvas.height) {
    ball.speedY = -ball.speedY;
  }

  // Colisão com os jogadores
  if (
    ball.x - ball.radius < player1.x + player1.width &&
    ball.y > player1.y &&
    ball.y < player1.y + player1.height
  ) {
    ball.speedX = -ball.speedX;
  }

  if (
    ball.x + ball.radius > player2.x &&
    ball.y > player2.y &&
    ball.y < player2.y + player2.height
  ) {
    ball.speedX = -ball.speedX;
  }

  // Verificar se a bola saiu da tela
  if (ball.x - ball.radius < 0) {
    player2.score++;
    resetBall();
  }

  if (ball.x + ball.radius > canvas.width) {
    player1.score++;
    resetBall();
  }
}

function resetBall() {
  ball.x = canvas.width / 2;
  ball.y = canvas.height / 2;
  ball.speedX = -ball.speedX;
  ball.speedY = Math.floor(Math.random() * 10) - 5;
}

function draw() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  drawRect(player1.x, player1.y, player1.width, player1.height, player1.color);
  drawRect(player2.x, player2.y, player2.width, player2.height, player2.color);
  drawCircle(ball.x, ball.y, ball.radius, ball.color);
  drawScore();
}

function gameLoop() {
  update();
  draw();
  requestAnimationFrame(gameLoop);
}

gameLoop();
```
