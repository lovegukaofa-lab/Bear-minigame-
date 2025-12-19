# Bear-minigame-<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<title>Pixel Bear</title>

<link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/emailjs-com@3/dist/email.min.js"></script>

<script>
(function(){
  emailjs.init("E3B4DtWnq-mAjmVH2");
})();
</script>

<style>
*{box-sizing:border-box}

body{
  margin:0;
  font-family:'Press Start 2P',cursive;
  background:linear-gradient(#9be7ff,#ffd6e8);
  height:100vh;
  display:flex;
  justify-content:center;
  align-items:center;
  image-rendering:pixelated;
  -webkit-font-smoothing:none;
}

#game{
  background:#fff;
  border:6px solid #000;
  width:95%;
  max-width:600px;
  padding:24px;
  text-align:center;
  box-shadow:8px 8px 0 #000;
}

.bear{font-size:72px;margin-bottom:16px}

.box{
  border:4px solid #000;
  padding:18px;
  margin-bottom:20px;
  background:#fffaf0;
  box-shadow:4px 4px 0 #000;
  min-height:120px;
  line-height:1.8;
}

#answers{position:relative}

button{
  font-family:'Press Start 2P',cursive;
  font-size:12px;
  padding:14px 16px;
  margin:8px;
  border:3px solid #000;
  background:#ffb6c1;
  box-shadow:3px 3px 0 #000;
  cursor:pointer;
}

button:active{
  box-shadow:1px 1px 0 #000;
  transform:translate(2px,2px);
}

#noBtn{position:absolute}

/* หัวใจลอย */
.heart{
  position:absolute;
  font-size:24px;
  animation:floatUp 2.5s linear forwards;
  pointer-events:none;
}
@keyframes floatUp{
  from{opacity:1;transform:translateY(0)}
  to{opacity:0;transform:translateY(-120px) scale(1.4)}
}

/* 🔒 Lock Screen */
#lockScreen{
  position:fixed;
  inset:0;
  background:linear-gradient(#9be7ff,#ffd6e8);
  display:flex;
  flex-direction:column;
  justify-content:center;
  align-items:center;
  z-index:9999;
}

#lockScreen input{
  font-family:'Press Start 2P',cursive;
  padding:12px;
  border:3px solid #000;
  width:240px;
  text-align:center;
}
</style>
</head>

<body>

<div id="lockScreen">
  <div class="bear">🧸🔒</div>
  <div class="box" id="lockText">
    ใส่รหัสลับก่อนเข้าเกมนะ 💖
  </div>
  <input id="passwordInput" type="password" placeholder="รหัสลับ">
  <br><br>
  <button onclick="checkPassword()">เริ่มเกม</button>
</div>

<div id="game">
  <div class="bear" id="bear">🧸</div>
  <div class="box" id="question"></div>
  <div id="answers"></div>
</div>

<script>
/* 🔑 เปลี่ยนรหัสตรงนี้ */
const PASSWORD = "foryouonly";

function checkPassword(){
  const input=document.getElementById("passwordInput").value;
  const text=document.getElementById("lockText");

  if(input===PASSWORD){
    document.getElementById("lockScreen").style.display="none";
  }else{
    text.innerHTML="รหัสไม่ถูกนะ… หรือไม่ใช่คนที่หมีรอ 🥺";
  }
}

/* ====== เกมเดิม ====== */
const bear=document.getElementById("bear");
const qBox=document.getElementById("question");
const aBox=document.getElementById("answers");

let step=0;
let shyCount=0;
let result={};

