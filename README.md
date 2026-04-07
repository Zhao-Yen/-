<H1> 自我簡介 </H1>

---------------------------
| 項次 | 項目 | 內容 |
|----:|------|------|
|1 | 圖片 | <img src="IMG_6043.jpeg" width="100" Height="500" />|
|2 | 姓名 | 陳肇延 |
|3 | 年級 | 大二 |
|4 | 就讀學校 | 龍華科大 |
|5 | 科系 | 資訊管理 |
|6 | 興趣 | 通常都在家裡玩電腦 |
----------------------------
最近都聽<br>
<a href="https://youtu.be/F--27qiJfRg?si=1Blgb2tsirgUr2PF" target="_blank"><img src="7735661.jpg" 
alt="phonk 流行曲" width="200" height="250" border="10" /></a>
<br>影片取自 youtube


<H1> 組員列表 </H1>

---------------------------
| A 組 | 姓名 | Github連結 |
|----:|------|------|
|組長 | 陳肇延 | [陳肇延 github](https://github.com/Zhao-Yen/-)|
|組員 | 官威宏 | [官威宏 github](https://github.com/shiinamashironoe/d1134423036)|
|組員 | 陳則閔 | [陳則閔 github](https://github.com/tse0218/1150324/blob/main/README.md)|
|組員 | 楊博涵 | [楊博涵 github]|
<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>團隊專用抽籤</title>
<style>
:root { --primary: #4f46e5; --bg: #fdfdfd; --card: #ffffff; --text: #0f172a; }
body { font-family: -apple-system, sans-serif; background: var(--bg); display: flex; justify-content: center; align-items: center; min-height: 100vh; margin: 0; }
.container { width: 92%; max-width: 380px; background: var(--card); padding: 2rem; border-radius: 30px; box-shadow: 0 20px 40px rgba(0,0,0,0.05); border: 1px solid #f1f5f9; }
h1 { font-size: 1.25rem; text-align: center; color: var(--text); letter-spacing: -0.5px; margin-bottom: 20px; }

textarea {
width: 100%; height: 110px; border: 2px solid #f1f5f9; border-radius: 18px;
padding: 15px; box-sizing: border-box; font-size: 1rem; outline: none;
transition: 0.3s; background: #f8fafc; color: #475569; line-height: 1.6;
}
textarea:focus { border-color: var(--primary); background: #fff; }

.result-display {
height: 100px; display: flex; align-items: center; justify-content: center;
margin: 20px 0; font-size: 2.2rem; font-weight: 900; color: var(--primary);
background: #eef2ff; border-radius: 20px; text-align: center;
}

button {
width: 100%; padding: 18px; border: none; border-radius: 16px;
font-weight: 700; cursor: pointer; transition: 0.2s; font-size: 1.1rem;
}
.btn-main { background: var(--primary); color: white; margin-bottom: 12px; box-shadow: 0 8px 15px rgba(79, 70, 229, 0.2); }
.btn-main:active { transform: scale(0.97); }
.btn-reset { background: transparent; color: #94a3b8; font-size: 0.9rem; text-decoration: underline; }

.options { margin-top: 20px; display: flex; justify-content: center; gap: 20px; font-size: 0.9rem; color: #64748b; }
.checkbox-group { display: flex; align-items: center; gap: 6px; cursor: pointer; }

@keyframes winner-anim { 0% { transform: scale(0.9); } 50% { transform: scale(1.05); } 100% { transform: scale(1); } }
.win { animation: winner-anim 0.4s ease-out; background: #4f46e5; color: white !important; }
</style>
</head>
<body>

<div class="container">
<h1>🎯 成員隨機抽籤</h1>

<textarea id="input"></textarea>

<div class="result-display" id="display">準備抽籤</div>

<button class="btn-main" onclick="roll()">開始抽籤</button>
<button class="btn-reset" onclick="resetToDefault()">恢復初始名單</button>

<div class="options">
<label class="checkbox-group"><input type="checkbox" id="removeMode"> 抽後移除</label>
<label class="checkbox-group"><input type="checkbox" id="soundMode" checked> 音效</label>
</div>
</div>

<script>
const defaultList = "陳肇延\n楊博涵\n陳則閔\n官威宏";
const inputEl = document.getElementById('input');
const displayEl = document.getElementById('display');
const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

// 初始化：優先讀取記憶，若無則顯示預設名單
window.onload = () => {
const saved = localStorage.getItem('fixed_draw_list');
inputEl.value = (saved !== null) ? saved : defaultList;
};

inputEl.addEventListener('input', () => {
localStorage.setItem('fixed_draw_list', inputEl.value);
});

function playTone(freq, type, duration, vol = 0.1) {
if (!document.getElementById('soundMode').checked) return;
const osc = audioCtx.createOscillator();
const gain = audioCtx.createGain();
osc.type = type;
osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
gain.gain.setValueAtTime(vol, audioCtx.currentTime);
gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + duration);
osc.connect(gain);
gain.connect(audioCtx.destination);
osc.start();
osc.stop(audioCtx.currentTime + duration);
}

function roll() {
const list = inputEl.value.trim().split('\n').filter(i => i.trim());
if (list.length === 0) return alert("請輸入名單！");

displayEl.classList.remove('win');
let count = 0;
const totalSteps = 15;

const timer = setInterval(() => {
displayEl.innerText = list[Math.floor(Math.random() * list.length)];
playTone(400 + (count * 30), 'sine', 0.08); // 跳動音效
count++;

if (count >= totalSteps) {
clearInterval(timer);
finish(list);
}
}, 60);
}

function finish(list) {
const winner = list[Math.floor(Math.random() * list.length)];
displayEl.innerText = winner;
displayEl.classList.add('win');

// 成功音效 (和弦)
[523, 659, 784].forEach((f, i) => {
setTimeout(() => playTone(f, 'triangle', 0.4, 0.15), i * 100);
});

if (document.getElementById('removeMode').checked) {
const newList = list.filter(item => item !== winner);
inputEl.value = newList.join('\n');
localStorage.setItem('fixed_draw_list', inputEl.value);
}
}

function resetToDefault() {
if(confirm("確定要恢復到「陳肇延、楊博涵...」四人名單嗎？")) {
inputEl.value = defaultList;
localStorage.setItem('fixed_draw_list', defaultList);
displayEl.innerText = "已重設";
displayEl.classList.remove('win');
}
}
</script>

</body>
</html>
