<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
body{margin:0;font-family:Arial,sans-serif;background:#f5f5f5;color:#333;}
header{padding:20px;text-align:center;background:#0077cc;color:#fff;font-size:24px;font-weight:bold;}
.wrap{max-width:600px;margin:20px auto;padding:10px;}
.card{background:#fff;padding:15px;margin-bottom:15px;border-radius:8px;box-shadow:0 0 5px rgba(0,0,0,0.1);}
input,select,button{width:100%;padding:10px;margin-top:5px;border-radius:5px;border:1px solid #ccc;font-size:14px;}
button{background:#0077cc;color:#fff;border:none;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:6px;}
button:hover{background:#005fa3;}
.nav{display:flex;justify-content:space-around;position:fixed;bottom:0;left:0;right:0;background:#eee;padding:10px;border-top:1px solid #ccc;}
.nav div{text-align:center;font-size:14px;cursor:pointer;display:flex;flex-direction:column;align-items:center;}
.plan{border:1px solid #ddd;padding:10px;margin-bottom:10px;border-radius:5px;display:flex;justify-content:space-between;align-items:center;}
.plan button{width:auto;padding:6px 12px;font-size:13px;}
.muted{color:#666;font-size:13px;}
.alert-note{background:#ffe0e0;color:#a00;padding:10px;margin-bottom:12px;border-radius:5px;text-align:center;}
.success-note{background:#e0ffe0;color:#0a0;padding:10px;margin-bottom:12px;border-radius:5px;text-align:center;}
</style>
</head>
<body>
<header><i class="fas fa-bolt"></i> VERBOSE</header>
<div class="wrap">

<!-- LOGIN -->
<div id="loginCard" class="card">
<h3><i class="fas fa-user"></i> Login / Signup</h3>
<p style="margin:0 0 8px 0;"><em>VERBOSE — Rock.Earn branch, serving <strong>1,000,000+ users</strong></em></p>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">New User</option>
</select>
<input id="inputUser" placeholder="Username">
<input id="inputPass" placeholder="Password" type="password">
<button onclick="doAuth()"><i class="fas fa-sign-in-alt"></i> Submit</button>
<p class="muted">Use same device/browser. Data stored locally. Refresh will NOT reset your data.</p>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card" style="display:none;">
<div class="alert-note"><i class="fas fa-exclamation-triangle"></i> All transactions are manually approved. Contact admin below.</div>
<div>
<p><strong>Welcome: </strong><span id="welcomeText"></span></p>
<p><strong>Member since: </strong><span id="memberSince"></span></p>
<p><strong>Balance: </strong>Rs <span id="balanceText">0</span></p>
<p><strong>Daily Earnings: </strong>Rs <span id="dailyText">0</span></p>
<button onclick="doLogout()"><i class="fas fa-sign-out-alt"></i> Logout</button>
</div>

<div class="card">
<h4><i class="fas fa-user-shield"></i> Administration Contact</h4>
<p><i class="fab fa-whatsapp"></i> WhatsApp: <strong>03705519562</strong></p>
<p><i class="fas fa-envelope"></i> Email: <strong>Rock.earn92@gmail.com</strong></p>
<p class="muted">Contact for deposit/withdraw or support issues.</p>
</div>
</div>

<!-- PLANS -->
<div id="plansCard" class="card" style="display:none;">
<h3><i class="fas fa-gift"></i> Available Plans</h3>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="depositCard" class="card" style="display:none;">
<h3><i class="fas fa-wallet"></i> Deposit</h3>
<label>Method</label>
<select id="depositMethod">
<option value="jazzcash">JazzCash — 03705519562</option>
<option value="easypaisa">EasyPaisa — 03379827882</option>
</select>
<label>Amount (PKR)</label>
<input id="depositAmount" placeholder="Enter amount">
<label>Transaction ID</label>
<input id="depositTx" placeholder="Enter TX ID">
<button onclick="submitDeposit()"><i class="fas fa-paper-plane"></i> Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawCard" class="card" style="display:none;">
<h3><i class="fas fa-money-bill-wave"></i> Withdrawal</h3>
<label>Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<label>Username</label>
<input id="withdrawUsername" readonly>
<label>Account / Mobile Number</label>
<input id="withdrawAccount" placeholder="Enter account/mobile">
<label>Amount (PKR)</label>
<input id="withdrawAmount" placeholder="Enter amount">
<button onclick="submitWithdraw()"><i class="fas fa-paper-plane"></i> Request Withdrawal</button>
</div>

</div>

<!-- NAV -->
<div class="nav">
<div onclick="nav('dashboardCard')"><i class="fas fa-home"></i> Home</div>
<div onclick="nav('plansCard')"><i class="fas fa-gift"></i> Plans</div>
<div onclick="nav('depositCard')"><i class="fas fa-wallet"></i> Deposit</div>
<div onclick="nav('withdrawCard')"><i class="fas fa-hand-holding-usd"></i> Withdraw</div>
<div onclick="nav('dashboardCard')"><i class="fas fa-headset"></i> Support</div>
</div>

<script>
// STORAGE KEYS
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
const KEY_DAILY='verbose_daily_';
const KEY_USER_PLANS='verbose_plans_';
const KEY_DEPOSITS='verbose_deposits';
const KEY_WITHDRAWS='verbose_withdraws';
let currentUser=localStorage.getItem(KEY_USER)||null;

// SAMPLE PLANS
let plans=[];
for(let i=1;i<=7;i++){
let invest=200*i;
let days=20+Math.floor(Math.random()*51);
plans.push({id:i,name:'Special Plan '+i,invest:invest,total:invest*3,days:days});
}
for(let i=8;i<=32;i++){
let invest=3000 + (i-8)*1000;
let days=20+Math.floor(Math.random()*51);
plans.push({id:i,name:'Plan '+(i-7),invest:invest,total:invest*2.5,days:days});
}

function fmt(n){return Number(n).toLocaleString();}

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
if(!localStorage.getItem(KEY_BAL+u)) localStorage.setItem(KEY_BAL+u,'0');
if(!localStorage.getItem(KEY_DAILY+u)) localStorage.setItem(KEY_DAILY+u,'0');
if(!localStorage.getItem(KEY_USER_PLANS+u)) localStorage.setItem(KEY_USER_PLANS+u,'[]');
}else{
if(localStorage.getItem(credKey)!==p){alert('Wrong username/password');return;}
}
localStorage.setItem(KEY_USER,u);
currentUser=u;
afterLoginUI();
}

function afterLoginUI(){nav('dashboardCard');renderDashboard();renderPlans();fillWithdrawUser();}

// NAV
function nav(cardId){
['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>document.getElementById(id).style.display='none');
document.getElementById(cardId).style.display='block';
}

// DASHBOARD
function renderDashboard(){
if(!currentUser) return;
document.getElementById('welcomeText').innerText=currentUser;
document.getElementById('memberSince').innerText=new Date().toLocaleDateString();
document.getElementById('balanceText').innerText=fmt(Number(localStorage.getItem(KEY_BAL+currentUser)||0));
document.getElementById('dailyText').innerText=fmt(Number(localStorage.getItem(KEY_DAILY+currentUser)||0));
}

function doLogout(){
localStorage.removeItem(KEY_USER);
currentUser=null;
nav('loginCard');
}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList');
container.innerHTML='';
plans.forEach(plan=>{
let div=document.createElement('div');
div.className='plan';
div.innerHTML=`<div>
<strong>${plan.name}</strong><br>
Invest: Rs ${fmt(plan.invest)} · Total: Rs ${fmt(plan.total)} · Days: ${plan.days}
</div>
<button onclick="buyPlan(${plan.id})"><i class="fas fa-cart-plus"></i> Buy</button>`;
container.appendChild(div);
});
}

function buyPlan(id){
if(!currentUser){alert('Login first');return;}
let plan=plans.find(p=>p.id===id);
if(!plan){alert('Plan not found');return;}
document.getElementById('depositAmount').value=plan.invest;
nav('depositCard');
alert(`Plan ${plan.name} selected. Submit deposit. Admin will approve.`);
}

// DEPOSIT
function submitDeposit(){
if(!currentUser){alert('Login first');return;}
const method=document.getElementById('depositMethod').value;
const amount=Number(document.getElementById('depositAmount').value);
const tx=document.getElementById('depositTx').value.trim();
if(!amount||!tx){alert('Enter amount & TX ID');return;}
let deposits=JSON.parse(localStorage.getItem(KEY_DEPOSITS)||'[]');
deposits.push({user:currentUser,method,amount,tx,time:Date.now(),approved:false});
localStorage.setItem(KEY_DEPOSITS,JSON.stringify(deposits));
alert('Deposit submitted! Admin will approve manually.');
document.getElementById('depositAmount').value='';
document.getElementById('depositTx').value='';
nav('dashboardCard');
}

// WITHDRAW
function fillWithdrawUser(){
if(!currentUser) return;
document.getElementById('withdrawUsername').value=currentUser;
}

function submitWithdraw(){
if(!currentUser){alert('Login first');return;}
const method=document.getElementById('withdrawMethod').value;
const account=document.getElementById('withdrawAccount').value.trim();
const amount=Number(document.getElementById('withdrawAmount').value);
if(!account||!amount){alert('Enter account & amount');return;}
let withdraws=JSON.parse(localStorage.getItem(KEY_WITHDRAWS)||'[]');
withdraws.push({user:currentUser,method,account,amount,time:Date.now(),approved:false});
localStorage.setItem(KEY_WITHDRAWS,JSON.stringify(withdraws));
alert('Withdrawal requested! Admin will approve manually.');
document.getElementById('withdrawAccount').value='';
document.getElementById('withdrawAmount').value='';
nav('dashboardCard');
}
</script>
</body>
</html>