const questions=[
 {q:"ถ้าวันนี้เป็นวันหยุด เธออยากทำอะไร?",a:["นอน","ดูซีรีส์","หาอะไรกิน","มีคนไปด้วย"]},
 {q:"คิดว่าหมีตัวนี้น่ารักมั้ย?",a:["น่ารักมาก","ก็น่ารัก","คนถามน่ารักกว่า"]},
 {q:"ถ้ามีคนชวนออกไปทันที?",a:["เขินแต่ไป","ขอคิด","ไปสิ","หนีแต่หนีไม่พ้น"]},
 {q:"ชอบดูหนังแนวไหน?",a:["ตลก","โรแมนติก","สยองขวัญ","อะไรก็ได้"]}
];

const sadLevel1=["เอ๊ะ… หนีทำไม 😳","หมีแค่ถามเองนะ","ลองคิดอีกทีมั้ย 🧸"];
const sadLevel2=["แอบเสียใจนิดนึงนะ… 😔","หมีเตรียมป๊อปคอร์นไว้แล้ว 🍿","หรือเราคิดไปคนเดียว 😢"];
const sadLevel3=["ถ้าไม่อยากไป… ก็บอกกันตรงๆ ก็ได้นะ 💔","หมีเข้าใจนะ แต่ก็แอบจุก 🧸","โอเค… หมีจะไม่หนีแล้ว 😞"];

function typeText(text,cb){
  qBox.innerHTML="";
  let i=0;
  const t=setInterval(()=>{
    qBox.innerHTML+=text[i++];
    if(i>=text.length){clearInterval(t);if(cb)cb();}
  },40);
}

function spawnHeart(){
  const h=document.createElement("div");
  h.className="heart";
  h.textContent="💖";
  h.style.left=Math.random()*80+10+"%";
  h.style.bottom="20px";
  document.body.appendChild(h);
  setTimeout(()=>h.remove(),2500);
}

function next(){
  aBox.innerHTML="";
  if(step<questions.length){
    bear.textContent="🧸😄";
    typeText(questions[step].q,()=>{
      questions[step].a.forEach(ans=>{
        const b=document.createElement("button");
        b.textContent=ans;
        b.onclick=()=>{step++;next()};
        aBox.appendChild(b);
      });
    });
  }else finalQuestion();
}

function finalQuestion(){
  bear.textContent="🧸😳";
  typeText("งั้น… ไปดูหนังด้วยกันมั้ย?",()=>{
    const yes=document.createElement("button");
    yes.textContent="ไปสิ 💕";
    yes.onclick=sweetPage;

    const no=document.createElement("button");
    no.textContent="ไม่ไปอ่ะ";
    no.id="noBtn";

    no.onmouseover=()=>{
      shyCount++;
      bear.textContent="🧸😢";
      let text=shyCount<=2?sadLevel1:shyCount<=4?sadLevel2:sadLevel3;
      qBox.innerHTML=text[Math.floor(Math.random()*text.length)];
      no.style.left=Math.random()*200-100+"px";
      no.style.top=Math.random()*100-50+"px";
    };

    aBox.appendChild(yes);
    aBox.appendChild(no);
  });
}

function sweetPage(){
  bear.textContent="🐻💖";
  spawnHeart();
  typeText("งั้นเลือกกันเลยดีกว่ามั้ย 🎬✨",()=>{
    aBox.innerHTML="";
    ["ตลก","โรแมนติก","อะไรก็ได้","สยองขวัญ"].forEach(m=>{
      const b=document.createElement("button");
      b.textContent="🎬 "+m;
      b.onclick=()=>{result.movie=m;chooseDay()};
      aBox.appendChild(b);
    });
  });
}

function chooseDay(){
  bear.textContent="🐻🥰";
  spawnHeart();
  typeText("สะดวกวันไหน?",()=>{
    aBox.innerHTML="";
    ["เสาร์","อาทิตย์","วันธรรมดา"].forEach(d=>{
      const b=document.createElement("button");
      b.textContent=d;
      b.onclick=()=>{
        typeText("งั้นนัดกันแล้วนะ 💕");
        aBox.innerHTML="";
      };
      aBox.appendChild(b);
    });
  });
}

next();
</script>

</body>
</html>
