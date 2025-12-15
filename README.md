<NEXA>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXA EARN – Smart Investment</title>

<style>
:root{--neon:#00f7ff;--pink:#ff4fd8;--bg:#050505;}
*{box-sizing:border-box}
body{margin:0;font-family:Arial;background:var(--bg);color:#fff}
header{text-align:center;padding:22px;font-size:28px;font-weight:900;
background:linear-gradient(90deg,var(--neon),var(--pink));
-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.tag{text-align:center;font-size:13px;color:#aaa;margin-top:-10px}

.page{max-width:430px;margin:14px auto;padding:14px;
background:rgba(255,255,255,.03);border-radius:12px;border:1px solid rgba(0,255,255,.08)}
.hidden{display:none}

input,select,button{
width:100%;padding:10px;margin-top:10px;border-radius:8px;
background:transparent;color:#fff;border:1px solid rgba(0,255,255,.2)
}
button{background:linear-gradient(90deg,var(--neon),var(--pink));
color:#000;font-weight:800;border:none;cursor:pointer}

.plan{border:1px solid rgba(0,255,255,.15);padding:12px;border-radius:10px;margin-top:10px}
.countdown{color:var(--neon);font-weight:700;font-size:13px}

.nav{
position:fixed;bottom:0;left:0;right:0;
display:flex;justify-content:space-around;
background:#070707;padding:10px;border-top:1px solid rgba(0,255,255,.2)
}
.nav div{text-align:center;font-size:13px;cursor:pointer}
.nav span{display:block;font-size:18px}

.support{margin-top:10px;padding:10px;border-radius:10px;
background:rgba(0,255,255,.08);cursor:pointer}
</style>
</head>

<body>

<header>NEXA EARN</header>
<div class="tag">Smart Investment • Daily Profit • Trusted System</div>

<!-- LOGIN -->
<div id="login" class="page">
<h3>Login / Signup</h3>
<input id="username" placeholder="Username">
<input id="password" type="password" placeholder="Password">
<button onclick="login()">Continue</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<b>User:</b> <span id="uName"></span><br><br>
<b>Balance:</b> Rs <span id="balance"></span><br>
<b>Daily Profit:</b> Rs <span id="daily"></span><br><br>
<button onclick="show('plans')">Plans Dekhein</button>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
<h3>Investment Plans</h3>
<div id="planList"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="page hidden">
<h3>Deposit</h3>
<select id="method" onchange="setNumber()">
<option>JazzCash</option>
<option>EasyPaisa</option>
</select>
<input id="number" readonly>
<input id="amount" readonly>
<input placeholder="Transaction ID">
<button>Submit</button>
</div>

<!-- HISTORY -->
<div id="history" class="page hidden">
<h3>History</h3>
<div id="historyList"></div>

<div class="support" onclick="openSupport()">🛠️ Support</div>
</div>

<!-- NAV -->
<div id="nav" class="nav hidden">
<div onclick="show('dashboard')"><span>🏠</span>Home</div>
<div onclick="show('plans')"><span>📦</span>Plans</div>
<div onclick="show('history')"><span>📜</span>History</div>
</div>

<script>
// ===== LOGIN SAVE =====
let user = localStorage.getItem("nexa_user");
let balance = Number(localStorage.getItem("nexa_bal")||5000);
let daily = Number(localStorage.getItem("nexa_daily")||0);
let history = JSON.parse(localStorage.getItem("nexa_hist")||"[]");

if(user) start();

function login(){
user = document.getElementById("username").value;
localStorage.setItem("nexa_user",user);
start();
}

function start(){
document.getElementById("login").classList.add("hidden");
document.getElementById("nav").classList.remove("hidden");
show("dashboard");
update();
renderPlans();
}

// ===== UI =====
function show(id){
document.querySelectorAll(".page").forEach(p=>p.classList.add("hidden"));
document.getElementById(id).classList.remove("hidden");
}

function update(){
uName.innerText=user;
balanceEl.innerText=balance;
dailyEl.innerText=daily;
}

// ===== PLANS =====
const plans=[
{n:"Special 1",amt:200,days:25,m:2.6,s:true},
{n:"Special 2",amt:400,days:30,m:2.6,s:true},
{n:"Special 3",amt:800,days:35,m:2.6,s:true},
{n:"Special 4",amt:1200,days:40,m:2.6,s:true},
{n:"Special 5",amt:1800,days:45,m:2.6,s:true},
{n:"Plan 6",amt:3000,days:60,m:2.2},
{n:"Plan 7",amt:5000,days:70,m:2.2}
];

function renderPlans(){
planList.innerHTML="";
plans.forEach((p,i)=>{
let end = localStorage.getItem("timer_"+i) || Date.now()+86400000;
localStorage.setItem("timer_"+i,end);

let div=document.createElement("div");
div.className="plan";
div.innerHTML=`
<b>${p.n}</b><br>
Invest: Rs ${p.amt}<br>
Total: Rs ${Math.round(p.amt*p.m)}<br>
Daily: Rs ${Math.round((p.amt*p.m)/p.days)}<br>
Days: ${p.days}
${p.s?`<div class="countdown" id="cd${i}"></div>`:""}
<button onclick="buy(${i})">Buy Now</button>`;
planList.appendChild(div);

if(p.s) countdown(i,end);
});
}

function countdown(i,end){
setInterval(()=>{
let t=end-Date.now();
if(t<0)return;
let h=Math.floor(t/3600000);
let m=Math.floor((t%3600000)/60000);
let s=Math.floor((t%60000)/1000);
document.getElementById("cd"+i).innerText=`Offer Ends: ${h}h ${m}m ${s}s`;
},1000);
}

// ===== BUY =====
function buy(i){
let p=plans[i];
document.getElementById("amount").value=p.amt;
setNumber();
show("deposit");
history.push(`Plan Buy: ${p.n} - Rs ${p.amt}`);
localStorage.setItem("nexa_hist",JSON.stringify(history));
renderHistory();
}

// ===== DEPOSIT =====
function setNumber(){
number.value = method.value==="JazzCash"?"03705519562":"03379827882";
}

// ===== HISTORY =====
function renderHistory(){
historyList.innerHTML="";
history.forEach(h=>{
let d=document.createElement("div");
d.className="plan";
d.innerText=h;
historyList.appendChild(d);
});
}
renderHistory();

// ===== SUPPORT =====
function openSupport(){
window.open("https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu","_blank");
}
</script>

</body>
</html>
