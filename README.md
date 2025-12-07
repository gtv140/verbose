<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
body {
    margin:0;
    font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background:#0f172a;
    color:#fff;
}
header {
    padding:20px; 
    text-align:center; 
    font-weight:700; 
    font-size:24px; 
    background:#111827; 
    color:#38bdf8;
    box-shadow:0 4px 12px rgba(0,0,0,0.5);
}
.wrap {max-width:480px; margin:12px auto; padding:12px;}
.card {
    background:linear-gradient(135deg,#111827,#1e293b);
    padding:15px; 
    border-radius:15px; 
    margin-bottom:15px; 
    box-shadow:0 8px 20px rgba(0,0,0,0.5);
    transition:transform 0.3s, background 0.3s;
}
.card:hover {transform: translateY(-4px); background:linear-gradient(135deg,#1e293b,#0f172a);}
input, select, button {
    width:100%; 
    padding:10px; 
    margin-top:8px; 
    border-radius:10px; 
    border:none; 
    font-size:14px;
}
input, select {background:#1e293b; color:#fff;}
button {background:#38bdf8; color:#111827; font-weight:700; cursor:pointer; border:none; transition: all 0.3s;}
button:hover {background:#0ea5e9; color:#fff; transform:scale(1.05);}
.nav {
    position:fixed; bottom:0; left:0; right:0; 
    display:flex; justify-content:space-around; 
    padding:12px; background:#111827; border-top:1px solid #1e293b;
    box-shadow:0 -4px 12px rgba(0,0,0,0.5);
}
.nav div {text-align:center; font-size:14px; cursor:pointer; color:#38bdf8;}
.plan {
    border:1px solid #38bdf8; border-radius:12px; padding:12px; 
    margin-bottom:10px; display:flex; justify-content:space-between; align-items:center;
    background:#111827; transition:all 0.3s; position:relative;
    box-shadow:0 6px 16px rgba(0,0,0,0.4);
}
.plan:hover {background:#0f172a; transform:scale(1.03);}
.plan-badge {
    position:absolute; top:-10px; right:-10px; background:#0ea5e9; padding:5px 8px; border-radius:50%; font-size:12px; font-weight:700;
    animation:badgePulse 1.5s infinite;
    box-shadow:0 2px 8px rgba(0,0,0,0.5);
}
@keyframes badgePulse {0%,100%{transform:scale(1);}50%{transform:scale(1.3);}}
.progress-container {background:#1e293b; border-radius:10px; height:12px; width:100%; margin-top:6px;}
.progress-bar {background:#38bdf8; height:100%; border-radius:10px; width:0%; transition:width 1s;}
.alert-note {background:#1e293b; color:#38bdf8; padding:12px; border-radius:12px; margin-bottom:14px; font-weight:600; text-align:center;}
.user-box {display:flex; justify-content:space-between; align-items:center; margin-bottom:12px;}
.hidden {display:none;}
.balance-box {
    background: linear-gradient(90deg,#0f172a,#1e293b);
    color:#38bdf8; padding:12px; border-radius:12px; text-align:center;
    margin-bottom:15px; box-shadow:0 4px 12px rgba(0,0,0,0.5);
}
.balance-box span {display:block; font-size:22px; font-weight:700;}
.buyBtn {background:#facc15; color:#111827; font-weight:700; border-radius:12px; padding:8px 12px;}
.buyBtn:hover {background:#eab308; transform:scale(1.05);}
.logoutBtn {
    position:fixed; bottom:60px; left:50%; transform:translateX(-50%);
    background:#f87171; color:#111827; padding:12px 20px; border-radius:12px;
    font-weight:700; border:none; cursor:pointer; box-shadow:0 6px 12px rgba(0,0,0,0.5);
    transition:all 0.3s;
}
.logoutBtn:hover {background:#ef4444; transform:scale(1.05);}
</style>
</head>
<body>
<header><i class="fas fa-bolt"></i> VERBOSE <i class="fas fa-bolt"></i></header>
<div class="wrap">

<!-- LOGIN / SIGNUP -->
<div id="loginCard" class="card">
<h3>Login / Signup</h3>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">New User</option>
</select>
<input id="inputUser" placeholder="Username"/>
<input id="inputPass" placeholder="Password" type="password"/>
<button onclick="doAuth()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<div class="alert-note">⚠️ Deposit/Withdrawal issues? Contact VERBOSE Admin:<br>Whatsapp: <a href="https://wa.me/03705519562">03705519562</a> | Email: <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></div>

<div class="balance-box">
Welcome, <span id="welcomeText">User</span><br>
Balance: <span id="balanceText">0</span> PKR
</div>

<div id="progressGraphs"></div>
</div>

<!-- PLANS -->
<div id="plansCard" class="card hidden">
<h3>Plans</h3>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="depositCard" class="card hidden">
<h3>Deposit</h3>
<label>Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<label>Number</label>
<input id="depositNumber" readonly/>
<label>Amount (PKR)</label>
<input id="depositAmount" readonly/>
<label>Transaction ID</label>
<input id="depositTx" placeholder="Enter TX ID"/>
<label>Upload Proof</label>
<input id="depositProof" type="file"/>
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawCard" class="card hidden">
<h3>Withdraw</h3>
<label>Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<label>Account / Mobile Number</label>
<input id="withdrawAccount" placeholder="Enter account or mobile number"/>
<label>Amount (PKR)</label>
<input id="withdrawAmount" placeholder="Enter amount"/>
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<!-- LOGOUT BUTTON -->
<button class="logoutBtn" onclick="logoutUser()">Logout</button>

<!-- NAV -->
<div class="nav">
<div onclick="nav('dashboardCard')"><i class="fas fa-home"></i><br>Home</div>
<div onclick="nav('plansCard')"><i class="fas fa-gift"></i><br>Plans</div>
<div onclick="nav('depositCard')"><i class="fas fa-wallet"></i><br>Deposit</div>
<div onclick="nav('withdrawCard')"><i class="fas fa-money-bill"></i><br>Withdraw</div>
</div>

<script>
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
let currentUser=localStorage.getItem(KEY_USER)||null;

// SAMPLE PLANS
let plans=[];
for(let i=1;i<=7;i++){
let invest=200*i; if(invest>3000) invest=3000; let days=20+Math.floor(Math.random()*51);
plans.push({id:i,name:'Special Offer '+i,invest:invest,total:invest*3,daily:Math.round(invest*3/days),days:days,offer:true});
}
for(let i=8;i<=30;i++){
let invest=3000+(i-8)*(30000-3000)/22; let days=20+Math.floor(Math.random()*51);
plans.push({id:i,name:'Plan '+i,invest:Math.round(invest),total:Math.round(invest*2.5),daily:Math.round((invest*2.5)/days),days:days,offer:false});
}

// AUTH
function doAuth(){
const u=document.getElementById('inputUser').value.trim();
const p=document.getElementById('inputPass').value.trim();
if(!u||!p){alert('Enter username & password'); return;}
const key='verbose_cred_'+u;
if(document.getElementById('authMode').value==='signup'){
if(localStorage.getItem(key)){alert('Username exists'); return;}
localStorage.setItem(key,p);
if(!localStorage.getItem(KEY_BAL+u)) localStorage.setItem(KEY_BAL+u,'0');
}else{
if(localStorage.getItem(key)!==p){alert('Wrong username/password'); return;}
}
localStorage.setItem(KEY_USER,u);
currentUser=u;
afterLoginUI();
}

// NAV
function nav(cardId){
['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>document.getElementById(id).classList.add('hidden'));
document.getElementById(cardId).classList.remove('hidden');
renderDashboard();
renderPlans();
updateDepositNumber();
}

// DASHBOARD
function afterLoginUI(){nav('dashboardCard');}
function renderDashboard(){
if(!currentUser) return;
document.getElementById('welcomeText').innerText=currentUser;
document.getElementById('balanceText').innerText=localStorage.getItem(KEY_BAL+currentUser)||'0';
renderProgressGraphs();
}

// LOGOUT
function logoutUser(){
localStorage.removeItem(KEY_USER);
currentUser=null;
nav('loginCard');
alert('Logged out successfully!');
}

// PROGRESS GRAPHS
function renderProgressGraphs(){
const container=document.getElementById('progressGraphs'); container.innerHTML='';
plans.slice(0,5).forEach(plan=>{
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
let percent=Math.min(100,Math.round((bal/plan.total)*100));
const div=document.createElement('div'); div.className='card';
div.innerHTML=`<strong>${plan.name}</strong><br>Daily: Rs ${plan.daily} | Total: Rs ${plan.total}<div class="progress-container"><div class="progress-bar" style="width:${percent}%"></div></div>`;
container.appendChild(div);
});
}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList'); container.innerHTML='';
plans.forEach(plan=>{
const div=document.createElement('div'); div.className='plan';
div.innerHTML=`<div><strong>${plan.name}</strong><br>Invest: Rs ${plan.invest}<br>Total: Rs ${plan.total}<br>Daily: Rs ${plan.daily}<br>Days: ${plan.days}${plan.offer?`<br><span id="timer_${plan.id}">Loading timer...</span>`:''}</div>
<div><button onclick="buyPlan(${plan.id})" class="buyBtn">Buy Now</button></div>`;
if(plan.offer){
const badge=document.createElement('div'); badge.className='plan-badge'; badge.innerText='HOT'; div.appendChild(badge);
startCountdown(plan.id);
}
container.appendChild(div);
});
}

// COUNTDOWN
function startCountdown(id){
const el=document.getElementById('timer_'+id);
let end=Date.now()+24*3600*1000;
setInterval(()=>{let diff=Math.floor((end-Date.now())/1000); if(diff<=0){el.innerText='Offer ended'; return;}
let h=Math.floor(diff/3600), m=Math.floor((diff%3600)/60), s=diff%60;
el.innerText=`${h}h:${m}m:${s}s`;},1000);
}

// BUY PLAN
function buyPlan(id){
const plan=plans.find(p=>p.id===id);
document.getElementById('depositAmount').value=plan.invest;
nav('depositCard');
alert(`Plan ${plan.name} selected. Submit your deposit.`);
}

// DEPOSIT
function updateDepositNumber(){
const method=document.getElementById('depositMethod').value;
document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function submitDeposit(){
if(!currentUser){alert('Login first'); return;}
const amount=Number(document.getElementById('depositAmount').value);
let prev=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
let target=prev+amount;
let current=prev;
let interval=setInterval(()=>{
current+=Math.ceil((target-current)/10);
if(current>=target){current=target; clearInterval(interval);}
document.getElementById('balanceText').innerText=current;
},50);
alert('Deposit submitted! Balance updated.');
nav('dashboardCard');
}

// WITHDRAW
function submitWithdraw(){
if(!currentUser){alert('Login first'); return;}
const amt=Number(document.getElementById('withdrawAmount').value);
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
if(amt>bal){alert('Insufficient balance'); return;}
bal-=amt;
localStorage.setItem(KEY_BAL+currentUser,bal);
alert('Withdrawal requested!');
nav('dashboardCard');
}

// KEEP USER LOGGED IN AFTER REFRESH
window.addEventListener('load', ()=>{
if(currentUser) afterLoginUI();
});
</script>
</body>
</html>
