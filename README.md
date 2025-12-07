<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Verbose</title>
<style>
body {margin:0; font-family: Arial, sans-serif; background:#f5f5f5; color:#333;}
.hidden {display:none;}
header {padding:20px; text-align:center; font-size:24px; font-weight:bold; background:#ddd;}
.wrap {max-width:500px; margin:20px auto; padding:15px; background:#fff; border:1px solid #ccc; border-radius:8px;}
.card {margin-bottom:15px; padding:12px; border:1px solid #ccc; border-radius:8px; background:#fafafa;}
input, select, button {width:100%; padding:10px; margin-top:5px; border-radius:5px; border:1px solid #ccc; font-size:14px;}
button {cursor:pointer;}
.nav {position:fixed; bottom:0; left:0; right:0; display:flex; justify-content:space-around; padding:10px; background:#eee; border-top:1px solid #ccc;}
.nav div {cursor:pointer;}
.muted {color:#666; font-size:13px; margin-top:5px;}
.plan {margin-bottom:10px; padding:8px; border:1px solid #ccc; border-radius:5px; background:#f0f0f0;}
</style>
</head>
<body>

<header>Verbose</header>

<div class="wrap">

<!-- LOGIN / SIGNUP -->
<div id="loginCard" class="card">
<h3>Login / Signup</h3>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">Signup</option>
</select>
<input id="inputUser" placeholder="Username"/>
<input id="inputPass" type="password" placeholder="Password"/>
<input id="referralInput" placeholder="Referral Code (optional)"/>
<button onclick="doAuth()">Submit</button>
<p class="muted">Tip: Use same device & browser. Data stored locally.</p>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<div id="welcomeText">Welcome —</div>
<div id="memberSince" class="muted">Member since —</div>
<div class="muted">Balance: Rs <span id="balanceText">0</span></div>
<div class="muted">Daily Profit: Rs <span id="dailyText">0</span></div>
<button onclick="doLogout()">Logout</button>
</div>

<!-- REFERRAL -->
<div class="card hidden">
<label>Referral Link</label>
<input id="referralLink" readonly/>
<button onclick="copyReferral()">Copy Link</button>
</div>

<!-- SUPPORT -->
<div class="card hidden">
<h3>Support / Administration</h3>
<p>Contact for any issues:</p>
<ul>
<li>WhatsApp: +923001112233</li>
<li>Email: support@verbose.com</li>
<li>Admin Name: Mr. Admin</li>
<li>All transactions are manually approved</li>
</ul>
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
<option value="jazzcash">JazzCash — 03705519562</option>
<option value="easypaisa">EasyPaisa — 03379827882</option>
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
<h3>Withdrawal</h3>
<label>Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<label>Username</label>
<input id="withdrawUsername" readonly/>
<label>Account / Mobile Number</label>
<input id="withdrawAccount" placeholder="Enter account or mobile number"/>
<label>Amount (PKR)</label>
<input id="withdrawAmount" placeholder="Enter amount"/>
<button onclick="submitWithdraw()">Request Withdrawal</button>
<p class="muted">Withdrawal will be approved manually. Contact support for urgent issues.</p>
</div>

</div>

<!-- NAV -->
<div class="nav">
<div onclick="nav('dashboardCard')">Home</div>
<div onclick="nav('plansCard')">Plans</div>
<div onclick="nav('depositCard')">Deposit</div>
<div onclick="nav('withdrawCard')">Withdraw</div>
<div onclick="nav('dashboardCard')">Support</div>
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

// FULL PLAN LIST
let plans = [];
// 7 Special Plans
for(let i=1;i<=7;i++){
let invest=200*i; if(invest>3000) invest=3000; let days=20+Math.floor(Math.random()*51);
plans.push({id:i,name:'Special Plan '+i,invest:invest,total:invest*3,days:days});
}
// 25 Normal Plans
for(let i=8;i<=32;i++){
let invest=Math.round(3000+(i-8)*(30000-3000)/24); let days=20+Math.floor(Math.random()*51);
plans.push({id:i,name:'Plan '+(i-7),invest:invest,total:Math.round(invest*2.5),days:days});
}
// 5 Coming Soon
for(let i=33;i<=37;i++){
plans.push({id:i,name:'Coming Soon',invest:0,total:0,days:0});
}

function fmt(n){return Number(n).toLocaleString('en-US');}

// AUTH
function doAuth(){
const mode=document.getElementById('authMode').value;
const u=document.getElementById('inputUser').value.trim();
const p=document.getElementById('inputPass').value.trim();
const ref=document.getElementById('referralInput').value.trim();
if(!u||!p){alert('Enter username & password');return;}
const credKey='verbose_cred_'+u;
if(mode==='signup'){
if(localStorage.getItem(credKey)){alert('Username exists');return;}
localStorage.setItem(credKey,p);
localStorage.setItem(KEY_BAL+u,'0');
localStorage.setItem(KEY_DAILY+u,'0');
localStorage.setItem(KEY_USER_PLANS+u,'[]');
if(ref && localStorage.getItem('verbose_cred_'+ref)){
let bal=Number(localStorage.getItem(KEY_BAL+ref)||0); bal+=30; localStorage.setItem(KEY_BAL+ref,bal);
alert(`Referral bonus Rs 30 added to ${ref}`);
}
}else{
if(localStorage.getItem(credKey)!==p){alert('Wrong username/password');return;}
}
localStorage.setItem(KEY_USER,u); currentUser=u; afterLoginUI();
}

function afterLoginUI(){nav('dashboardCard'); renderPlans(); updateReferralLink();}
function updateReferralLink(){if(!currentUser) return; document.getElementById('referralLink').value=window.location.href+'?ref='+currentUser;}
function copyReferral(){const link=document.getElementById('referralLink'); link.select(); document.execCommand('copy'); alert('Referral link copied!');}
function doLogout(){localStorage.removeItem(KEY_USER); currentUser=null; nav('loginCard');}

// NAV
function nav(cardId){
['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>document.getElementById(id).classList.add('hidden'));
document.getElementById(cardId).classList.remove('hidden');
renderDashboard();
fillWithdrawUser();
}

// DASHBOARD
function renderDashboard(){
if(!currentUser) return;
document.getElementById('welcomeText').innerText='Welcome, '+currentUser;
document.getElementById('memberSince').innerText='Member since: '+new Date().toLocaleDateString();
document.getElementById('balanceText').innerText=fmt(Number(localStorage.getItem(KEY_BAL+currentUser)||0));
document.getElementById('dailyText').innerText=fmt(Number(localStorage.getItem(KEY_DAILY+currentUser)||0));
}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList'); container.innerHTML='';
plans.forEach(plan=>{
const div=document.createElement('div');
div.className='plan';
div.innerHTML=`<strong>${plan.name}</strong><br>Invest: Rs ${fmt(plan.invest)} - Total: Rs ${fmt(plan.total)} - Days: ${plan.days}<br>
<button onclick="selectPlan(${plan.id})">Select Plan</button>`;
container.appendChild(div);
});
});

function selectPlan(id){
if(!currentUser){alert('Login first'); return;}
const plan = plans.find(p=>p.id===id);
if(plan.name==='Coming Soon'){alert('Plan not available yet'); return;}
let userPlans = JSON.parse(localStorage.getItem(KEY_USER_PLANS+currentUser)||'[]');
userPlans.push({planId:plan.id,dailyProfit:Math.round(plan.total/plan.days),lastCredit:Date.now()});
localStorage.setItem(KEY_USER_PLANS+currentUser,JSON.stringify(userPlans));
document.getElementById('depositAmount').value=plan.invest;
updateDepositNumber();
nav('depositCard');
alert(`Plan ${plan.name} selected. Submit your deposit. Admin will approve manually.`);
}

// DEPOSIT
function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';}
function submitDeposit(){
if(!currentUser){alert('Login first'); return;}
const tx=document.getElementById('depositTx').value.trim();
const proof=document.getElementById('depositProof').files[0];
const amount=Number(document.getElementById('depositAmount').value);
if(!tx||!proof||!amount){alert('All fields required'); return;}
const deposits=JSON.parse(localStorage.getItem(KEY_DEPOSITS)||[]);
deposits.push({user:currentUser,method:document.getElementById('depositMethod').value,amount,tx,proof:proof.name,time:Date.now(),approved:false});
localStorage.setItem(KEY_DEPOSITS,JSON.stringify(deposits));
alert('Deposit submitted! Admin will approve manually.');
document.getElementById('depositTx').value=''; document.getElementById('depositProof').value='';
nav('dashboardCard');
}

// WITHDRAW
function fillWithdrawUser(){if(!currentUser) return; document.getElementById('withdrawUsername').value=currentUser;}
function submitWithdraw(){
if(!currentUser){alert('Login first'); return;}
const method=document.getElementById('withdrawMethod').value;
const account=document.getElementById('withdrawAccount').value.trim();
const amount=Number(document.getElementById('withdrawAmount').value);
if(!account||!amount){alert('Enter account and amount'); return;}
const withdraws=JSON.parse(localStorage.getItem(KEY_WITHDRAWS)||[]);
withdraws.push({user:currentUser,method,account,amount,time:Date.now(),approved:false});
localStorage.setItem(KEY_WITHDRAWS,JSON.stringify(withdraws));
alert('Withdrawal request submitted! Admin will approve manually.');
document.getElementById('withdrawAccount').value=''; document.getElementById('withdrawAmount').value='';
nav('dashboardCard');
}

if(currentUser){afterLoginUI();}
</script>

</body>
</html>
