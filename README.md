# Li-es-Java-code-
// Variables
var score = 0;
var lives = 3;
var gameOver = false;
var won = false;
var timer = 0;
var speed = 2;

// Create Sprites
var platform = createSprite(100, 50);
platform.setAnimation("platform");
platform.velocityY = speed;

var platform2 = createSprite(300, 250);
platform2.setAnimation("platform");
platform2.velocityY = speed;

var item = createSprite(randomNumber(50, 350), randomNumber(-30, -60));
item.setAnimation("star");
item.velocityY = speed;
item.scale=0.1

var item2 = createSprite(randomNumber(50, 350), randomNumber(-30, -60));
item2.setAnimation("star");
item2.velocityY = speed;
item2.scale=0.1

var player = createSprite(200, 0);
player.setAnimation("alien");

// ==================== DRAW LOOP ====================
function draw() {

  // Tela de vitória
  if (won) {
    winScreen();
    return;
  }

  // Tela de game over
  if (gameOver) {
    gameOverScreen();
    return;
  }

  // Timer (60 frames = 1 segundo)
  timer = timer + 1;

  // Dificuldade progressiva a cada 10 pontos
  speed = 2 + (score / 10) * 0.5;
  platform.velocityY = speed;
  platform2.velocityY = speed;
  item.velocityY = speed;
  item2.velocityY = speed;

  // Background muda após 20 pontos
  if (score <= 20) {
    background1();
  } else {
    background2();
  }

  showScore();
  loopPlatforms();
  loopItems();
  playerFall();
  controlPlayer();
  playerLands();
  collectItems();
  checkFall();
  drawSprites();
}

// ==================== BACKGROUNDS ====================
function background1() {
  background("darkBlue");
  noStroke();
  fill("yellow");
  ellipse(randomNumber(0, 400), randomNumber(0, 400), 3, 3);
  ellipse(randomNumber(0, 400), randomNumber(0, 400), 3, 3);
  ellipse(340, 50, 60, 60);
  fill("darkBlue");
  ellipse(320, 30, 60, 60);
}

function background2() {
  background("pink");
  noStroke();
  fill("yellow");
  ellipse(randomNumber(0, 400), randomNumber(0, 400), 3, 3);
  ellipse(randomNumber(0, 400), randomNumber(0, 400), 3, 3);
  ellipse(340, 50, 60, 60);
}

// ==================== HUD ====================
function showScore() {
  fill("white");
  textSize(20);
  text("Score: ", 10, 10, 80, 20);
  text(score, 80, 29);
  text("Vidas: " + lives, 10, 50);
  text("Tempo: " + floor(timer / 60) + "s", 10, 70);
}

// ==================== TELAS ====================
function winScreen() {
  background("black");
  fill("gold");
  textSize(30);
  text("VOCE GANHOU!", 70, 150);
  fill("white");
  textSize(20);
  text("Score: 100", 130, 210);
  text("Tempo: " + floor(timer / 60) + "s", 130, 240);
}

function gameOverScreen() {
  background("black");
  fill("red");
  textSize(30);
  text("GAME OVER", 100, 180);
  fill("white");
  textSize(20);
  text("Score: " + score, 150, 230);
}

// ==================== PLATAFORMAS ====================
function loopPlatforms() {
  if (platform.y > 450) {
    platform.y = -10;
    platform.x = randomNumber(50, 350);
  }
  if (platform2.y > 450) {
    platform2.y = -10;
    platform2.x = randomNumber(50, 350);
  }
}

// ==================== ITENS ====================
function loopItems() {
  if (item.y > 410) {
    item.y = randomNumber(-10, -50);
    item.x = randomNumber(50, 350);
  }
  if (item2.y > 410) {
    item2.y = randomNumber(-10, -50);
    item2.x = randomNumber(50, 350);
  }
}

function collectItems() {
  if (player.isTouching(item)) {
    score = score + 1;
    item.y = randomNumber(-10, -50);
    item.x = randomNumber(50, 350);
    if (score >= 100) {
      won = true;
    }
  }
  if (player.isTouching(item2)) {
    score = score + 1;
    item2.y = randomNumber(-10, -50);
    item2.x = randomNumber(50, 350);
    if (score >= 100) {
      won = true;
    }
  }
}

// ==================== PLAYER ====================
function playerFall() {
  player.velocityY = player.velocityY + 0.1;
}

function controlPlayer() {
  if (keyDown("up")) {
    player.velocityY = -3;
  }
  if (keyDown("left")) {
    player.x = player.x - 3;
  }
  if (keyDown("right")) {
    player.x = player.x + 3;
  }
}

function playerLands() {
  player.collide(platform);
  player.collide(platform2);
}

function checkFall() {
  if (player.y > 450) {
    lives = lives - 1;
    player.x = 200;
    player.y = 0;
    player.velocityY = 0;
    if (lives <= 0) {
      gameOver = true;
    }
  }
}
