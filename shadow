// Base canvas setup and variables
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

let player = {
  x: 50,
  y: canvas.height - 100,
  width: 30,
  height: 30,
  velocityY: 0,
  jumping: false,
  speed: 3
};

let gravity = 0.5;
let platforms = [];
let powerUps = [];
let obstacles = [];
let score = 0;
let starsCollected = 0;
let comboCount = 0;
let level = 1;
const achievements = [
  { name: 'First Jump', condition: () => score >= 10 },
  { name: 'Star Collector', condition: () => starsCollected >= 50 }
];

const jumpSound = new Audio('jump.mp3');
const collectSound = new Audio('collect.mp3');

function playJumpSound() {
  jumpSound.play();
}

function playCollectSound() {
  collectSound.play();
}

function generatePlatform() {
  const platformWidth = Math.random() * (150 - 50) + 50;
  const platformX = canvas.width;
  const platformY = Math.random() * (canvas.height - 200) + 100;
  platforms.push({ x: platformX, y: platformY, width: platformWidth, height: 10 });
}

function generatePowerUp() {
  const powerUpTypes = ['speed', 'shield', 'doublePoints'];
  const type = powerUpTypes[Math.floor(Math.random() * powerUpTypes.length)];
  powerUps.push({ x: canvas.width, y: Math.random() * canvas.height, type });
}

function generateObstacle() {
  const obstacleY = Math.random() * (canvas.height - 50);
  obstacles.push({ x: canvas.width, y: obstacleY, width: 20, height: 50 });
}

function updateCombo(success) {
  if (success) {
    comboCount++;
    score += comboCount * 10;
  } else {
    comboCount = 0;
  }
}

function checkAchievements() {
  achievements.forEach(achievement => {
    if (achievement.condition() && !achievement.unlocked) {
      achievement.unlocked = true;
      alert(`Achievement Unlocked: ${achievement.name}`);
    }
  });
}

function increaseDifficulty() {
  player.speed += 0.5;
  gravity += 0.05;
}

function changeBackground() {
  const backgrounds = ['bg1.png', 'bg2.png', 'bg3.png'];
  document.body.style.backgroundImage = `url(${backgrounds[level % backgrounds.length]})`;
}

function checkLevelUp() {
  if (score >= level * 100) {
    level++;
    increaseDifficulty();
    changeBackground();
  }
}

function resizeCanvas() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
}

function gameLoop() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  // Gravity and movement
  player.velocityY += gravity;
  player.y += player.velocityY;

  if (player.y > canvas.height - player.height) {
    player.y = canvas.height - player.height;
    player.jumping = false;
  }

  // Draw player
  ctx.fillStyle = 'blue';
  ctx.fillRect(player.x, player.y, player.width, player.height);

  // Platforms
  platforms.forEach((plat, index) => {
    plat.x -= player.speed;
    ctx.fillStyle = 'green';
    ctx.fillRect(plat.x, plat.y, plat.width, plat.height);

    // Collision detection
    if (
      player.x < plat.x + plat.width &&
      player.x + player.width > plat.x &&
      player.y + player.height < plat.y + 10 &&
      player.y + player.height > plat.y
    ) {
      player.y = plat.y - player.height;
      player.jumping = false;
      player.velocityY = 0;
    }

    if (plat.x + plat.width < 0) platforms.splice(index, 1);
  });

  // Spawn logic
  if (Math.random() < 0.02) generatePlatform();
  if (Math.random() < 0.01) generatePowerUp();
  if (Math.random() < 0.015) generateObstacle();

  // Update level
  checkLevelUp();
  checkAchievements();

  requestAnimationFrame(gameLoop);
}

window.addEventListener('resize', resizeCanvas);
resizeCanvas();

document.addEventListener('keydown', (e) => {
  if (e.code === 'Space' && !player.jumping) {
    player.velocityY = -10;
    player.jumping = true;
    playJumpSound();
  }
});

canvas.addEventListener('touchstart', function(e) {
  if (!player.jumping) {
    player.velocityY = -10;
    player.jumping = true;
    playJumpSound();
  }
});

gameLoop();
