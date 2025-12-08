<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
:root{
--bg:#0b0f1a;
--primary:#4da6ff;
--text:#e6ebf5;
--muted:#a0aec0;
--alert:#ff4d4d;
--success:#0f0;
}
body{margin:0;font-family:Arial,sans-serif;background:var(--bg);color:var(--text);}
.hidden{display:none;}
header{padding:20px;text-align:center;font-weight:900;color:var(--primary);font-size:28px;}
.wrap{max-width:480px;margin:12px auto;padding:12px;}
.card{background:#111827;padding:12px;border-radius:12px;margin-bottom:12px;border:1px solid var(--primary);}
input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:8px;border:1px solid #333;background:#1f2937;color:var(--text);font-size:14px;}
.btn{background:var(--primary);border:none;color:#fff;font-weight:600;cursor:pointer;padding:10px;border-radius:8px;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:#1f2937;border-top:1px solid #333;}
.nav div{color:var(--primary);text-align:center;font-size:13px;cursor:pointer;}
.countdown{font-weight:600;color:var(--primary);}
.muted{color:var(--muted);font-size:13px;}
.plan{display:flex;justify-content:space-between;gap:10px;padding:10px;border-radius:8px;border:1px solid #333;margin-bottom:8px;background:#111827;}
.plan .meta{flex:1;}
.plan .actions{width:110px;text-align:right;}
.user-box{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;}
.user-box .badge{background:#1f2937;padding:4px 8px;border-radius:999px;color:var(--primary);}
.admin-box{display:flex;justify-content:space-around;margin-top:12px;}
.admin-box div{flex:1;text-align:center;padding:10px;background:#1f2937;border-radius:10px;margin:2px;cursor:pointer;}
.admin-box div i{font-size:20px;margin-bottom:4px;display:block;color:var(--primary);}
.alert-note{background:#2c2f3a;color:var(--alert);padding:10px;margin-bottom:12px;border-radius:8px;font-weight:600;text-align:center;}
.success-note{background:#1f3a1f;color:var(--success);padding:10px;margin-bottom:12px;border-radius:8px;font-weight:600;text-align:center;}
.referral-box{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;}
.referral-box input{flex:1;}
.copy-btn{width:auto;padding:6px 12px;margin-left:4px;}
.logout-btn{background:var(--alert);color:#fff;padding:8px 12px;border:none;border-radius:6px;cursor:pointer;}
</style>
</head>
<body>
<header><i class="fas fa-coins"></i> VERBOSE</header>
<div class="wrap">

<!-- LOGIN / SIGNUP -->
<div id="loginCard" class="card">
<h3 style="margin:0 0 8px 0;color:var(--primary)"><i class="fas fa-user"></i> Login / Signup</h3>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">New User</option>
</select>
<input id="inputUser" placeholder="Username"/>
<input id="inputPass" placeholder="Password" type="password"/>
<input id="referralInput" placeholder="Referral Code (optional)"/>
<button class="btn" onclick="doAuth()"><i class="fas fa-sign-in-alt"></i> Submit</button>
<p class="muted">Tip: Use same device & browser. Data stored locally.</p>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<div class="alert-note">⚠️ Any issue with Deposit/Withdrawal? Contact Admin immediately.</div>
<div class="user-box">
<div>
<div id="welcomeText" style="font-weight:800;color:var(--primary)"><i class="fas fa-user-check"></i> Welcome —</div>
<div id="memberSince" class="muted">Member since —</div>
</div>
<div style="text-align:right;">
<div class="muted">Balance</div>
<div style="font-weight:900;font-size:18px"><i class="fas fa-coins"></i> Rs <span id="balanceText">0</span></div>
<div class="badge"><i class="fas fa-calendar-day"></i> Daily: Rs <span id="dailyText">0</span></div>
<button class="logout-btn" onclick="doLogout()"><i class="fas fa-sign-out-alt"></i> Logout</button>
</div>
</div>

<!-- PLANS -->
<div id="plansCard" class="card hidden">
<h3 style="color:var(--primary);margin-top:0"><i class="fas fa-gift"></i> Plans</h3>
<div class="muted" style="margin-bottom:8px;">Special Offers: 7 plans (24h countdown). Normal: 25 plans + 5 Coming Soon.</div>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="depositCard" class="card hidden">
<h3 style="color:var(--primary);margin:0 0 6px 0;"><i class="fas fa-hand-holding-usd"></i> Deposit</h3>
<label class="muted">Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash — 03705519562</option>
<option value="easypaisa">EasyPaisa — 03379827882</option>
</select>
<label class="muted">Number</label>
<input id="depositNumber" readonly/>
<label class="muted">Amount (PKR)</label>
<input id="depositAmount" readonly/>
<label class="muted">Transaction ID</label>
<input id="depositTx" placeholder="Enter TX ID"/>
<label class="muted">Upload Proof</label>
<input id="depositProof" type="file"/>
<button class="btn" onclick="submitDeposit()"><i class="fas fa-paper-plane"></i> Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawCard" class="card hidden">
<h3 style="color:var(--primary);margin:0 0 6px 0;"><i class="fas fa-money-bill-wave"></i> Withdrawal</h3>
<label class="muted">Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<label class="muted">Username</label>
<input id="withdrawUsername" readonly/>
<label class="muted">Account / Mobile Number</label>
<input id="withdrawAccount" placeholder="Enter account or mobile number"/>
<label class="muted">Amount (PKR)</label>
<input id="withdrawAmount" placeholder="Enter amount"/>
<button class="btn" onclick="submitWithdraw()"><i class="fas fa-paper-plane"></i> Request Withdrawal</button>
<p class="muted" style="margin-top:6px;text-align:center;">⚠️ Withdrawal will be approved manually. Contact Support for urgent issues.</p>
</div>

</div>

<!-- NAV -->
<div class="nav">
<div onclick="nav('dashboardCard')"><i class="fas fa-home"></i>Home</div>
<div onclick="nav('plansCard')"><i class="fas fa-box"></i>Plans</div>
<div onclick="nav('depositCard')"><i class="fas fa-wallet"></i>Deposit</div>
<div onclick="nav('withdrawCard')"><i class="fas fa-hand-holding-usd"></i>Withdraw</div>
</div>

<script>
// STORAGE KEYS
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
const KEY_USER_PLANS='verbose_plans_';
let currentUser=localStorage.getItem(KEY_USER)||null;

// SAMPLE PLANS
const plans=[];
for(let i=1;i<=7;i++){plans.push({id:i,name:'Special Plan '+i,invest:200*i,multiplier:3,total:600*i,days:20+Math.floor(Math.random()*10),offer:true});}
for(let i=8;i<=32;i++){plans.push({id:i,name:'Plan '+(i-7),invest:3000+(i-8)*1000,multiplier:2.5,total:(3000+(i-8)*1000)*2.5,days:20+Math.floor(Math.random()*10),offer:false});}
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
localStorage.setItem(KEY_BAL+u,0);
localStorage.setItem(KEY_USER_PLANS+u,'[]');
if(ref && localStorage.getItem('verbose_cred_'+ref)){
localStorage.setItem(KEY_BAL+ref,Number(localStorage.getItem(KEY_BAL+ref)||0)+30);
alert('Referral bonus Rs 30 added to '+ref);
}
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
updateDepositNumber();
}

function doLogout(){
localStorage.removeItem(KEY_USER);
currentUser=null;
nav('loginCard');
}

// NAV
function nav(cardId){
['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>document.getElementById(id).classList.add('hidden'));
document.getElementById(cardId).classList.remove('hidden');
if(cardId==='dashboardCard') renderDashboard();
if(cardId==='withdrawCard') fillWithdrawUser();
}

// DASHBOARD
function renderDashboard(){
if(!currentUser) return;
document.getElementById('welcomeText').innerText='Welcome, '+currentUser;
document.getElementById('memberSince').innerText='Member since: '+new Date().toLocaleDateString();
document.getElementById('balanceText').innerText=fmt(Number(localStorage.getItem(KEY_BAL+currentUser)||0));
}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList');container.innerHTML='';
plans.forEach(plan=>{
const div=document.createElement('div');div.className='plan';
const dailyProfit=Math.round(plan.total/plan.days);
div.innerHTML=`<div class="meta"><div style="font-weight:800">${plan.name}</div>
<div class="muted" style="margin-top:4px">Invest: Rs ${fmt(plan.invest)} · Total: Rs ${fmt(plan.total)} · Days: ${plan.days} · Daily: Rs ${fmt(dailyProfit)}</div>
</div>
<div class="actions"><button class="btn" onclick="buyPlan(${plan.id})">Buy Now</button></div>`;
container.appendChild(div);
});
}

// BUY PLAN
function buyPlan(id){
if(!currentUser){alert('Login first');return;}
const plan=plans.find(p=>p.id===id);
let userPlans=JSON.parse(localStorage.getItem(KEY_USER_PLANS+currentUser)||'[]');
const dailyProfit=Math.round(plan.total/plan.days);
userPlans.push({planId:plan.id,dailyProfit,lastCredit:Date.now()});
localStorage.setItem(KEY_USER_PLANS+currentUser,JSON.stringify(userPlans));
document.getElementById('depositAmount').value=plan.invest;
updateDepositNumber();
nav('depositCard');
alert(`Plan ${plan.name} selected. Submit your deposit.`);
}

// DEPOSIT
function updateDepositNumber(){
const method=document.getElementById('depositMethod').value;
document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function submitDeposit(){
if(!currentUser){alert('Login first');return;}
const tx=document.getElementById('depositTx').value.trim();
const proof=document.getElementById('depositProof').files[0];
const amount=Number(document.getElementById('depositAmount').value);
if(!tx||!proof||!amount){alert('All fields required');return;}
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);bal+=amount;
localStorage.setItem(KEY_BAL+currentUser,bal);
alert('Deposit submitted!');
document.getElementById('depositTx').value='';
document.getElementById('depositProof').value='';
renderDashboard();
nav('dashboardCard');
}

// WITHDRAW
function fillWithdrawUser(){if(!currentUser) return;document.getElementById('withdrawUsername').value=currentUser;}
function submitWithdraw(){
if(!currentUser){alert('Login first');return;}
const account=document.getElementById('withdrawAccount').value.trim();
const amount=Number(document.getElementById('withdrawAmount').value);
if(!account||!amount){alert('Enter account and amount');return;}
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
if(amount>bal){alert('Insufficient balance');return;}
bal-=amount;
localStorage.setItem(KEY_BAL+currentUser,bal);
alert('Withdrawal requested. Admin will approve manually.');
document.getElementById('withdrawAccount').value='';
document.getElementById('withdrawAmount').value='';
renderDashboard();
nav('dashboardCard');
}
</script>
</body>
</html>
