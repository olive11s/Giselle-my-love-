# Giselle-my-love-
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>For Giselle ❤️</title>
<style>
    html, body {
        margin: 0; padding: 0; overflow: hidden;
        font-family: 'Arial', sans-serif; touch-action: none; background: #ffe6f2;
    }
    #gamePhase, #valentinePhase, #finalMessage, #startOverlay {
        width: 100vw; height: 100vh; position: absolute; top: 0; left: 0; display: none;
    }
    #startOverlay {
        background: #ffe6f2; display: flex; flex-direction: column; 
        justify-content: center; align-items: center; z-index: 100;
    }
    #gamePhase { background: linear-gradient(to top, #ff9a9e 0%, #fecfef 100%); }
    
    #basket {
        position: absolute; bottom: 8vh; left: 50%;
        width: 100px; height: 100px;
        transform: translateX(-50%);
        z-index: 10; font-size: 70px;
        display: flex; justify-content: center; align-items: center;
    }

    .tulip {
        position: absolute; width: 60px; height: 60px;
        font-size: 50px; text-align: center; z-index: 5;
        display: flex; justify-content: center; align-items: center;
    }

    #timer, #score {
        position: absolute; top: 5vh; font-size: 24px;
        font-weight: bold; color: #fff; text-shadow: 1px 1px 5px rgba(0,0,0,0.3); z-index: 20;
    }
    #timer { left: 5vw; }
    #score { right: 5vw; }

    #valentinePhase { background-color: #ffcce6; text-align: center; flex-direction: column; justify-content: center; align-items: center; }
    
    #noMessage {
        color: #d6336c; font-weight: bold; font-size: 18px;
        height: 30px; margin-bottom: 10px; padding: 0 20px;
    }

    #finalMessage { 
        background: white; display: none; flex-direction: column; 
        justify-content: center; align-items: center; text-align: center;
        padding: 20px; box-sizing: border-box;
    }
    .message-card {
        background: #fff0f5; padding: 30px; border-radius: 20px;
        box-shadow: 0 10px 30px rgba(0,0,0,0.1); border: 2px solid #ffb3d1;
        max-width: 85%;
    }
    .btn { padding: 15px 30px; font-size: 22px; border-radius: 50px; border: none; cursor: pointer; position: absolute; transition: transform 0.2s; }
    #yesBtn { background: #ff4d94; color: white; bottom: 20vh; left: 15%; box-shadow: 0 5px 15px rgba(255,77,148,0.4); z-index: 50; }
    #noBtn { background: #aaa; color: white; bottom: 20vh; right: 15%; z-index: 50; }
    
    .falling-emoji {
        position: absolute; top: -60px; z-index: 99;
        animation: fall linear forwards;
    }
    @keyframes fall {
        to { transform: translateY(110vh) rotate(360deg); }
    }
</style>
</head>
<body>

<div id="startOverlay" style="display: flex;">
    <h1 style="color: #ff4d94; font-size: 36px;">Hi Giselle! ❤️</h1>
    <p style="color: #d6336c; font-size: 18px;">Catch the tulips 🌷</p>
    <button onclick="initGame()" style="padding: 20px 40px; font-size: 24px; border-radius: 50px; border: none; background: #ff4d94; color: white; margin-top: 20px;">PLAY 🌷</button>
</div>

<div id="gamePhase">
    <div id="timer">Time: 15</div>
    <div id="score">🌷: 0</div>
    <div id="basket">🧺</div>
</div>

<div id="valentinePhase">
    <div id="noMessage"></div>
    <h2 style="font-size: 28px; color: #ff4d94; padding: 0 10px;">Will you be my Valentine? 💌</h2>
    <button id="yesBtn" class="btn">YES! 💖</button>
    <button id="noBtn" class="btn">No 😢</button>
</div>

<div id="finalMessage">
    <div class="message-card">
        <h1 style="color: #ff4d94;">Yay! 💖</h1>
        <p style="font-size: 20px; color: #555; line-height: 1.6;">
            Giselle, you make every day brighter.<br>
            I'm so lucky to have you.<br><br>
            <strong>Happy Valentine's Day!</strong><br>
            💋💋💋
        </p>
    </div>
</div>

<audio id="music" loop>
    <source src="https://www.bensound.com/bensound-music/bensound-romantic.mp3" type="audio/mpeg">
</audio>

