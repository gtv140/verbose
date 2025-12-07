<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE — Neon Premium</title>
<style>
:root{--bg:#040404;--neon:#00f7ff;--accent:#ff5cff;--muted:rgba(230,247,251,0.6);}
body{margin:0;font-family:Arial,sans-serif;background:#040404;color:#e6fbff;}
.hidden{display:none;}
header{padding:20px;text-align:center;font-weight:900;color:var(--neon);text-shadow:0 0 10px var(--neon);font-size:22px;}
.wrap{max-width:480px;margin:12px auto;padding:12px;}
.card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));padding:12px;border-radius:12px;margin-bottom:12px;border:1px solid rgba(0,255,240,0.06);}
input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);background:transparent;color:#dff;font-size:14px;}
.btn{background:linear-gradient(90deg,var(--neon),var(--accent));border:none;color:#001;font-weight:800;cursor:pointer;padding:10px;border-radius:10px;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:rgba(0,0,0,0.6);border-top:1px solid rgba(0,255,240,0.03);}
.nav div{color:var(--neon);text-align:center;font-size:13px;cursor:pointer;}
.countdown{font-weight:800;color:var(--neon);margin-top:6px;}
.muted{color:var(--muted);font-size:13px;}
.plan{display:flex;justify-content:space-between;gap:10px;padding:10px;border-radius:10px;border:1px solid rgba(0,255,240,0.04);margin-bottom:8px;background:linear-gradient(180deg,rgba(255,255,255,0.01),rgba(0,0,0,0.04));}
.plan .meta{flex:1;}
.plan .actions{width:100px;text-align:right;}
.user-box{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;}
.user-box .badge{background:rgba(0,255,240,0.06);padding:4px 8px;border-radius:999px;color:var(--neon);}
</style>
</head>
<body>
<header>⚡ VERBOSE — Neon Premium</header>
<div class="wrap">

<!-- LOGIN -->
<div id="loginCard" class="card">
<h3 style="margin:0 0 8px 0;color:var(--neon)">Login / Signup</h3>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">New User</option>
</select>
<input id="inputUser" placeholder="Username" />
<input id="inputPass" placeholder="Password" type="password" />
<button class="btn" onclick="doAuth()">Submit</button>
<p class="muted">Tip: Same device & browser. Data stored locally.</p>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<div class="user-box">
<div>
<div id="welcomeText" style="font-weight:800;color:var(--neon)">Welcome —</div>
<div id="memberSince" class="muted">Member since —</div>
</div>
<div style="text-align:right;">
<div class="muted">Balance</div>
<div style="font-weight:900;font-size:18px">Rs <span id="balanceText">0</span></div>
<div class="badge">Daily: Rs <span id="dailyText">0</span></div>
<div class="btn" style="font-size:13px;padding:4px 8px;margin-top:4px;" onclick="doLogout()">Logout</div>
</div>
</div>
</div>

<!-- PLANS -->
<div id="plansCard" class="card hidden">
<h3 style="color:var(--neon);margin-top:0">Special Offers</h3>
<div class="muted" style="margin-bottom:8px;">7 plans with 24h countdown timer.</div>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="depositCard" class="card hidden">
<h3 style="color:var(--neon);margin:0 0 6px 0;">Deposit</h3>
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
<button class="btn" onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawCard" class="card hidden">
<h3 style="color:var(--neon);margin:0 0 6px 0;">Withdrawal</h3>
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
<button class="btn" onclick="submitWithdraw()">Request Withdrawal</button>
</div>

</div>

<!-- NAV -->
<div class="nav">
<div onclick="nav('dashboardCard')">🏠 Home</div>
<div onclick="nav('plansCard')">📦 Plans</div>
<div onclick="nav('depositCard')">💰 Deposit</div>
<div onclick="nav('withdrawCard')">💵 Withdraw</div>
</div>

<script>
// STORAGE KEYS
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
const KEY_DAILY='verbose_daily_';
const KEY_USER_PLANS='verbose_plans_';
const KEY_OFFERS='verbose_offer_';
const KEY_DEPOSITS='verbose_deposits';
const KEY_WITHDRAWS='verbose_withdraws';

// STATE
let currentUser=localStorage.getItem(KEY_USER)||null;
let plans=[];
let offerIntervals={};

// GENERATE 7 SPECIAL OFFERS
for(let i=1;i<=7;i++){
plans.push({id:i,name:'Plan '+i,invest:200*i,multiplier:3,total:200*i*3,offer:true});
}

function fmt(n){return Number(n).toLocaleString('en-US');}

// AUTH
function doAuth(){
const mode=document.getElementById('authMode').value;
const u=(document.getElementById('inputUser').value||'').trim();
const p=(document.getElementById('inputPass').value||'').trim();
if(!u||!p){alert('Enter username & password');return;}
const credKey='verbose_cred_'+u;
if(mode==='signup'){if(localStorage.getItem(credKey)){alert('Username exists');return;}
localStorage.setItem(credKey,p);localStorage.setItem(KEY_BAL+u,'0');localStorage.setItem(KEY_DAILY+u,'0');localStorage.setItem(KEY_USER_PLANS+u,'[]');}
else{if(localStorage.getItem(credKey)!==p){alert('Wrong username/password');return;}}
localStorage.setItem(KEY_USER,u);currentUser=u;afterLoginUI();
}

function afterLoginUI(){nav('dashboardCard');renderDashboard();renderPlans();startOffers();computeDailyCredits();}

// LOGOUT
function doLogout(){localStorage.removeItem(KEY_USER);currentUser=null;nav('loginCard');Object.values(offerIntervals).forEach(i=>clearInterval(i));}

// NAV
function nav(cardId){['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>document.getElementById(id).classList.add('hidden'));document.getElementById(cardId).classList.remove('hidden');renderDashboard();fillWithdrawUser();}

// DASHBOARD
function renderDashboard(){if(!currentUser)return;document.getElementById('welcomeText').innerText='Welcome, '+currentUser;document.getElementById('memberSince').innerText='Member since: '+new Date().toLocaleDateString();
document.getElementById('balanceText').innerText=fmt(Number(localStorage.getItem(KEY_BAL+currentUser)||0));
document.getElementById('dailyText').innerText=fmt(Number(localStorage.getItem(KEY_DAILY+currentUser)||0));}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList');container.innerHTML='';
plans.forEach(plan=>{
const div=document.createElement('div');div.className='plan';
div.innerHTML=`<div class="meta"><div style="font-weight:800">${plan.name}</div><div class="muted" style="margin-top:4px">Invest: Rs ${fmt(plan.invest)} · Total: Rs ${fmt(plan.total)}</div>${plan.offer?`<div class="countdown" id="countdown_${plan.id}">Loading timer...</div>`:''}</div>
<div class="actions">${plan.offer?'<button class="btn" onclick="buyPlan('+plan.id+')">Buy Now</button>':''}</div>`;container.appendChild(div);
});
}

// OFFERS COUNTDOWN
function startOffers(){plans.forEach(plan=>{if(!plan.offer)return;
const key=KEY_OFFERS+plan.id;let endTs=Number(localStorage.getItem(key)||0);if(!endTs||endTs<Date.now()){endTs=Date.now()+24*3600*1000;localStorage.setItem(key,endTs);}
const el=document.getElementById('countdown_'+plan.id);if(!el)return;function tick(){const diff=Math.floor((endTs-Date.now())/1000);if(diff<=0){el.innerText='Offer ended';clearInterval(offerIntervals[plan.id]);return;}
const h=Math.floor(diff/3600),m=Math.floor((diff%3600)/60),s=diff%60;el.innerText=`${String(h).padStart(2,'0')}h:${String(m).padStart(2,'0')}m:${String(s).padStart(2,'0')}s`;}tick();offerIntervals[plan.id]=setInterval(tick,1000);});}

// BUY PLAN
function buyPlan(id){if(!currentUser){alert('Login first');return;}
const plan=plans.find(p=>p.id===id);if(!plan)return;
let userPlans=JSON.parse(localStorage.getItem(KEY_USER_PLANS+currentUser)||'[]');
const dailyProfit=Math.round(plan.total/7);
userPlans.push({planId:plan.id,dailyProfit,lastCredit:Date.now()});
localStorage.setItem(KEY_USER_PLANS+currentUser,JSON.stringify(userPlans));
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);bal+=plan.invest;localStorage.setItem(KEY_BAL+currentUser,bal);alert(`Plan ${plan.name} purchased! Rs ${fmt(plan.invest)} added.`);renderDashboard();nav('depositCard');document.getElementById('depositAmount').value=plan.invest;updateDepositNumber();}

// DEPOSIT
function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';}
function submitDeposit(){
if(!currentUser){alert('Login first');return;}
const tx=(document.getElementById('depositTx').value||'').trim();const proof=document.getElementById('depositProof').files[0];const amount=Number(document.getElementById('depositAmount').value)||0;
if(!tx||!proof||!amount){alert('All fields required');return;}
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);bal+=amount;localStorage.setItem(KEY_BAL+currentUser,bal);
const deposits=JSON.parse(localStorage.getItem(KEY_DEPOSITS)||'[]');deposits.push({user:currentUser,method:document.getElementById('depositMethod').value,amount,tx,proof:proof.name,time:Date.now()});
localStorage.setItem(KEY_DEPOSITS,JSON.stringify(deposits));renderDashboard();alert('Deposit submitted & balance updated!');document.getElementById('depositTx').value='';document.getElementById('depositProof').value='';nav('dashboardCard');}

