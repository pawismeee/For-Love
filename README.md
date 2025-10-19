
<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Em có yêu anh không?</title>
  <style>
    html, body {
      margin: 0;
      padding: 0;
      background: #000;
      overflow: hidden;
      font-family: 'Arial', sans-serif;
      height: 100%;
      width: 100%;
      touch-action: manipulation;
    }

    .rain-container {
      position: absolute;
      width: 100vw;
      height: 100vh;
      overflow: hidden;
      pointer-events: none;
    }

    .phrase, .emoji-image {
      position: absolute;
      top: -50px;
      animation: fall linear forwards;
    }

    .phrase {
      font-size: clamp(16px, 5vw, 36px);
      font-weight: bold;
      white-space: nowrap;
    }

    .emoji-image {
      width: 40px;
      height: 40px;
      pointer-events: none;
    }

    @keyframes fall {
      to {
        transform: translateY(110vh);
        opacity: 0;
      }
    }

    .stars {
      position: fixed;
      width: 100%;
      height: 100%;
      background: url('https://raw.githubusercontent.com/VincentGarreau/particles.js/master/demo/img/stars.png') repeat;
      z-index: -1;
      animation: moveStars 200s linear infinite;
    }

    @keyframes moveStars {
      0% { background-position: 0 0; }
      100% { background-position: -10000px 5000px; }
    }

    .question {
      position: fixed;
      top: 30%;
      left: 50%;
      transform: translateX(-50%);
      text-align: center;
      font-size: 28px;
      font-weight: bold;
      z-index: 10;
      background: linear-gradient(90deg, #ff3399, #ff66cc, #ffcc00, #00ffff, #ff3399);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-size: 300% 100%;
      animation: colorShift 5s linear infinite;
    }

    @keyframes colorShift {
      0% { background-position: 0% 50%; }
      100% { background-position: 300% 50%; }
    }

    .buttons {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      display: flex;
      justify-content: center;
      gap: 60px;
      z-index: 10;
      background-color: rgba(255, 105, 180, 0.2);
      padding: 20px;
      border-radius: 20px;
      backdrop-filter: blur(8px);
    }

    .btn {
      padding: 15px 25px;
      font-size: 18px;
      border: none;
      border-radius: 12px;
      cursor: pointer;
      background-color: #fff;
      color: #000;
      transition: all 0.3s ease;
      box-shadow: 0 0 10px #ff99cc;
    }

    .btn:hover {
      background-color: #ffccff;
      color: #000;
    }

    .explode {
      animation: explode 0.6s ease forwards;
    }

    @keyframes explode {
      0% { transform: scale(2.5); opacity: 1; }
      100% { transform: scale(5); opacity: 0; }
    }

    .love-message {
      position: fixed;
      top: 45%;
      left: 50%;
      transform: translate(-50%, -50%);
      color: #fff;
      font-size: 28px;
      font-weight: bold;
      text-align: center;
      z-index: 999;
      display: none;
      opacity: 0;
    }

    .love-message.show {
      display: block;
      animation: fadeIn 2s ease-in-out forwards, glow 3s ease-in-out infinite;
    }

    .love-message.fadeout {
      animation: fadeOut 3s ease-in-out forwards;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translate(-50%, -60%) scale(0.8); }
      to { opacity: 1; transform: translate(-50%, -50%) scale(1); }
    }

    @keyframes fadeOut {
      0% { opacity: 1; transform: translate(-50%, -50%) scale(1); }
      100% { opacity: 0; transform: translate(-50%, -55%) scale(1.1); }
    }

    @keyframes glow {
      0%, 100% { text-shadow: 0 0 10px #ff3399, 0 0 20px #ff66cc, 0 0 30px #ffccff; }
      50% { text-shadow: 0 0 20px #ff99cc, 0 0 40px #ff66ff, 0 0 60px #ffffff; }
    }

    /* Dòng chữ chạy ngang */
    .no-message {
      position: fixed;
      top: 70%;
      left: -100%;
      font-size: 22px;
      color: #f693e0;
      font-weight: bold;
      white-space: nowrap;
      z-index: 999;
      pointer-events: none;
      opacity: 0;
    }

    .no-message.active {
      animation: slideAcross 5s linear forwards;
    }

    @keyframes slideAcross {
      0% { left: 100%; opacity: 0; }
      10% { opacity: 1; }
      50% { left: 50%; transform: translateX(-50%); opacity: 1; }
      100% { left: -120%; opacity: 0; }
    }
  </style>
</head>
<body>

<div class="stars"></div>
<div class="rain-container" id="rain"></div>

<audio id="bgmusic" preload="auto" loop>
  <source src="video.mp3" type="audio/mpeg">
</audio>

<div class="question">Em có yêu anh không? 💖</div>
<div class="buttons">
  <button class="btn yes-btn" id="yes-btn">Có 🥰</button>
  <button class="btn no-btn" id="no-btn">Không 😢</button>
</div>

<div id="love-message" class="love-message">Anh cũng yêu em nhiều lắm 💖</div>
<div id="no-message" class="no-message">Công chúa chắc là không yêu tôi rồi 😢</div>

<script>
  const container = document.getElementById('rain');
  const audio = document.getElementById('bgmusic');
  const yesBtn = document.getElementById('yes-btn');
  const noBtn = document.getElementById('no-btn');
  const question = document.querySelector('.question');
  const buttons = document.querySelector('.buttons');
  const loveMessage = document.getElementById('love-message');
  const noMessage = document.getElementById('no-message');

  const phrases = [
    'I Love You 💖', 'Yêu em vcl 💋', '💗', 'Iu Bbi 💞', 'Mãi Mãi Iu Em', 'Toi Iu Em', 'Love You To The Moon And Back', 'I wait for the day you love me', 'Love me just once',
    'Tôi mãi yêu em 💖', '💌 Anh nhớ em', '🌹 Em là duy nhất', 'Chỉ Yêu Mình Em', 'Iu Công Chúa', 'Em Là Đẹp Nhất', 'Love You To The Moon And Back', 'Love You To The Moon And Back',
    '😘 Hôn em nè', '🎵 Bên em mãi mãi', '❤️', '💘', '💝', '💕', '💗', '💓', '💞'
  ];

  const colors = [
    '#ff4d4d', '#ff66cc', '#ffcc00', '#00ffff',
    '#00ff00', '#ff6600', '#6699ff', '#cc33ff',
    '#ff1493', '#ffa500', '#00fa9a', '#7b68ee',
    '#f08080', '#ffd700', '#1e90ff', '#adff2f',
    '#ff69b4', '#ff7f50', '#da70d6', '#40e0d0',
    '#ff6347', '#8a2be2', '#00ced1', '#e9967a',
    '#ffb6c1', '#ffdab9', '#98fb98', '#ba55d3',
    '#87cefa', '#ff00ff', '#dc143c', '#66cdaa',
    '#f5deb3', '#ffdead', '#e0ffff'
  ];

  let scaleLevel = 1;
  let rainingStarted = false;
  let firstNoClick = true;

  function getRandomItem(arr) {
    return arr[Math.floor(Math.random() * arr.length)];
  }

  function createFallingPhrase() {
    const el = document.createElement('div');
    el.classList.add('phrase');
    el.innerText = getRandomItem(phrases);

    const vw = window.innerWidth;
    const spacing = 60;
    const columns = Math.floor(vw / spacing);
    const column = Math.floor(Math.random() * columns);
    el.style.left = `${column * spacing}px`;

    const color = getRandomItem(colors);
    el.style.color = color;
    el.style.animationDuration = `${2 + Math.random() * 3}s`;
    el.style.textShadow = `0 0 5px ${color}, 0 0 10px ${color}, 0 0 20px ${color}, 0 0 40px ${color}`;
    container.appendChild(el);
    setTimeout(() => el.remove(), 6000);
  }

  function createFallingEmoji() {
    const emojiImages = ['anh11.png', 'anh12.gif', 'anh13.png', 'anh14.gif', 'anh15.gif', 'anh16.png', 'anh17.png', 'anh18.png', 'anh19.png'];
    const img = document.createElement('img');
    img.src = getRandomItem(emojiImages);
    img.classList.add('emoji-image');

    const vw = window.innerWidth;
    const spacing = 50;
    const columns = Math.floor(vw / spacing);
    const column = Math.floor(Math.random() * columns);
    img.style.left = `${column * spacing}px`;
    img.style.animationDuration = `${3 + Math.random() * 3}s`;

    container.appendChild(img);
    setTimeout(() => img.remove(), 7000);
  }

  function startLoveEffect() {
    audio.play().catch(() => {});
    question.style.display = 'none';
    buttons.style.display = 'none';

    loveMessage.classList.remove('fadeout');
    loveMessage.classList.add('show');

    setTimeout(() => {
      loveMessage.classList.remove('show');
      loveMessage.classList.add('fadeout');
    }, 5000);

    if (!rainingStarted) {
      let count = 0;
      setInterval(() => {
        createFallingPhrase();
        if (count % 4 === 0) createFallingEmoji();
        count++;
      }, 200);
      rainingStarted = true;
    }
  }

  yesBtn.addEventListener('click', startLoveEffect);

  noBtn.addEventListener('click', () => {
    if (firstNoClick) {
      noMessage.classList.remove('active');
      void noMessage.offsetWidth; // Trigger reflow to restart animation
      noMessage.classList.add('active');
      firstNoClick = false;
    }

    scaleLevel += 0.2;
    yesBtn.style.transform = `scale(${scaleLevel})`;
    if (scaleLevel >= 2.5) {
      yesBtn.classList.add('explode');
      setTimeout(startLoveEffect, 700);
    }
  });
</script>

</body>
</html>
