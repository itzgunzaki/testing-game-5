<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Catch the Falling Object</title>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@700&family=Spectral:wght@700&display=swap" rel="stylesheet">
  <style>
    body {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      background: linear-gradient(to bottom, #4B0082, #E6E6FA);
      font-family: 'Roboto', sans-serif;
    }

    #gameArea {
      position: relative;
      width: 400px;
      height: 400px;
      border: 2px solid #333;
      overflow: hidden;
      background-color: white;
      box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
      margin-bottom: 20px;
      display: none;
    }

    #fallingObject {
      position: absolute;
      width: 50px;
      height: 50px;
      background: linear-gradient(135deg, #8A2BE2, #9370DB);
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
      transition: transform 0.2s;
    }

    #score,
    #highestScore {
      font-size: 24px;
      color: #4B0082;
      font-weight: bold;
      background-color: rgba(255, 255, 255, 0.8);
      padding: 5px;
      border-radius: 5px;
      box-shadow: 0 0 5px rgba(0, 0, 0, 0.5);
      margin-bottom: 5px;
    }

    button {
      padding: 10px 20px;
      font-size: 18px;
      background-color: #4B0082;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      transition: background-color 0.3s, transform 0.2s;
      margin: 5px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
    }

    button:hover {
      background-color: #5C009E;
      transform: scale(1.05);
    }

    #retryButton {
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      display: none;
      text-align: center;
    }

    #difficultyButtons {
      margin-top: 10px;
    }
  </style>
</head>

<body>
  <div id="score">Score: 0</div>
  <div id="highestScore">Highest Score: 0</div>

  <div id="difficultyButtons">
    <button onclick="setDifficulty('easy')">Easy</button>
    <button onclick="setDifficulty('medium')">Medium</button>
    <button onclick="setDifficulty('hard')">Hard</button>
  </div>

  <div id="gameArea" onclick="backgroundClick()">
    <div id="fallingObject"></div>
    <button id="retryButton" onclick="showDifficulty()">Retry</button>
  </div>

  <script>
    const gameArea = document.getElementById('gameArea');
    const fallingObject = document.getElementById('fallingObject');
    const scoreDisplay = document.getElementById('score');
    const highestScoreDisplay = document.getElementById('highestScore');
    const retryButton = document.getElementById('retryButton');
    let score = 0;
    let highestScore = 0;
    let objectY = 0;
    let objectX = Math.random() * 350;
    let interval;
    let speed = 50;
    let size = 40;
    const lossThreshold = 300;
    let gameActive = false;

    function startGame() {
      gameArea.style.display = 'block';
      retryButton.style.display = 'none';
      fallingObject.style.left = objectX + 'px';
      fallingObject.style.width = size + 'px';
      fallingObject.style.height = size + 'px';
      objectY = 0;
      score = 0;
      scoreDisplay.innerText = 'Score: ' + score;
      gameActive = true;
      clearInterval(interval);
      interval = setInterval(moveObject, speed);
    }

    function moveObject() {
      objectY += 5;
      if (objectY > gameArea.clientHeight) {
        gameOver();
      }
      fallingObject.style.top = objectY + 'px';
    }

    function resetObject() {
      objectY = 0;
      objectX = Math.random() * 350;
      fallingObject.style.left = objectX + 'px';
    }
    fallingObject.addEventListener('click', (event) => {
      event.stopPropagation();
      if (!gameActive) return;
      if (objectY < lossThreshold) {
        score++;
        scoreDisplay.innerText = 'Score: ' + score;
        fallingObject.style.transform = 'scale(1.2)';
        setTimeout(() => {
          fallingObject.style.transform = 'scale(1)';
        }, 200);
        resetObject();
      } else {
        gameOver();
      }
    });

    function backgroundClick() {
      if (gameActive) gameOver();
    }

    function gameOver() {
      clearInterval(interval);
      gameActive = false;
      retryButton.style.display = 'block';
      if (score > highestScore) {
        highestScore = score;
        highestScoreDisplay.innerText = 'Highest Score: ' + highestScore;
      }
    }

    function showDifficulty() {
      retryButton.style.display = 'none';
      document.getElementById('difficultyButtons').style.display = 'block';
      gameArea.style.display = 'none';
    }

    function setDifficulty(level) {
      switch (level) {
        case 'easy':
          speed = 50;
          size = 40;
          break;
        case 'medium':
          speed = 30;
          size = 40;
          break;
        case 'hard':
          speed = 20;
          size = 40;
          break;
      }
      document.getElementById('difficultyButtons').style.display = 'none';
      startGame();
    }
    window.onload = function() {
      document.getElementById('difficultyButtons').style.display = 'block';
    };
  </script>
</body>

</html>
