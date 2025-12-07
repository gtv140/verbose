<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE - Rock Earn Branch</title>
<style>
body {margin:0;font-family:Arial,sans-serif;background:#f4f4f4;color:#333;}
header {background:#333;color:#fff;text-align:center;padding:15px;font-size:22px;font-weight:bold;}
.wrap {max-width:480px;margin:15px auto;padding:10px;}
.card {background:#fff;padding:12px;margin-bottom:12px;border-radius:10px;box-shadow:0 2px 6px rgba(0,0,0,0.1);}
input, select, button {width:100%;padding:10px;margin-top:6px;border-radius:6px;border:1px solid #ccc;font-size:14px;}
button {background:#333;color:#fff;border:none;cursor:pointer;}
.nav {display:flex;justify-content:space-around;padding:10px;background:#eee;position:fixed;bottom:0;left:0;right:0;}
.nav div {text-align:center;cursor:pointer;}
.hidden {display:none;}
.plan {border:1px solid #ccc;padding:8px;margin-bottom:8px;border-radius:6px;}
.plan h4 {margin:0 0 4px 0;}
.alert {background:#ffdddd;color:#900;padding:8px;margin-bottom:10px;border-radius:5px;}
.success {background:#ddffdd;color:#090;padding:8px;margin-bottom:10px;border-radius:5px;}
</style>
</head>
<body>
<header>VERBOSE - Rock Earn Branch</header>
<div class="wrap">

<!-- LOGIN / SIGNUP -->
<div id="loginCard" class="card">
<h3>Login / Signup</h3>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">Sign Up</option>
</select>
<input id="inputUser" placeholder="Username"/>
<input id="inputPass" placeholder="Password" type="password"/>
<input id="referralInput" placeholder="Referral Code (optional)"/>
<button onclick="doAuth()">Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<div id="welcomeText" style="font-weight:bold;"></div>
<div id="memberSince" style="font-size:13px;color:#555;"></div>
<div>Balance: Rs <span id="balanceText">0</span></div>
<div>Daily Profit: Rs <span id="dailyText">0</span></div>
<button onclick="doLogout()" style="margin-top:10px;">Logout</button>

<h4>Plans</h4>
<div id="plansList"></div>

<h4>Support</h4>
<p>Email: Rock.earn92@gmail.com</p>
<p>WhatsApp: 03705519562</p>
</div>

<!-- NAV -->
<div class="nav">
<div onclick="nav('dashboardCard')">Dashboard</div>
<div onclick="nav('plansCard')">Plans</div>
<div onclick="nav('depositCard')">Deposit</div>
<div onclick="nav('withdrawCard')">Withdraw</div>
</div>

<!-- PLANS CARD -->
<div id="plansCard" class="card hidden">
<h3>All Plans</h3>
<div id="allPlans"></div>
</div>

<!-- DEPOSIT -->
<div id="depositCard" class="card hidden">
<h3>Deposit</h3>
<select id="depositMethod">
<option value="jazzcash">JazzCash — 03705519562</option>
<option value="easypaisa">EasyPaisa — 03379827882</option>
</select>
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
<input id="withdrawAccount" placeholder="Account / Mobile Number"/>
<input id="withdrawAmount" placeholder="Amount"/>
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<script>
// STORAGE KEYS
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
const KEY_DAILY='verbose_daily_';
const KEY_USER_PLANS='verbose_plans_';
let currentUser=localStorage.getItem(KEY_USER)||null;

// SAMPLE PLANS
let plans = [];
for(let i=1;i<=7;i++){
plans.push({id:i,name:'Special Plan '+i,invest:200*i,total:(200*i)*3,days:20+Math.floor(Math.random()*11),offer:true});
}
for(let i=8;i<=32;i++){
let invest=Math.round(3000+(i-8)*(30000-3000)/24);
plans.push({id:i,name:'Plan '+(i-7),invest:invest,total:Math.round(invest*2.5),days:20+Math.floor(Math.random()*51),offer:false});
}
for(let i=33;i<=37;i++){
plans.push({id:i,name:'Coming Soon',invest:0,total:0,days:0,offer:false});
}

// AUTH
function doAuth(){
const mode=document.getElementById('authMode').value;
const u=document.getElementById('inputUser').value.trim();
const p=document.getElementById('inputPass').value.trim();
const ref=document.getElementById('referralInput').value.trim();
if(!u||!p){alert('Enter username & password'); return;}
const credKey='verbose_cred_'+u;
if(mode==='signup'){
if(localStorage.getItem(credKey)){alert('Username exists'); return;}
localStorage.setItem(credKey,p);
localStorage.setItem(KEY_BAL+u,'0');
localStorage.setItem(KEY_DAILY+u,'0');
localStorage.setItem(KEY_USER_PLANS+u,'[]');
}else{
if(localStorage.getItem(credKey)!==p){alert('Wrong username/password'); return;}
}
localStorage.setItem(KEY_USER,u);currentUser=u;
afterLoginUI();
}

function afterLoginUI(){
nav('dashboardCard');
renderDashboard();
renderPlans();
renderAllPlans();
}

function nav(cardId){
['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>{
document.getElementById(id).classList.add('hidden');
});
document.getElementById(cardId).classList.remove('hidden');
if(cardId==='dashboardCard') renderDashboard();
}

function renderDashboard(){
if(!currentUser) return;
document.getElementById('welcomeText').innerText='Welcome, '+currentUser;
document.getElementById('memberSince').innerText='Member since: '+new Date().toLocaleDateString();
document.getElementById('balanceText').innerText=localStorage.getItem(KEY_BAL+currentUser)||0;
document.getElementById('dailyText').innerText=localStorage.getItem(KEY_DAILY+currentUser)||0;
}

function renderPlans(){
const container=document.getElementById('plansList');container.innerHTML='';
plans.forEach(plan=>{
let dailyProfit=plan.days>0?Math.round(plan.total/plan.days):0;
const div=document.createElement('div');
div.className='plan';
div.innerHTML=`<h4>${plan.name}</h4><p>Invest: Rs ${plan.invest} | Total: Rs ${plan.total} | Days: ${plan.days} | Daily: Rs ${dailyProfit}</p>`;
container.appendChild(div);
});
}

function renderAllPlans(){
const container=document.getElementById('allPlans');container.innerHTML='';
plans.forEach(plan=>{
let dailyProfit=plan.days>0?Math.round(plan.total/plan.days):0;
const div=document.createElement('div');
div.className='plan';
div.innerHTML=`<h4>${plan.name}</h4><p>Invest: Rs ${plan.invest} | Total: Rs ${plan.total} | Days: ${plan.days} | Daily: Rs ${dailyProfit}</p>`;
container.appendChild(div);
});
}

function doLogout(){
localStorage.removeItem(KEY_USER);currentUser=null;
nav('loginCard');
}

// DEPOSIT
function submitDeposit(){
if(!currentUser){alert('Login first'); return;}
const amount=Number(document.getElementById('depositAmount').value)||0;
if(amount<=0){alert('Enter amount'); return;}
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
bal+=amount;
localStorage.setItem(KEY_BAL+currentUser,bal);
alert('Deposit added successfully!');
nav('dashboardCard');
renderDashboard();
}

// WITHDRAW
function submitWithdraw(){
if(!currentUser){alert('Login first'); return;}
const amount=Number(document.getElementById('withdrawAmount').value)||0;
if(amount<=0){alert('Enter amount'); return;}
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
if(amount>bal){alert('Insufficient balance'); return;}
bal-=amount;
localStorage.setItem(KEY_BAL+currentUser,bal);
alert('Withdrawal requested successfully!');
nav('dashboardCard');
renderDashboard();
}
</script>
</body>
</html>
