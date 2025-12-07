<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
body{margin:0;font-family:Arial,sans-serif;background:#f0f0f0;color:#333;}
header{padding:15px;text-align:center;background:#007bff;color:#fff;font-size:24px;font-weight:bold;}
.wrap{max-width:480px;margin:15px auto;padding:12px;}
.card{background:#fff;padding:12px;border-radius:12px;margin-bottom:12px;box-shadow:0 2px 6px rgba(0,0,0,0.1);}
input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:6px;border:1px solid #ccc;font-size:14px;}
.btn{background:#007bff;color:#fff;font-weight:600;border:none;cursor:pointer;padding:10px;border-radius:6px;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:#fff;border-top:1px solid #ccc;}
.nav div{text-align:center;font-size:12px;cursor:pointer;color:#007bff;}
.plan{display:flex;justify-content:space-between;gap:10px;padding:10px;border:1px solid #ccc;border-radius:8px;margin-bottom:8px;background:#f9f9f9;}
.plan .meta{flex:1;}
.plan .actions{width:110px;text-align:right;}
.hidden{display:none;}
.alert-note{background:#ffe0e0;color:#d00;padding:10px;border-radius:6px;text-align:center;font-weight:600;}
.success-note{background:#e0ffe0;color:#070;padding:10px;border-radius:6px;text-align:center;font-weight:600;}
.referral-box{display:flex;gap:6px;margin-bottom:12px;}
.referral-box input{flex:1;}
.logout-btn{display:block;width:100%;margin-top:10px;background:#dc3545;}
.countdown{font-weight:bold;color:#007bff;}
</style>
</head>
<body>
<header>VERBOSE</header>
<div class="wrap">

<!-- LOGIN -->
<div id="loginCard" class="card hidden">
<h3>Login / Signup</h3>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">New User</option>
</select>
<input id="inputUser" placeholder="Username"/>
<input id="inputPass" placeholder="Password" type="password"/>
<input id="referralInput" placeholder="Referral Code (optional)"/>
<button class="btn" onclick="doAuth()">Submit</button>
<p style="font-size:12px;color:#555;">Tip: Use same device & browser. Data stored locally.</p>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<div class="alert-note">⚠️ All transactions are manually approved. Deposit/Withdrawal issues? Contact Support immediately.</div>
<div>
<div id="welcomeText" style="font-weight:800;">Welcome —</div>
<div id="memberSince" style="font-size:12px;color:#555;">Member since —</div>
<div style="margin-top:8px;">Balance: Rs <span id="balanceText">0</span></div>
<div style="margin-top:4px;">Daily: Rs <span id="dailyText">0</span></div>
</div>
<div class="referral-box">
<input id="referralLink" readonly/>
<button class="btn" onclick="copyReferral()">Copy</button>
</div>
<button class="btn logout-btn" onclick="doLogout()">Logout</button>
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
<button class="btn" onclick="submitDeposit()">Submit Deposit</button>
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
<label>Account / Mobile Number</label>
<input id="withdrawAccount" placeholder="Enter account or mobile number"/>
<label>Amount (PKR)</label>
<input id="withdrawAmount" placeholder="Enter amount"/>
<button class="btn" onclick="submitWithdraw()">Request Withdrawal</button>
<p style="font-size:12px;color:#555;text-align:center;">⚠️ Withdrawal will be approved manually.</p>
</div>

<!-- NAV -->
<div class="nav">
<div onclick="nav('dashboardCard')"><i class="fas fa-home"></i><br>Home</div>
<div onclick="nav('plansCard')"><i class="fas fa-box"></i><br>Plans</div>
<div onclick="nav('depositCard')"><i class="fas fa-wallet"></i><br>Deposit</div>
<div onclick="nav('withdrawCard')"><i class="fas fa-hand-holding-usd"></i><br>Withdraw</div>
</div>

<script>
// STORAGE
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
const KEY_DAILY='verbose_daily_';
const KEY_USER_PLANS='verbose_plans_';
const KEY_DEPOSITS='verbose_deposits';
const KEY_WITHDRAWS='verbose_withdraws';
const KEY_OFFERS='verbose_offer_';
let currentUser=localStorage.getItem(KEY_USER)||null;
let plans=[],offerIntervals={};

// PLANS
for(let i=1;i<=7;i++){let invest=200*i;if(invest>3000) invest=3000;let days=20+Math.floor(Math.random()*10);plans.push({id:i,name:'Special Plan '+i,invest:invest,multiplier:3,total:invest*3,days:days,offer:true});}
for(let i=8;i<=32;i++){let invest=Math.round(3000+(i-8)*(30000-3000)/24);let days=20+Math.floor(Math.random()*61);let total=Math.round(invest*2.5);plans.push({id:i,name:'Plan '+(i-7),invest:invest,multiplier:2.5,total:total,days:days,offer:false});}
for(let i=33;i<=37;i++){plans.push({id:i,name:'Coming Soon',invest:0,multiplier:0,total:0,days:0,offer:false});}

function fmt(n){return Number(n).toLocaleString('en-US');}

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
if(ref && localStorage.getItem('verbose_cred_'+ref)){let bal=Number(localStorage.getItem(KEY_BAL+ref)||0);bal+=30;localStorage.setItem(KEY_BAL+ref,bal);alert(`Referral bonus Rs 30 added to ${ref}`);}
}else{if(localStorage.getItem(credKey)!==p){alert('Wrong username/password');return;}}
localStorage.setItem(KEY_USER,u);currentUser=u;afterLoginUI();
}

function afterLoginUI(){nav('dashboardCard');renderPlans();startOffers();updateReferralLink();updateDailyProfit();}

function updateReferralLink(){if(!currentUser) return;document.getElementById('referralLink').value=window.location.href+'?ref='+currentUser;}
function copyReferral(){const link=document.getElementById('referralLink');link.select();document.execCommand('copy');alert('Referral link copied!');}
function doLogout(){localStorage.removeItem(KEY_USER);currentUser=null;nav('loginCard');Object.values(offerIntervals).forEach(i=>clearInterval(i));}

// NAV
function nav(cardId){['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>document.getElementById(id).classList.add('hidden'));document.getElementById(cardId).classList.remove('hidden');renderDashboard();fillWithdrawUser();updateReferralLink();}

// DASHBOARD
function renderDashboard(){if(!currentUser)return;
document.getElementById('welcomeText').innerText='Welcome, '+currentUser;
document.getElementById('memberSince').innerText='Member since: '+new Date().toLocaleDateString();
document.getElementById('balanceText').innerText=fmt(Number(localStorage.getItem(KEY_BAL+currentUser)||0));
document.getElementById('dailyText').innerText=fmt(Number(localStorage.getItem(KEY_DAILY+currentUser)||0));
}

// DAILY PROFIT
function updateDailyProfit(){if(!currentUser) return;
let userPlans=JSON.parse(localStorage.getItem(KEY_USER_PLANS+currentUser)||'[]');
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
let dailyTotal=0;
userPlans.forEach(p=>{dailyTotal+=p.dailyProfit;});
bal+=dailyTotal;
localStorage.setItem(KEY_BAL+currentUser,bal);
localStorage.setItem(KEY_DAILY+currentUser,dailyTotal);
renderDashboard();
setTimeout(updateDailyProfit,24*60*60*1000); // next day
}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList');container.innerHTML='';
plans.forEach(plan=>{
const div=document.createElement('div');div.className='plan';if(plan.name==='Coming Soon') div.className+=' coming-soon';
let dailyProfit=plan.days>0?Math.round(plan.total/plan.days):0;
div.innerHTML=`<div class="meta"><strong>${plan.name}</strong><div style="font-size:12px;color:#555;">Invest: Rs ${fmt(plan.invest)} · Total: Rs ${fmt(plan.total)} · Days: ${plan.days} · Daily: Rs ${fmt(dailyProfit)}</div>${plan.offer?`<div class="countdown" id="countdown_${plan.id}">Loading...</div>`:''}</div><div class="actions">${plan.offer||plan.name!=='Coming Soon'?`<button class="btn" onclick="buyPlan(${plan.id})">Buy</button>`:''}</div>`;
container.appendChild(div);
});
}

// OFFER TIMER
function startOffers(){plans.forEach(plan=>{if(!plan.offer)return;
const key=KEY_OFFERS+plan.id;let endTs=Number(localStorage.getItem(key)||0);if(!endTs||endTs<Date.now()){endTs=Date.now()+24*3600*1000;localStorage.setItem(key,endTs);}
const el=document.getElementById('countdown_'+plan.id);if(!el)return;function tick(){const diff=Math.floor((endTs-Date.now())/1000);if(diff<=0){el.innerText='Offer ended';clearInterval(offerIntervals[plan.id]);return;}
const h=Math.floor(diff/3600),m=Math.floor((diff%3600)/60),s=diff%60;el.innerText=`${String(h).padStart(2,'0')}h:${String(m).padStart(2,'0')}m:${String(s).padStart(2,'0')}s`;}tick();offerIntervals[plan.id]=setInterval(tick,1000);});}

// BUY PLAN
function buyPlan(id){if(!currentUser){alert('Login first');return;}const plan=plans.find(p=>p.id===id);if(!plan || plan.name==='Coming Soon'){alert('Plan not available');return;}
let userPlans=JSON.parse(localStorage.getItem(KEY_USER_PLANS+currentUser)||'[]');const dailyProfit=Math.round(plan.total/plan.days);userPlans.push({planId:plan.id,dailyProfit,lastCredit:Date.now()});localStorage.setItem(KEY_USER_PLANS+currentUser,JSON.stringify(userPlans));
document.getElementById('depositAmount').value=plan.invest;updateDepositNumber();nav('depositCard');alert(`Plan ${plan.name} selected. Submit your deposit.`);}

// DEPOSIT
function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';}
function submitDeposit(){if(!currentUser){alert('Login first');return;}
const tx=(document.getElementById('depositTx').value||'').trim();const proof=document.getElementById('depositProof').files[0];const amount=Number(document.getElementById('depositAmount').value)||0;
if(!tx||!proof||!amount){alert('All fields required');return;}
const deposits=JSON.parse(localStorage.getItem(KEY_DEPOSITS)||[]);deposits.push({user:currentUser,method:document.getElementById('depositMethod').value,amount,tx,proof:proof.name,time:Date.now(),approved:false});
localStorage.setItem(KEY_DEPOSITS,JSON.stringify(deposits));alert('Deposit submitted!');document.getElementById('depositTx').value='';document.getElementById('depositProof').value='';nav('dashboardCard');}

// WITHDRAW
function fillWithdrawUser(){if(!currentUser) return;}
function submitWithdraw(){if(!currentUser){alert('Login first');return;}
const account=document.getElementById('withdrawAccount').value.trim();
const amount=Number(document.getElementById('withdrawAmount').value);
if(!account||!amount){alert('Enter account and amount');return;}
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
if(amount>bal){alert('Insufficient balance');return;}
bal-=amount;localStorage.setItem(KEY_BAL+currentUser,bal);
const withdraws=JSON.parse(localStorage.getItem(KEY_WITHDRAWS)||[]);withdraws.push({user:currentUser,method:document.getElementById('withdrawMethod').value,account,amount,time:Date.now(),approved:false});
localStorage.setItem(KEY_WITHDRAWS,JSON.stringify(withdraws));alert('Withdrawal submitted!');document.getElementById('withdrawAmount').value='';document.getElementById('withdrawAccount').value='';nav('dashboardCard');}
</script>
</body>
</html>
