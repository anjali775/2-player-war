<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Emoji Battle Arena</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      background: #f2f2f2;
    }
    #arena {
      position: relative;
      width: 600px;
      height: 400px;
      border: 3px solid #333;
      margin: 30px auto;
      background-color: #e0e0e0;
    }
    .player {
      position: absolute;
      font-size: 40px;
      width: 40px;
      height: 40px;
      text-align: center;
    }
    #player1 {
      color: red;
    }
    #player2 {
      color: blue;
    }
    .health-bars {
      display: flex;
      justify-content: space-around;
      margin-top: 20px;
    }
    .health {
      font-size: 20px;
    }
    .winner {
      font-size: 26px;
      color: green;
      margin-top: 20px;
    }
  </style>
</head>
<body>

<h1>⚔️ Emoji Battle Arena 🛡️</h1>
<p>Player 1: W A S D to move, F to attack<br>Player 2: Arrow keys to move, M to attack</p>

<div id="arena">
  <div id="player1" class="player" style="top: 50px; left: 50px;">😡</div>
  <div id="player2" class="player" style="top: 300px; left: 500px;">😈</div>
</div>

<div class="health-bars">
  <div class="health">❤️ Player 1: <span id="health1">100</span></div>
  <div class="health">💙 Player 2: <span id="health2">100</span></div>
</div>

<div class="winner" id="winnerMsg"></div>

<script>
  const p1 = document.getElementById("player1");
  const p2 = document.getElementById("player2");
  const health1 = document.getElementById("health1");
  const health2 = document.getElementById("health2");
  const winnerMsg = document.getElementById("winnerMsg");

  let pos1 = { x: 50, y: 50 };
  let pos2 = { x: 500, y: 300 };
  let hp1 = 100;
  let hp2 = 100;

  const moveAmount = 10;

  function updatePosition() {
    p1.style.left = pos1.x + "px";
    p1.style.top = pos1.y + "px";
    p2.style.left = pos2.x + "px";
    p2.style.top = pos2.y + "px";
  }

  function checkAttack(attacker, target, onHit) {
    const dx = Math.abs(attacker.x - target.x);
    const dy = Math.abs(attacker.y - target.y);
    if (dx <= 40 && dy <= 40) onHit();
  }

  function checkWinner() {
    if (hp1 <= 0) {
      winnerMsg.textContent = "🎉 Player 2 Wins!";
    } else if (hp2 <= 0) {
      winnerMsg.textContent = "🎉 Player 1 Wins!";
    }
  }

  document.addEventListener("keydown", (e) => {
    if (winnerMsg.textContent) return;

    switch (e.key.toLowerCase()) {
      // Player 1 movement (WASD)
      case "w": pos1.y -= moveAmount; break;
      case "s": pos1.y += moveAmount; break;
      case "a": pos1.x -= moveAmount; break;
      case "d": pos1.x += moveAmount; break;
      // Player 2 movement (Arrows)
      case "ArrowUp": pos2.y -= moveAmount; break;
      case "ArrowDown": pos2.y += moveAmount; break;
      case "ArrowLeft": pos2.x -= moveAmount; break;
      case "ArrowRight": pos2.x += moveAmount; break;
      // Player 1 attack (F)
      case "f":
        checkAttack(pos1, pos2, () => {
          hp2 -= 10;
          health2.textContent = hp2;
          checkWinner();
        });
        break;
      // Player 2 attack (M)
      case "m":
        checkAttack(pos2, pos1, () => {
          hp1 -= 10;
          health1.textContent = hp1;
          checkWinner();
        });
        break;
    }
    updatePosition();
  });

  updatePosition();
</script>

</body>
</html>