// WITHDRAW
function fillWithdrawUser(){document.getElementById('withdrawUsername').value=currentUser||'';}
function submitWithdraw(){
if(!currentUser){alert('Login first');return;}
const method=document.getElementById('withdrawMethod').value;
const acc=(document.getElementById('withdrawAccount').value||'').trim();
const amt=Number(document.getElementById('withdrawAmount').value)||0;
if(!acc||!amt){alert('Enter account & amount');return;}
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);if(amt>bal){alert('Insufficient balance');return;}
bal-=amt;localStorage.setItem(KEY_BAL+currentUser,bal);
const withdraws=JSON.parse(localStorage.getItem(KEY_WITHDRAWS)||'[]');withdraws.push({user:currentUser,method,account:acc,amount:amt,time:Date.now(),status:'pending'});
localStorage.setItem(KEY_WITHDRAWS,JSON.stringify(withdraws));renderDashboard();alert('Withdrawal request submitted!');nav('dashboardCard');}

// DAILY PROFIT
function computeDailyCredits(){
if(!currentUser)return;
let userPlans=JSON.parse(localStorage.getItem(KEY_USER_PLANS+currentUser)||'[]');
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
let dailyTotal=Number(localStorage.getItem(KEY_DAILY+currentUser)||0);
const now=Date.now();
userPlans.forEach(p=>{
const days=Math.floor((now-p.lastCredit)/86400000);
if(days>0){bal+=p.dailyProfit*days;dailyTotal+=p.dailyProfit*days;p.lastCredit+=days*86400000;}
});
localStorage.setItem(KEY_BAL+currentUser,bal);localStorage.setItem(KEY_DAILY+currentUser,dailyTotal);localStorage.setItem(KEY_USER_PLANS+currentUser,JSON.stringify(userPlans));renderDashboard();
}
setInterval(computeDailyCredits,60000); // every minute
</script>
</body>
</html>
