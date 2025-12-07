<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
body {margin:0; font-family:Arial,sans-serif; background:#f5f5f5; color:#333;}
header {padding:20px; text-align:center; font-weight:700; font-size:24px; background:#0066cc; color:#fff;}
.wrap {max-width:480px; margin:12px auto; padding:12px;}
.card {background:#fff; padding:12px; border-radius:10px; margin-bottom:12px; box-shadow:0 2px 6px rgba(0,0,0,0.1);}
input, select, button {width:100%; padding:10px; margin-top:6px; border-radius:6px; border:1px solid #ccc; font-size:14px;}
button {background:#0066cc; color:#fff; font-weight:700; cursor:pointer; border:none;}
button:hover {background:#005bb5;}
.nav {position:fixed; bottom:0; left:0; right:0; display:flex; justify-content:space-around; padding:10px; background:#eee; border-top:1px solid #ccc;}
.nav div {text-align:center; font-size:13px; cursor:pointer; color:#333;}
.plan {border:1px solid #ccc; border-radius:8px; padding:10px; margin-bottom:8px; background:#fafafa; display:flex; justify-content:space-between; align-items:center;}
.plan button {width:auto; padding:6px 12px;}
.alert-note {background:#ffe5e5; color:#cc0000; padding:10px; border-radius:6px; margin-bottom:12px; font-weight:600; text-align:center;}
.user-box {display:flex; justify-content:space-between; margin-bottom:12px;}
.referral-box {display:flex; gap:4px; margin-bottom:12px;}
.hidden {display:none;}
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
<div class="alert-note">⚠️ Deposit/Withdrawal issues? Contact VERBOSE Admin: <br>Whatsapp: <a href="https://wa.me/03705519562">03705519562</a> | Email: <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></div>
<div class="user-box">
<div>
<div id="welcomeText" style="font-weight:700;">Welcome —</div>
<div id="memberSince">Member since —</div>
</div>
<div>
<div>Balance: Rs <span id="balanceText">0</span></div>
</div>
</div>
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

<!-- NAV -->
<div class="nav">
<div onclick="nav('dashboardCard')"><i class="fas fa-home"></i><br>Home</div>
<div onclick="nav('plansCard')"><i class="fas fa-gift"></i><br>Plans</div>
<div onclick="nav('depositCard')"><i class="fas fa-wallet"></i><br>Deposit</div>
<div onclick="nav('withdrawCard')"><i class="fas fa-money-bill"></i><br>Withdraw</div>
</div>

<script>
// STORAGE KEYS
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
localStorage.setItem(KEY_BAL+u,'0');
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
document.getElementById('welcomeText').innerText='Welcome, '+currentUser;
document.getElementById('memberSince').innerText='Member since: '+new Date().toLocaleDateString();
document.getElementById('balanceText').innerText=localStorage.getItem(KEY_BAL+currentUser)||'0';
}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList'); container.innerHTML='';
plans.forEach(plan=>{
const div=document.createElement('div'); div.className='plan';
div.innerHTML=`<div><strong>${plan.name}</strong><br>Invest: Rs ${plan.invest}<br>Total: Rs ${plan.total}<br>Daily: Rs ${plan.daily}<br>Days: ${plan.days}${plan.offer?`<br><span id="timer_${plan.id}">Loading timer...</span>`:''}</div>
<div><button onclick="buyPlan(${plan.id})">Buy Now</button></div>`;
container.appendChild(div);
if(plan.offer){startCountdown(plan.id);}
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
localStorage.setItem(KEY_BAL+currentUser,amount); alert('Deposit submitted! Balance updated.');
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
</script>
</body>
</html>