<script>
const basket = document.getElementById('basket');
const scoreEl = document.getElementById('score');
const timerEl = document.getElementById('timer');
const music = document.getElementById('music');
const noBtn = document.getElementById('noBtn');
const yesBtn = document.getElementById('yesBtn');
const noMessage = document.getElementById('noMessage');

let score = 0;
let timeLeft = 15;
let gameActive = false;
let noClickCount = 0;
let yesScale = 1;

const phrases = [
    "No? Don't press that! 🥺",
    "That hurts my feelings... 💔",
    "Are you sure? 🤨",
    "One more time and no kisses! 💋🚫",
    "You're breaking my heart! 😭",
    "Think about the tulips! 🌷",
    "Wrong button! Try the pink one! 👉"
];

function initGame() {
    document.getElementById('startOverlay').style.display = 'none';
    document.getElementById('gamePhase').style.display = 'block';
    music.play().catch(() => {});
    gameActive = true;
    startTimer();
    spawnTulips();
}

function startTimer() {
    const timerInterval = setInterval(() => {
        timeLeft--;
        timerEl.textContent = `Time: ${timeLeft}`;
        if (timeLeft <= 0) {
            clearInterval(timerInterval);
            gameActive = false;
            showValentine();
        }
    }, 1000);
}

function spawnTulips() {
    if (!gameActive) return;
    createTulip();
    setTimeout(spawnTulips, 500);
}

function createTulip() {
    const tulip = document.createElement('div');
    tulip.className = 'tulip';
    tulip.innerHTML = '🌷';
    tulip.style.left = Math.random() * (window.innerWidth - 60) + 'px';
    tulip.style.top = '-60px';
    document.getElementById('gamePhase').appendChild(tulip);

    let pos = -60;
    const fall = setInterval(() => {
        pos += 6;
        tulip.style.top = pos + 'px';
        const bRect = basket.getBoundingClientRect();
        const tRect = tulip.getBoundingClientRect();

        if (tRect.bottom >= bRect.top + 20 && tRect.right >= bRect.left && tRect.left <= bRect.right && tRect.top <= bRect.bottom) {
            score++;
            scoreEl.textContent = `🌷: ${score}`;
            tulip.remove();
            clearInterval(fall);
        }
        if (pos > window.innerHeight) { tulip.remove(); clearInterval(fall); }
    }, 20);
}

document.addEventListener('touchmove', (e) => {
    if (!gameActive) return;
    let touchX = e.touches[0].clientX;
    let newLeft = touchX - (basket.offsetWidth / 2);
    if (newLeft < 0) newLeft = 0;
    if (newLeft > window.innerWidth - basket.offsetWidth) newLeft = window.innerWidth - basket.offsetWidth;
    basket.style.left = newLeft + 'px';
}, { passive: false });

function showValentine() {
    document.getElementById('gamePhase').style.display = 'none';
    document.getElementById('valentinePhase').style.display = 'flex';
}

function handleNoAction(e) {
    if(e) e.preventDefault();
    noMessage.textContent = phrases[noClickCount % phrases.length];
    noClickCount++;
    const maxX = window.innerWidth - noBtn.offsetWidth;
    const maxY = window.innerHeight - noBtn.offsetHeight;
    noBtn.style.left = Math.random() * maxX + 'px';
    noBtn.style.top = Math.random() * maxY + 'px';
    yesScale += 0.15;
    yesBtn.style.transform = `scale(${yesScale})`;
}

noBtn.addEventListener('touchstart', handleNoAction);
noBtn.addEventListener('click', handleNoAction);

yesBtn.addEventListener('click', () => {
    // Quick playful message before the sweet final card
    noMessage.textContent = "I knew you couldn't say no! 😉";
    setTimeout(() => {
        document.getElementById('valentinePhase').style.display = 'none';
        document.getElementById('finalMessage').style.display = 'flex';
        celebrate();
    }, 1200);
});

function celebrate() {
    const emojis = ['🌷', '💖', '✨', '💋', '🌸'];
    for(let i=0; i<60; i++) {
        setTimeout(() => {
            const el = document.createElement('div');
            el.className = 'falling-emoji';
            el.innerHTML = emojis[Math.floor(Math.random() * emojis.length)];
            el.style.left = Math.random() * 100 + 'vw';
            el.style.fontSize = Math.random() * 20 + 30 + 'px';
            el.style.animationDuration = Math.random() * 2 + 2 + 's';
            document.body.appendChild(el);
            setTimeout(() => el.remove(), 4000);
        }, i * 80);
    }
}
</script>
</body>
</html>
