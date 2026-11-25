
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<link rel="manifest" href="manifest.json">
<meta name="theme-color" content="#07111f">
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Speed Libya</title>
<style>
*{box-sizing:border-box}
body{
margin:0;min-height:100vh;
background:linear-gradient(145deg,#06152d,#102e55);
color:white;font-family:Arial,sans-serif;
text-align:center;padding:20px;
}
main{width:100%;max-width:440px;margin:auto}
h1{font-size:34px;color:#48d9ff;margin-bottom:5px}
p{color:#b8d9f5}
.gauge{
width:min(85vw,330px);height:min(85vw,330px);
margin:35px auto 20px;border-radius:50%;
position:relative;
background:conic-gradient(from 225deg,#35d9ff 0deg,#1686e8 270deg,transparent 270deg);
display:grid;place-items:center;
}
.gauge:before{
content:"";position:absolute;width:82%;height:82%;
border-radius:50%;background:#0b2040;
box-shadow:inset 0 0 25px #0008;
}
.inner{z-index:1}
#speed{font-size:58px;font-weight:bold}
.unit{color:#55dfff;font-size:18px}
.needle{
position:absolute;width:4px;height:40%;
background:#fff;bottom:50%;left:calc(50% - 2px);
transform-origin:bottom;transform:rotate(-135deg);
border-radius:5px;z-index:2;
}
button{
background:#28c9f5;color:#06152d;border:0;
border-radius:15px;padding:16px 35px;
font-size:18px;font-weight:bold;cursor:pointer;margin:15px;
}
button:disabled{opacity:.5}
.stat{
background:#17375e;border:1px solid #2b5d88;
padding:15px;border-radius:12px;margin:10px;
}
.stat strong{display:block;font-size:20px;color:#55dfff;margin-top:8px}
#status{color:#b8d9f5;min-height:25px}
#pro{
max-width:440px;margin:45px auto 20px;
padding:22px;background:#102b27;
border-radius:20px;
}
.plan{
background:#19443d;padding:15px;
border-radius:14px;margin:15px 0;
}
.plan button{margin-bottom:5px}
</style>
</head>
<body>

<main>
<h1>Speed Libya</h1>
<p>اختبر سرعة الإنترنت عندك</p>

<div class="gauge">
<div class="needle" id="needle"></div>
<div class="inner">
<div id="speed">0</div>
<div class="unit">Mbps</div>
</div>
</div>

<div class="stat">
سرعة التحميل
<strong id="down">-- Mbps</strong>
</div>

<button id="start">ابدأ الاختبار</button>
<div id="status">جاهز للاختبار</div>
</main>

<section id="pro">
<h2>🚀 Speed Libya PRO</h2>
<p>مميزات إضافية للمشتركين</p>

<div class="plan">
<h3>Ping وLatency</h3>
<p>قياس سرعة الاستجابة بالمللي ثانية (MS)</p>
</div>

<div class="plan">
<h3>اشتراك يومي</h3>
<h2>1 دينار</h2>
<button onclick="selectPlan('يومي',1)">اختيار الباقة</button>
</div>

<div class="plan">
<h3>اشتراك أسبوعي</h3>
<h2>5 دنانير</h2>
<button onclick="selectPlan('أسبوعي',5)">اختيار الباقة</button>
</div>

<div class="plan">
<h3>اشتراك شهري</h3>
<h2>10 دنانير</h2>
<button onclick="selectPlan('شهري',10)">اختيار الباقة</button>
</div>

<p id="plan-message">النسخة المجانية متاحة للجميع ❤️</p>
</section>

<script>
const $=id=>document.getElementById(id);

function showSpeed(n){
n=Math.max(0,Math.min(300,n));
$("speed").textContent=n.toFixed(1);
$("needle").style.transform=`rotate(${-135+n/300*270}deg)`;
}

async function downloadTest(){
const bytes=25000000;
const start=performance.now();
let received=0;

const response=await fetch(
"https://speed.cloudflare.com/__down?bytes="+bytes,
{cache:"no-store"}
);

if(!response.ok)throw Error("Download failed");

const reader=response.body.getReader();

while(true){
const {done,value}=await reader.read();
if(done)break;
received+=value.length;

const seconds=(performance.now()-start)/1000;
if(seconds>0)showSpeed(received*8/seconds/1000000);
}

return received*8/((performance.now()-start)/1000)/1000000;
}

$("start").onclick=async()=>{
$("start").disabled=true;
$("status").textContent="جاري قياس سرعة الإنترنت...";

try{
const speed=await downloadTest();
$("down").textContent=speed.toFixed(1)+" Mbps";
showSpeed(speed);
$("status").textContent="اكتمل الاختبار بنجاح!";
}catch(e){
$("status").textContent="تعذر الاختبار، حاول مرة ثانية";
}finally{
$("start").disabled=false;
}
};

function selectPlan(plan,price){
$("plan-message").textContent=
"اخترت الباقة "+plan+" بسعر "+price+
" دينار. الدفع غير مربوط حالياً.";
}
</script>

</body>
<img src="file_00000000a3e48210be0fc32e205d6813.png"
style="width:100%;height:auto;display:block;">
</html>
