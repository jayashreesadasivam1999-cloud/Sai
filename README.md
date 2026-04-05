<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dreamy Apology</title>
<style>
body{
  margin:0;overflow:hidden;font-family:Segoe UI;
  background: radial-gradient(circle at center, #2a1a3f, #000);
  color:#fff;
}

/* 🌌 STARS */
.star{position:absolute;width:2px;height:2px;background:#fff;animation:twinkle 2s infinite;}
@keyframes twinkle{50%{opacity:0.3;}}

/* 🌧 RAIN */
.rain{position:absolute;width:1px;height:20px;background:rgba(255,255,255,0.2);animation:rainFall linear infinite;}
@keyframes rainFall{to{transform:translateY(100vh);}}

/* GLASS */
.glass{
  background: rgba(200,150,255,0.08);
  backdrop-filter: blur(20px);
  padding:40px;
  border-radius:20px;
  box-shadow:0 0 40px rgba(200,150,255,0.4);
}

/* CENTER */
.center{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);text-align:center;}

/* NEON TEXT */
h2,h3{text-shadow:0 0 10px #d8b4ff,0 0 20px #c084fc;}

/* BUTTON */
.btn{
  padding:14px 28px;margin:10px;
  border:none;border-radius:30px;
  cursor:pointer;font-size:18px;
  transition:0.4s;
}
.accept{background:linear-gradient(45deg,#c084fc,#e9d5ff);box-shadow:0 0 20px #c084fc;}
.deny{background:rgba(255,255,255,0.1);}

/* 💔 GLASS BREAK */
.break{animation:breakGlass 0.8s forwards;}
@keyframes breakGlass{0%{transform:scale(1);opacity:1;}100%{transform:scale(2) rotate(20deg);opacity:0;}}

/* LETTER / PAPER */
#letterScene,#forgiveScene{display:none;}
.paper{
  width:320px;height:220px;padding:20px;color:black;
  background:white;border-radius:10px;
  box-shadow:inset 0 0 20px rgba(0,0,0,0.8),0 0 20px rgba(0,0,0,0.5);
  transform:rotateX(-90deg);transform-origin:top;transition:1.2s;
}
.paper.open{transform:rotateX(0);}
.text{opacity:0;line-height:1.6;}

/* ✂️ TEAR EFFECT */
.tear{animation:tear 2s forwards;}
@keyframes tear{
  0%{clip-path:polygon(0 0,100% 0,100% 100%,0 100%);}
  100%{clip-path:polygon(0 0,100% 0,60% 50%,40% 50%,100% 100%,0 100%);
  transform:translateY(100vh) rotate(10deg);opacity:0;}
}

/* FADE */
.fade{animation:fade 2s forwards;}
@keyframes fade{to{opacity:0;}}
</style>
</head>
<body>

<audio id="bgm" loop>
<source src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_115b9b48c1.mp3">
</audio>

<script>
/* PARTICLES */
for(let i=0;i<100;i++){
 let s=document.createElement("div"); s.className="star";
 s.style.left=Math.random()*100+"vw";
 s.style.top=Math.random()*100+"vh";
 document.body.appendChild(s);
}
for(let i=0;i<80;i++){
 let r=document.createElement("div"); r.className="rain";
 r.style.left=Math.random()*100+"vw";
 r.style.animationDuration=(1+Math.random()*2)+"s";
 document.body.appendChild(r);
}
</script>

<!-- SCENE 1 -->
<div id="scene1" class="center glass">
<h2>Please accept my apology for the mistake I made earlier.</h2>
<button id="accept" class="btn accept">Accept ❤️</button>
<button id="deny" class="btn deny">Deny 💔</button>
</div>

<!-- SCENE 1.5: Forgive -->
<div id="forgiveScene" class="center glass">
<h3 id="forgiveMsg">I know you will forgive me 🙇🏼‍♀️🌻❤️</h3>
<button id="openLetterBtn" class="btn accept">Open to view letter 🌻💌</button>
</div>

<!-- SCENE 2: Letter -->
<div id="letterScene" class="center">
<div id="paper" class="paper">
<div id="text" class="text"></div>
</div>
</div>

<script>
const audio=document.getElementById("bgm");
function playMusic(){ if(audio.paused){audio.volume=0.25; audio.play().catch(()=>{}); }}

/* Scene1 Logic */
let denyTexts=[
"Are you sure 💔","Really you want 😣","Think again!","Think twice 🥺","Think thrice 😅",
"Take deep breath 😮‍💨","Then don’t regret 😷","You might be wrong",
"Final answer 🫠","You're breaking my heart 💔😣"
];
let i=0,scale=1;
const acceptBtn=document.getElementById("accept");
const denyBtn=document.getElementById("deny");

acceptBtn.onclick=()=>{
 playMusic();
 document.getElementById("scene1").classList.add("fade");
 setTimeout(()=>{
  document.getElementById("scene1").style.display="none";
  showForgiveScene();
 },2000);
};

denyBtn.onclick=()=>{
 playMusic(); i++; scale+=0.2; acceptBtn.style.transform=`scale(${scale})`;
 if(i<denyTexts.length){ denyBtn.innerText=denyTexts[i]; }
 else{
  denyBtn.classList.add("break");
  setTimeout(()=>{denyBtn.style.display="none";},800);
  setTimeout(showForgiveScene,2000);
 }
};

/* Scene1.5: Forgive */
function showForgiveScene(){
 let forgiveScene=document.getElementById("forgiveScene");
 forgiveScene.style.display="block";
 let forgiveMsg=document.getElementById("forgiveMsg");
 forgiveMsg.style.opacity=0;
 forgiveMsg.style.transition="opacity 1s";
 setTimeout(()=>{forgiveMsg.style.opacity=1;},200);

 let openBtn=document.getElementById("openLetterBtn");
 openBtn.style.display="none";
 setTimeout(()=>{openBtn.style.display="inline-block";},3000);

 openBtn.onclick=()=>{openLetter();};
}

/* Scene2: Open Letter */
function openLetter(){
 playMusic();
 document.getElementById("forgiveScene").classList.add("fade");
 setTimeout(()=>{
  document.getElementById("forgiveScene").style.display="none";
  document.getElementById("letterScene").style.display="block";
  let paper=document.getElementById("paper"); paper.classList.add("open");
  setTimeout(typeText,1000);
 },1000);
}

/* Handwriting Type + Paper Tear */
function typeText(){
 let msg=`I Let you go with smile...
But you'll never know how much it hurt to act strong...
When all I wanted was you to hold me and stay 💔🌻`;

 let box=document.getElementById("text"); box.style.opacity=1;
 let index=0;
 function write(){
  if(index<msg.length){ box.innerHTML+=msg[index]; index++; setTimeout(write,40);}
  else{ setTimeout(()=>{document.getElementById("paper").classList.add("tear");},30000);}
 }
 write();
}
</script>
</body>
</html>
