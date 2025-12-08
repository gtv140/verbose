<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE Premium Final</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
:root{
--bg:#0b0f1a;
--primary:#4da6ff;
--secondary:#0f62ff;
--text:#e6ebf5;
--muted:#a0aec0;
--success:#0f0;
--alert:#ff4d4d;
}
body{margin:0;font-family:Arial,sans-serif;color:var(--text);background:#0b0f1a;overflow-x:hidden;}
header{padding:20px;text-align:center;font-weight:900;color:var(--primary);font-size:28px;text-shadow:0 0 8px #4da6ff;}
.wrap{max-width:480px;margin:20px auto;padding:12px;}
.card{background:rgba(17,24,39,0.95);padding:12px;border-radius:12px;margin-bottom:12px;border:1px solid var(--primary);box-shadow:0 0 15px #4da6ff;}
input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:8px;border:1px solid #333;background:#1f2937;color:var(--text);font-size:14px;}
.btn{background:var(--primary);border:none;color:#fff;font-weight:600;cursor:pointer;padding:10px;border-radius:8px;text-shadow:0 0 4px #4da6ff;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:#1f2937;border-top:1px solid #333;box-shadow:0 -2px 10px #4da6ff;}
.nav div{color:var(--primary);text-align:center;font-size:13px;cursor:pointer;text-shadow:0 0 3px #4da6ff;}
.countdown{font-weight:600;color:#4da6ff;}
.muted{color:var(--muted);font-size:13px;}
.plan{display:flex;justify-content:space-between;gap:10px;padding:10px;border-radius:8px;border:1px solid #333;margin-bottom:8px;background:#111827;}
.plan .meta{flex:1;}
.plan .actions{width:110px;text-align:right;}
.user-box{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;}
.user-box .badge{background:#1f2937;padding:4px 8px;border-radius:999px;color:var(--primary);}
.admin-box{display:flex;justify-content:space-around;margin-top:12px;}
.admin-box div{flex:1;text-align:center;padding:10px;background:#1f2937;border-radius:10px;margin:2px;cursor:pointer;box-shadow:0 0 8px #4da6ff;}
.admin-box div i{font-size:20px;margin-bottom:4px;display:block;color:var(--primary);}
.alert-note{background:#2c2f3a;color:var(--alert);padding:10px;margin-bottom:12px;border-radius:8px;font-weight:600;text-align:center;}
.success-note{background:#1f3a1f;color:var(--success);padding:10px;margin-bottom:12px;border-radius:8px;font-weight:600;text-align:center;}
.referral-box{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;}
.referral-box input{flex:1;}
.copy-btn{width:auto;padding:6px 12px;margin-left:4px;}
.logout-btn{background:var(--alert);color:#fff;padding:8px 12px;border:none;border-radius:6px;cursor:pointer;}
.hidden{display:none;}
body::before{
content:"";position:fixed;top:0;left:0;width:100%;height:100%;background:linear-gradient(270deg,#0f62ff,#4da6ff,#0f62ff);background-size:600% 600%;z-index:-1;animation:bgAnim 12s ease infinite;
}
@keyframes bgAnim{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
</style>
</head>
<body>
<header><i class="fas fa-coins"></i> VERBOSE Premium</header>
<div class="wrap">

<!-- LOGIN -->
<div id="loginCard" class="card">
<h3 style="margin:0 0 8px 0;color:var(--primary)"><i class="fas fa-user"></i> Login / Signup</h3>
<select id="authMode"><option value="login">Login</option><option value="signup">New User</option></select>
<input id="inputUser" placeholder="Username"/>
<input id="inputPass" placeholder="Password" type="password"/>
<input id="referralInput" placeholder="Referral Code (optional)"/>
<button class="btn" onclick="doAuth()"><i class="fas fa-sign-in-alt"></i> Submit</button>
<p class="muted">Tip: Use same device & browser. Data stored locally.</p>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<div class="alert-note">⚠️ Any issue with Deposit/Withdrawal? Contact Admin.</div>
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

<!-- REFERRAL -->
<div class="referral-box">
<input id="referralLink" readonly/>
<button class="btn copy-btn" onclick="copyReferral()"><i class="fas fa-copy"></i> Copy</button>
</div>

<!-- PLANS -->
<div id="plansCard" class="card hidden">
<h3 style="color:var(--primary);margin-top:0"><i class="fas fa-gift"></i> Plans</h3>
<div class="muted" style="margin-bottom:8px;">Special Offers + Normal Plans</div>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="depositCard" class="card hidden">
<h3 style="color:var(--primary);margin:0 0 6px 0;"><i class="fas fa-hand-holding-usd"></i> Deposit</h3>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash — 03705519562</option>
<option value="easypaisa">EasyPaisa — 03379827882</option>
</select>
<input id="depositNumber" readonly/>
<input id="depositAmount" readonly/>
<input id="depositTx" placeholder="Transaction ID"/>
<input id="depositProof" type="file"/>
<button class="btn" onclick="submitDeposit()"><i class="fas fa-paper-plane"></i> Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawCard" class="card hidden">
<h3 style="color:var(--primary);margin:0 0 6px 0;"><i class="fas fa-money-bill-wave"></i> Withdrawal</h3>
<select id="withdrawMethod"><option value="jazzcash">JazzCash</option><option value="easypaisa">EasyPaisa</option><option value="bank">Bank</option></select>
<input id="withdrawUsername" readonly/>
<input id="withdrawAccount" placeholder="Account / Mobile"/>
<input id="withdrawAmount" placeholder="Amount"/>
<button class="btn" onclick="submitWithdraw()"><i class="fas fa-paper-plane"></i> Request Withdrawal</button>
<p class="muted" style="text-align:center;">⚠️ Admin approves manually</p>
</div>

<!-- SUPPORT -->
<div id="supportBox" class="card hidden">
<h3 style="color:var(--primary)">Support</h3>
<p>WhatsApp: <a href="https://wa.me/923705519562" target="_blank">03705519562</a></p>
<p>Email: <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></p>
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
// Storage & state
const KEY_USER='verbose_user', KEY_BAL='verbose_balance_', KEY_DAILY='verbose_daily_', KEY_USER_PLANS='verbose_plans_', KEY_DEPOSITS='verbose_deposits', KEY_WITHDRAWS='verbose_withdraws';
let currentUser=localStorage.getItem(KEY_USER)||null;
let plans=[];

// Setup plans
for(let i=1;i<=7;i++){plans.push({id:i,name:'Special '+i,invest:200*i,total:200*i*3,daily:Math.round(200*i*3/20)});}
for(let i=8;i<=32;i++){let invest=3000+(i-8)*1000;plans.push({id:i,name:'Plan '+(i-7),invest:invest,total:Math.round(invest*2.5),daily:Math.round(invest*2.5/20)});}

// Utils
function fmt(n){return Number(n).toLocaleString('en-US');}

// Auth
function doAuth(){
let u=document.getElementById('inputUser').value.trim();
let p=document.getElementById('inputPass').value.trim();
let ref=document.getElementById('referralInput').value.trim();
if(!u||!p){alert('Enter username & password');return;}
let credKey='verbose_cred_'+u;
if(document.getElementById('authMode').value==='signup'){
if(localStorage.getItem(credKey)){alert('Username exists');return;}
localStorage.setItem(credKey,p);localStorage.setItem(KEY_BAL+u,'0');localStorage.setItem(KEY_DAILY+u,'0');localStorage.setItem(KEY_USER_PLANS+u,'[]');
if(ref && localStorage.getItem('verbose_cred_'+ref)){let b=Number(localStorage.getItem(KEY_BAL+ref)||0);localStorage.setItem(KEY_BAL+ref,b+30);}
}else{if(localStorage.getItem(credKey)!==p){alert('Wrong username/password');return;}}
localStorage.setItem(KEY_USER,u);currentUser=u;nav('dashboardCard');renderDashboard();renderPlans();updateReferral();
}
function doLogout(){localStorage.removeItem(KEY_USER);currentUser=null;nav('loginCard');}

// Navigation
function nav(cardId){
['loginCard','dashboardCard','plansCard','depositCard','withdrawCard','supportBox'].forEach(id=>document.getElementById(id).classList.add('hidden'));
document.getElementById(cardId).classList.remove('hidden');
renderDashboard();fillWithdrawUser();updateReferral();
}

// Dashboard
function renderDashboard(){if(!currentUser) return;
document.getElementById('welcomeText').innerText='Welcome, '+currentUser;
document.getElementById('balanceText').innerText=fmt(Number(localStorage.getItem(KEY_BAL+currentUser)||0));
document.getElementById('dailyText').innerText=fmt(Number(localStorage.getItem(KEY_DAILY+currentUser)||0));
document.getElementById('memberSince').innerText='Member since: '+new Date().toLocaleDateString();
}

// Plans
function renderPlans(){
let container=document.getElementById('plansList');container.innerHTML='';
plans.forEach(p=>{
let div=document.createElement('div');div.className='plan';
div.innerHTML=`<div class="meta"><b>${p.name}</b><div class="muted">Invest: Rs ${fmt(p.invest)} · Total: Rs ${fmt(p.total)} · Daily: Rs ${fmt(p.daily)}</div></div>
<div class="actions"><button class="btn" onclick="buyPlan(${p.id})">Buy</button></div>`;
container.appendChild(div);
});}

// Buy Plan
function buyPlan(id){if(!currentUser){alert('Login first');return;}
let plan=plans.find(p=>p.id===id);
document.getElementById('depositAmount').value=plan.invest;
updateDepositNumber();
nav('depositCard');
alert('Plan '+plan.name+' selected. Submit deposit.');}

// Deposit
function updateDepositNumber(){let m=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=m==='jazzcash'?'03705519562':'03379827882';}
function submitDeposit(){if(!currentUser){alert('Login first');return;}
let amt=Number(document.getElementById('depositAmount').value)||0;
let tx=document.getElementById('depositTx').value.trim();
if(!tx||amt<=0){alert('Enter TX and amount');return;}
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);bal+=amt;localStorage.setItem(KEY_BAL+currentUser,bal);
alert('Deposit submitted!');nav('dashboardCard');renderDashboard();}

// Withdraw
function fillWithdrawUser(){if(currentUser) document.getElementById('withdrawUsername').value=currentUser;}
function submitWithdraw(){if(!currentUser){alert('Login first');return;}
let amt=Number(document.getElementById('withdrawAmount').value)||0;if(amt<=0){alert('Enter amount');return;}
alert('Withdrawal request submitted! Admin will approve.');document.getElementById('withdrawAmount').value='';}
function updateReferral(){if(currentUser) document.getElementById('referralLink').value=window.location.href+'?ref='+currentUser;}
function copyReferral(){document.getElementById('referralLink').select();document.execCommand('copy');alert('Referral link copied!');}
</script>
</body>
</html>
