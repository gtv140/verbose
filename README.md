<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
:root {
  --bg: #0d1117;
  --card-bg: #161b22;
  --primary: #1e6fff;
  --text: #c9d1d9;
  --muted: #8b949e;
  --success: #2ea44f;
  --alert: #ff7b72;
}
body {margin:0; font-family:Arial,sans-serif; background:var(--bg); color:var(--text);}
header {padding:20px; text-align:center; font-size:28px; font-weight:900; color:var(--primary);}
.card {background:var(--card-bg); padding:15px; border-radius:12px; margin:12px; border:1px solid rgba(255,255,255,0.05);}
input, select, button {width:100%; padding:10px; margin:5px 0; border-radius:8px; border:1px solid rgba(255,255,255,0.1); background:transparent; color:var(--text);}
button {cursor:pointer; background:var(--primary); color:#fff; font-weight:600;}
.nav {position:fixed; bottom:0; left:0; right:0; display:flex; justify-content:space-around; padding:10px; background:var(--card-bg); border-top:1px solid rgba(255,255,255,0.1);}
.nav div {color:var(--text); text-align:center; cursor:pointer;}
.hidden{display:none;}
.alert-box {background:rgba(255,123,114,0.1); padding:10px; border-radius:8px; color:var(--alert); text-align:center; margin-bottom:10px;}
.plan {display:flex; justify-content:space-between; padding:10px; border-radius:10px; border:1px solid rgba(255,255,255,0.05); margin-bottom:8px; background:rgba(255,255,255,0.02);}
.plan .meta {flex:1;}
.plan .actions {text-align:right; width:110px;}
.referral-box {display:flex; margin-bottom:12px;}
.referral-box input {flex:1;}
.logout-btn {position:fixed; bottom:60px; left:50%; transform:translateX(-50%); padding:8px 16px; border:none; border-radius:10px; background:#ff4b4b; color:#fff; cursor:pointer; font-weight:600;}
.icon-grid {display:grid; grid-template-columns:repeat(4,1fr); gap:12px; margin-bottom:12px;}
.icon-grid div {text-align:center; padding:12px; border-radius:10px; background:rgba(255,255,255,0.05); cursor:pointer;}
.icon-grid div i {font-size:24px; color:var(--primary); margin-bottom:6px; display:block;}
</style>
</head>
<body>

<header><i class="fas fa-coins"></i> VERBOSE</header>

<div class="card" id="loginCard">
<h3>Login / Signup</h3>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">New User</option>
</select>
<input id="inputUser" placeholder="Username">
<input id="inputPass" placeholder="Password" type="password">
<input id="referralInput" placeholder="Referral Code (optional)">
<button onclick="doAuth()"><i class="fas fa-sign-in-alt"></i> Submit</button>
<p style="color:var(--muted); font-size:13px;">Use same device. Data stored locally.</p>
</div>

<div class="card hidden" id="dashboardCard">
<div class="alert-box">⚠️ Deposit or Withdrawal issues? Contact Administration immediately.</div>
<div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:12px;">
<div>
<div id="welcomeText" style="font-weight:800; color:var(--primary);"></div>
<div id="memberSince" style="color:var(--muted); font-size:13px;"></div>
</div>
<div style="text-align:right;">
<div style="color:var(--muted); font-size:13px;">Balance</div>
<div style="font-weight:900; font-size:18px;"><i class="fas fa-coins"></i> Rs <span id="balanceText">0</span></div>
</div>
</div>

<div class="referral-box">
<input id="referralLink" readonly>
<button onclick="copyReferral()"><i class="fas fa-copy"></i> Copy</button>
</div>

<div class="icon-grid">
<div><i class="fas fa-gift"></i> Offers</div>
<div><i class="fas fa-wallet"></i> Deposit</div>
<div><i class="fas fa-hand-holding-usd"></i> Withdraw</div>
<div><i class="fas fa-user"></i> Profile</div>
<div><i class="fas fa-chart-line"></i> Stats</div>
<div><i class="fas fa-cogs"></i> Settings</div>
<div><i class="fas fa-bell"></i> Alerts</div>
<div><i class="fas fa-question-circle"></i> Support</div>
</div>

<div id="plansCard">
<h3>Special & Normal Plans</h3>
<div id="plansList"></div>
</div>

<div id="depositCard" class="hidden">
<h3>Deposit</h3>
<label>Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<label>Number</label>
<input id="depositNumber" readonly>
<label>Amount</label>
<input id="depositAmount" readonly>
<label>Transaction ID</label>
<input id="depositTx" placeholder="Enter TX ID">
<label>Upload Proof</label>
<input type="file" id="depositProof">
<button onclick="submitDeposit()"><i class="fas fa-paper-plane"></i> Submit Deposit</button>
</div>

<div id="withdrawCard" class="hidden">
<h3>Withdrawal</h3>
<label>Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<label>Username</label>
<input id="withdrawUsername" readonly>
<label>Account / Mobile Number</label>
<input id="withdrawAccount" placeholder="Enter account or mobile number">
<label>Amount</label>
<input id="withdrawAmount" placeholder="Enter amount">
<button onclick="submitWithdraw()"><i class="fas fa-paper-plane"></i> Request Withdrawal</button>
</div>

<button class="logout-btn hidden" id="logoutBtn" onclick="doLogout()"><i class="fas fa-sign-out-alt"></i> Logout</button>

<div class="nav">
<div onclick="nav('dashboardCard')"><i class="fas fa-home"></i> Home</div>
<div onclick="nav('plansCard')"><i class="fas fa-box"></i> Plans</div>
<div onclick="nav('depositCard')"><i class="fas fa-wallet"></i> Deposit</div>
<div onclick="nav('withdrawCard')"><i class="fas fa-hand-holding-usd"></i> Withdraw</div>
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
let plans=[];

// Add plans
for(let i=1;i<=7;i++){plans.push({id:i,name:'Special Plan '+i,invest:200*i,multiplier:3,total:200*i*3,days:Math.floor(Math.random()*51)+20,offer:true});}
for(let i=8;i<=30;i++){let invest=3000+(i-8)*1000;plans.push({id:i,name:'Plan '+(i-7),invest:invest,multiplier:2.5,total:invest*2.5,days:Math.floor(Math.random()*51)+20,offer:false});}
for(let i=31;i<=35;i++){plans.push({id:i,name:'Coming Soon',invest:0,multiplier:0,total:0,days:0,offer:false});}

function fmt(n){return Number(n).toLocaleString();}

// AUTH
function doAuth(){
const mode=document.getElementById('authMode').value;
const u=(document.getElementById('inputUser').value||'').trim();
const p=(document.getElementById('inputPass').value||'').trim();
const ref=document.getElementById('referralInput').value.trim();
if(!u||!p){alert('Enter username & password');return;}
const credKey='verbose_cred_'+u;
if(mode==='signup'){
if(localStorage.getItem(credKey)){alert('Username exists');return;}
localStorage.setItem(credKey,p);localStorage.setItem(KEY_BAL+u,'0');localStorage.setItem(KEY_DAILY+u,'0');localStorage.setItem(KEY_USER_PLANS+u,'[]');
if(ref && localStorage.getItem('verbose_cred_'+ref)){let bal=Number(localStorage.getItem(KEY_BAL+ref)||0);bal+=30;localStorage.setItem(KEY_BAL+ref,bal);}
}else{if(localStorage.getItem(credKey)!==p){alert('Wrong username/password');return;}}
localStorage.setItem(KEY_USER,u);currentUser=u;afterLoginUI();
}

function afterLoginUI(){nav('dashboardCard');renderPlans();updateReferralLink();document.getElementById('logoutBtn').classList.remove('hidden'); updateDashboard();}
function updateReferralLink(){if(!currentUser) return;document.getElementById('referralLink').value=window.location.href+'?ref='+currentUser;}
function copyReferral(){const link=document.getElementById('referralLink');link.select();document.execCommand('copy');alert('Referral link copied!');}
function doLogout(){localStorage.removeItem(KEY_USER);currentUser=null;nav('loginCard');document.getElementById('logoutBtn').classList.add('hidden');}

// NAV
function nav(cardId){['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>document.getElementById(id).classList.add('hidden'));document.getElementById(cardId).classList.remove('hidden');updateDashboard(); fillWithdrawUser();}

// DASHBOARD
function updateDashboard(){if(!currentUser) return;
document.getElementById('welcomeText').innerText='Welcome, '+currentUser;
document.getElementById('memberSince').innerText='Member since: '+new Date().toLocaleDateString();
document.getElementById('balanceText').innerText=fmt(Number(localStorage.getItem(KEY_BAL+currentUser)||0));
}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList');container.innerHTML='';
plans.forEach(plan=>{
const div=document.createElement('div');div.className='plan';
let dailyProfit=plan.days>0?Math.round(plan.total/plan.days):0;
div.innerHTML=`<div class="meta"><strong>${plan.name}</strong><br>Invest: Rs ${fmt(plan.invest)} · Total: Rs ${fmt(plan.total)} · Days: ${plan.days} · Daily: Rs ${fmt(dailyProfit)}</div>
<div class="actions">${plan.name!=='Coming Soon'?'<button onclick="buyPlan('+plan.id+')">Buy Now</button>':''}</div>`;
container.appendChild(div);
});
}

// BUY PLAN
function buyPlan(id){if(!currentUser){alert('Login first');return;}
const plan=plans.find(p=>p.id===id);if(!plan || plan.name==='Coming Soon'){alert('Plan not available');return;}
let userPlans=JSON.parse(localStorage.getItem(KEY_USER_PLANS+currentUser)||'[]');
userPlans.push({planId:plan.id,dailyProfit:Math.round(plan.total/plan.days),lastCredit:Date.now()});
localStorage.setItem(KEY_USER_PLANS+currentUser,JSON.stringify(userPlans));
document.getElementById('depositAmount').value=plan.invest;updateDepositNumber();nav('depositCard');alert(`Plan ${plan.name} selected. Submit your deposit.`);}

// DEPOSIT
function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';}
function submitDeposit(){if(!currentUser){alert('Login first');return;}
const tx=(document.getElementById('depositTx').value||'').trim();const proof=document.getElementById('depositProof').files[0];const amount=Number(document.getElementById('depositAmount').value)||0;
if(!tx||!proof||!amount){alert('All fields required');return;}
const deposits=JSON.parse(localStorage.getItem(KEY_DEPOSITS)||'[]');
deposits.push({user:currentUser,method:document.getElementById('depositMethod').value,amount,tx,proof:proof.name,time:Date.now(),approved:false});
localStorage.setItem(KEY_DEPOSITS,JSON.stringify(deposits));
alert('Deposit submitted! Admin will approve manually.');document.getElementById('depositTx').value='';document.getElementById('depositProof').value='';updateDashboard(); nav('dashboardCard');}

// WITHDRAW
function fillWithdrawUser(){if(!currentUser) return;document.getElementById('withdrawUsername').value=currentUser;}
function submitWithdraw(){if(!currentUser){alert('Login first');return;}
const method=document.getElementById('withdrawMethod').value;
const account=document.getElementById('withdrawAccount').value.trim();
const amount=Number(document.getElementById('withdrawAmount').value);
if(!account||!amount){alert('Enter account and amount');return;}
let balance=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
if(amount>balance){alert('Insufficient balance'); return;}
balance-=amount;localStorage.setItem(KEY_BAL+currentUser,balance);alert('Withdrawal requested!'); updateDashboard(); nav('dashboardCard');}

// INITIALIZE
if(currentUser){afterLoginUI();}
</script>

</body>
</html>
