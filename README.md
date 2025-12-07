<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
body{margin:0;font-family:Arial,sans-serif;background:#f4f4f4;color:#111;}
header{padding:20px;text-align:center;font-weight:900;font-size:26px;color:#333;border-bottom:1px solid #ccc;}
.wrap{max-width:600px;margin:20px auto;padding:10px;}
.card{background:#fff;padding:15px;border-radius:10px;margin-bottom:15px;box-shadow:0 2px 6px rgba(0,0,0,0.1);}
input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:6px;border:1px solid #ccc;font-size:14px;}
button{background:#007BFF;color:#fff;border:none;cursor:pointer;}
button:hover{background:#0056b3;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:#eee;border-top:1px solid #ccc;}
.nav div{cursor:pointer;text-align:center;font-size:13px;color:#333;}
.hidden{display:none;}
.plan{display:flex;justify-content:space-between;align-items:center;padding:8px;border:1px solid #ddd;border-radius:6px;margin-bottom:8px;}
.plan .info{flex:1;}
.plan .actions button{padding:6px 10px;font-size:13px;}
.countdown{font-size:12px;color:#555;margin-top:4px;}
.logout-btn{margin-top:10px;background:#dc3545;}
.muted{color:#666;font-size:13px;}
</style>
</head>
<body>

<header>VERBOSE - Rock Earn Branch</header>
<div class="wrap">

<!-- LOGIN -->
<div id="loginCard" class="card">
<h3>Login / Signup</h3>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">Signup</option>
</select>
<input id="inputUser" placeholder="Username"/>
<input id="inputPass" type="password" placeholder="Password"/>
<button onclick="doAuth()">Submit</button>
<p class="muted">Tip: Use same browser/device. Data stored locally.</p>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<h3>Dashboard</h3>
<p id="welcomeText"></p>
<p id="memberSince" class="muted"></p>
<p>Balance: Rs <span id="balanceText">0</span></p>
<p>Daily Profit: Rs <span id="dailyText">0</span></p>
<button class="logout-btn" onclick="doLogout()">Logout</button>
</div>

<!-- PLANS -->
<div id="plansCard" class="card hidden">
<h3>Plans</h3>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="depositCard" class="card hidden">
<h3>Deposit</h3>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash — 03705519562</option>
<option value="easypaisa">EasyPaisa — 03379827882</option>
</select>
<input id="depositNumber" readonly/>
<input id="depositAmount" placeholder="Amount"/>
<input id="depositTx" placeholder="Transaction ID"/>
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawCard" class="card hidden">
<h3>Withdraw</h3>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<input id="withdrawUsername" readonly/>
<input id="withdrawAccount" placeholder="Account / Mobile Number"/>
<input id="withdrawAmount" placeholder="Amount"/>
<button onclick="submitWithdraw()">Request Withdrawal</button>
<p class="muted">⚠️ Withdrawals manually approved. Contact support if urgent.</p>
</div>

<!-- NAV -->
<div class="nav">
<div onclick="nav('dashboardCard')"><i class="fas fa-home"></i><br>Home</div>
<div onclick="nav('plansCard')"><i class="fas fa-box"></i><br>Plans</div>
<div onclick="nav('depositCard')"><i class="fas fa-wallet"></i><br>Deposit</div>
<div onclick="nav('withdrawCard')"><i class="fas fa-hand-holding-usd"></i><br>Withdraw</div>
<div onclick="openSupport()"><i class="fas fa-headset"></i><br>Support</div>
</div>

<script>
// Storage Keys
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
const KEY_DAILY='verbose_daily_';
const KEY_USER_PLANS='verbose_plans_';
const KEY_DEPOSITS='verbose_deposits';
const KEY_WITHDRAWS='verbose_withdraws';
const KEY_OFFERS='verbose_offer_';
let currentUser=localStorage.getItem(KEY_USER)||null;
let plans=[],offerIntervals={};

// Initialize Plans
for(let i=1;i<=7;i++){
let invest=200*i;if(invest>3000) invest=3000;
let days=20+Math.floor(Math.random()*51);
plans.push({id:i,name:'Special Plan '+i,invest:invest,total:invest*3,days:days,special:true});
}
for(let i=8;i<=32;i++){
let invest=3000+Math.floor((i-8)*(30000-3000)/24);
let days=20+Math.floor(Math.random()*51);
plans.push({id:i,name:'Plan '+(i-7),invest:invest,total:Math.round(invest*2.5),days:days,special:false});
}
for(let i=33;i<=37;i++){
plans.push({id:i,name:'Coming Soon',invest:0,total:0,days:0,special:false});
}

function fmt(n){return Number(n).toLocaleString('en-US');}

// AUTH
function doAuth(){
const mode=document.getElementById('authMode').value;
const u=document.getElementById('inputUser').value.trim();
const p=document.getElementById('inputPass').value.trim();
if(!u||!p){alert('Enter username & password');return;}
const credKey='verbose_cred_'+u;
if(mode==='signup'){
if(localStorage.getItem(credKey)){alert('Username exists');return;}
localStorage.setItem(credKey,p);
localStorage.setItem(KEY_BAL+u,'0');
localStorage.setItem(KEY_DAILY+u,'0');
localStorage.setItem(KEY_USER_PLANS+u,'[]');
}else{
if(localStorage.getItem(credKey)!==p){alert('Wrong username/password');return;}
}
localStorage.setItem(KEY_USER,u);
currentUser=u;
afterLogin();
}

function afterLogin(){
nav('dashboardCard');
renderDashboard();
renderPlans();
startOffers();
}

function doLogout(){
currentUser=null;
localStorage.removeItem(KEY_USER);
nav('loginCard');
}

function nav(cardId){
['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>document.getElementById(id).classList.add('hidden'));
document.getElementById(cardId).classList.remove('hidden');
if(cardId==='dashboardCard') renderDashboard();
}

// DASHBOARD
function renderDashboard(){
if(!currentUser) return;
document.getElementById('welcomeText').innerText='Welcome, '+currentUser;
document.getElementById('memberSince').innerText='Member since: '+new Date().toLocaleDateString();
document.getElementById('balanceText').innerText=fmt(Number(localStorage.getItem(KEY_BAL+currentUser)||0));
document.getElementById('dailyText').innerText=fmt(Number(localStorage.getItem(KEY_DAILY+currentUser)||0));
document.getElementById('withdrawUsername').value=currentUser;
}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList');
container.innerHTML='';
plans.forEach(plan=>{
let daily=Math.round(plan.total/plan.days)||0;
let div=document.createElement('div');
div.className='plan';
div.innerHTML=`<div class="info">
<strong>${plan.name}</strong><br>
Invest: Rs ${fmt(plan.invest)} · Total: Rs ${fmt(plan.total)} · Days: ${plan.days} · Daily: Rs ${fmt(daily)}
${plan.special?`<div class="countdown" id="countdown_${plan.id}"></div>`:''}
</div>
<div class="actions">${plan.name!=='Coming Soon'?'<button onclick="buyPlan('+plan.id+')">Buy</button>':'Coming Soon'}</div>`;
container.appendChild(div);
});
}

// Special Offer Countdown
function startOffers(){
plans.forEach(plan=>{
if(!plan.special) return;
const key=KEY_OFFERS+plan.id;
let endTs=Number(localStorage.getItem(key)||0);
if(!endTs||endTs<Date.now()){
endTs=Date.now()+24*3600*1000;
localStorage.setItem(key,endTs);
}
const el=document.getElementById('countdown_'+plan.id);
if(!el) return;
function tick(){
const diff=Math.floor((endTs-Date.now())/1000);
if(diff<=0){el.innerText='Offer ended';clearInterval(offerIntervals[plan.id]);return;}
const h=Math.floor(diff/3600), m=Math.floor((diff%3600)/60), s=diff%60;
el.innerText=`Time Left: ${String(h).padStart(2,'0')}h:${String(m).padStart(2,'0')}m:${String(s).padStart(2,'0')}s`;
}
tick();
offerIntervals[plan.id]=setInterval(tick,1000);
});
}

// BUY PLAN
function buyPlan(id){
if(!currentUser){alert('Login first');return;}
const plan=plans.find(p=>p.id===id);
if(!plan){alert('Plan not available');return;}
let userPlans=JSON.parse(localStorage.getItem(KEY_USER_PLANS+currentUser)||'[]');
userPlans.push({planId:plan.id,dailyProfit:Math.round(plan.total/plan.days),lastCredit:Date.now()});
localStorage.setItem(KEY_USER_PLANS+currentUser,JSON.stringify(userPlans));
document.getElementById('depositAmount').value=plan.invest;
nav('depositCard');
alert('Plan selected. Submit deposit. Admin will approve manually.');
}

// DEPOSIT
function updateDepositNumber(){
const method=document.getElementById('depositMethod').value;
document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}

function submitDeposit(){
if(!currentUser){alert('Login first');return;}
const amount=Number(document.getElementById('depositAmount').value);
const tx=document.getElementById('depositTx').value.trim();
if(!tx||!amount){alert('Enter TX ID & Amount');return;}
let deposits=JSON.parse(localStorage.getItem(KEY_DEPOSITS)||'[]');
deposits.push({user:currentUser,method:document.getElementById('depositMethod').value,amount,tx,time:Date.now(),approved:false});
localStorage.setItem(KEY_DEPOSITS,JSON.stringify(deposits));
alert('Deposit submitted! Admin will approve manually.');
document.getElementById('depositTx').value='';
document.getElementById('depositAmount').value='';
nav('dashboardCard');
}

// WITHDRAW
function submitWithdraw(){
if(!currentUser){alert('Login first');return;}
const account=document.getElementById('withdrawAccount').value.trim();
const amount=Number(document.getElementById('withdrawAmount').value);
if(!account||!amount){alert('Enter account & amount');return;}
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
if(amount>bal){alert('Insufficient balance');return;}
bal-=amount;
localStorage.setItem(KEY_BAL+currentUser,bal);
let withdraws=JSON.parse(localStorage.getItem(KEY_WITHDRAWS)||'[]');
withdraws.push({user:currentUser,method:document.getElementById('withdrawMethod').value,account,amount,time:Date.now(),approved:false});
localStorage.setItem(KEY_WITHDRAWS,JSON.stringify(withdraws));
alert('Withdrawal requested! Admin will approve manually.');
document.getElementById('withdrawAmount').value='';
document.getElementById('withdrawAccount').value='';
renderDashboard();
}

// SUPPORT
function openSupport(){
alert('Contact Admin:\nWhatsApp: 03705519562\nEmail: Rock.earn92@gmail.com');
}
</script>
</body>
</html>
