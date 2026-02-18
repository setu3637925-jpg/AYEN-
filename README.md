<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>AYEN_AI_V4</title>

<style>
body { -webkit-user-select:none; user-select:none; margin:0; background:transparent; font-family:sans-serif; }

#ai-box {
    position:fixed; top:100px; left:15px; width:150px;
    background:rgba(10,15,25,0.98); border:1.5px solid #00f2ff; border-radius:12px;
    box-shadow:0 0 20px rgba(0,242,255,0.4); z-index:100000; touch-action:none;
}

.handle {
    width:100%; height:22px; background:#00f2ff; cursor:move;
    border-radius:10px 10px 0 0; display:flex; align-items:center; justify-content:center;
}
.handle::after { content:"MOVE AYEN-AI"; font-size:9px; font-weight:bold; color:#000; }

.content { padding:10px; text-align:center; }
.title { color:#00f2ff; font-size:11px; font-weight:bold; margin-bottom:8px; }

input[type="password"] {
    width:80%; padding:5px; border-radius:5px; border:1px solid #334155;
    background:#000; color:#fff; margin-bottom:8px; font-size:10px; text-align:center;
}
button {
    background:#00f2ff; border:none; padding:5px 12px; border-radius:5px;
    font-size:10px; font-weight:bold; cursor:pointer;
}

#training-sec, #main-ui { display:none; }
.train-btn { width:45%; margin:2px; padding:6px; font-size:9px; }
.big-btn { background:#ff4757; color:white; }
.small-btn { background:#2ed573; color:white; }

.timer-txt { font-size:11px; color:#fbbf24; margin-bottom:5px; }
.res-box {
    background:#000; border:1px solid #1e293b; border-radius:8px;
    padding:12px 0; margin:6px 0;
}
#val { font-size:22px; font-weight:900; }
.status { font-size:8px; color:#4ade80; animation:blink 1s infinite; }
@keyframes blink { 50% { opacity:0.4; } }
</style>
</head>

<body oncontextmenu="return false;">

<div id="ai-box">
<div class="handle" id="moveTrigger"></div>
<div class="content">

<div class="title">AYEN SYSTEM V4</div>

<div id="login-sec">
    <input type="password" id="pass-field" placeholder="Enter Password">
    <button onclick="verify()">LOGIN</button>
</div>

<div id="training-sec">
    <div style="color:#fff; font-size:9px; margin-bottom:8px;">
        Select Last 3 Results:
    </div>
    <button class="train-btn big-btn" onclick="addHistory('BIG')">BIG</button>
    <button class="train-btn small-btn" onclick="addHistory('SMALL')">SMALL</button>
    <div id="history-dots" style="margin-top:8px; color:#00f2ff; font-size:12px;"></div>
</div>

<div id="main-ui">
    <div class="timer-txt">TIME: <span id="clock">--</span>s</div>
    <div class="res-box">
        <div id="val">---</div>
    </div>
    <div class="status">● AI CONNECTED</div>
</div>

</div>
</div>

<script>
const KEY = "65";
const PATTERN = ["BIG","BIG","SMALL","BIG","SMALL","SMALL","BIG","SMALL"];
let history = [];

function verify() {
    if(document.getElementById('pass-field').value === KEY){
        document.getElementById('login-sec').style.display='none';
        document.getElementById('training-sec').style.display='block';
    } else {
        alert("Wrong Password!");
    }
}

function addHistory(type){
    history.push(type);
    document.getElementById('history-dots').innerText += (type==='BIG'?' B':' S');

    if(history.length===3){
        document.getElementById('training-sec').style.display='none';
        document.getElementById('main-ui').style.display='block';
        startHack();
    }
}

function startHack(){

    const valEl = document.getElementById('val');

    setInterval(()=>{

        let now = Math.floor(Date.now()/1000);

        // গ্লোবাল ৩০ সেকেন্ড সাইকেল
        let timer = 30 - (now % 30);
        document.getElementById('clock').innerText = timer;

        // গ্লোবাল প্যাটার্ন সাইকেল
        let cycleIndex = Math.floor(now/30);
        let patternIndex = cycleIndex % PATTERN.length;

        let result = PATTERN[patternIndex];

        valEl.innerText = result;
        valEl.style.color = (result==="BIG")?"#ff4757":"#2ed573";

    },1000);
}

// Drag system
const box=document.getElementById('ai-box');
const drag=document.getElementById('moveTrigger');
let isMoving=false,curX=0,curY=0,startX=0,startY=0;

const onStart=(e)=>{
    isMoving=true;
    const t=e.type==='touchstart'?e.touches[0]:e;
    startX=t.clientX-curX;
    startY=t.clientY-curY;
};

const onMove=(e)=>{
    if(!isMoving)return;
    const t=e.type==='touchmove'?e.touches[0]:e;
    curX=t.clientX-startX;
    curY=t.clientY-startY;
    box.style.transform=`translate3d(${curX}px,${curY}px,0)`;
};

drag.addEventListener('mousedown',onStart);
drag.addEventListener('touchstart',onStart);
document.addEventListener('mousemove',onMove);
document.addEventListener('touchmove',onMove,{passive:false});
document.addEventListener('mouseup',()=>isMoving=false);
document.addEventListener('touchend',()=>isMoving=false);

</script>
</body>
</html>
