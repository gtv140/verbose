<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
:root{
--bg:#0d1117;
--blue:#1f6feb;
--muted:#8b949e;
--card:#161b22;
--success:#2ea043;
--alert:#ff7b72;
}
body{margin:0;font-family:sans-serif;background:var(--bg);color:#c9d1d9;}
header{padding:15px;text-align:center;font-size:24px;font-weight:bold;color:var(--blue);}
.wrap{max-width:500px;margin:10px auto;padding:10px;}
.card{background:var(--card);padding:12px;margin-bottom:12px;border-radius:10px;}
input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:6px;border:1px solid #30363d;background:transparent;color:#c9d1d9;}
button{cursor:pointer;background:var(--blue);border:none;color:white;font-weight:bold;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:#161b22;border-top:1px solid #30363d;}
.nav div{color:#c9d1d9;text-align:center;font-size:13px;cursor:pointer;}
.plan{display:flex;justify-content:space-between;align-items:center;padding:8px;border-radius:8px;margin-bottom:6px;background:#21262d;}
.plan .meta{flex:1;font-size:14px;}
.plan button{width:100px;font-size:13px;padding:4px;}
.referral-box{display:flex;gap:6px;margin-bottom:10px;}
.referral-box input{flex:1;}
.admin-box div{margin-top:6px;padding:8px;background:#21262d;border-radius:6px;text-align:center;}
.hidden{display:none;}
.alert-note{background:rgba(255,123,114,0.1);color:var(--alert);padding:10px;border-radius:6px;margin-bottom:10px;text-align:center;}
.success-note{background:rgba(46,160,67,0.1);color:var(--success);padding:10px;border-radius:6px;margin-bottom:10px;text-align:center;}
</style>
</head>
<body>
<header>VERBOSE</header>
<div class="wrap">

<!-- LOGIN -->
<div id="loginCard" class="card">
<h3>Login / Signup</h3>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">New User</option>
</select>
<input id="inputUser" placeholder="Username"/>
<input id="inputPass" placeholder="Password" type="password"/>
<input id="referralInput" placeholder="Referral Code (optional)"/>
<button onclick="doAuth()">Submit</button>
<p style="font-size:12px;color:var(--muted)">Use same browser/device. Data stored locally.</p>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<div class="alert-note">⚠️ Any deposit/withdrawal issue? Contact administration immediately.</div>
<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;">
<div>
<div id="welcomeText" style="font-weight:bold;"></div>
<div id="memberSince" style="color:var(--muted);font-size:12px;"></div>
</div>
<div style="text-align:right;">
<div style="font-size:12px;color:var(--muted)">Balance</div>
<div style="font-weight:bold;font-size:16px">Rs <span id="balanceText">0</span></div>
<div style="margin-top:4px;"><button onclick="doLogout()" style="width:auto;font-size:12px;padding:4px;">Logout</button></div>
</div>
</div>

<div class="referral-box">
<input id="referralLink" readonly/>
<button onclick="copyReferral()">Copy Link</button>
</div>

<div class="admin-box">
<div>
<strong>Admin Support</strong><br>
WhatsApp: <a href="https://wa.me/03705519562" target="_blank">03705519562</a><br>
Email: <a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a>
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
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="depositNumber" readonly/>
<input id="depositAmount" readonly/>
<input id="depositTx" placeholder="Transaction ID"/>
<input type="file" id="depositProof"/>
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- NAV -->
<div class="nav">
<div onclick="nav('dashboardCard')"><i class="fas fa-home"></i><br>Home</div>
<div onclick="nav('plansCard')"><i class="fas fa-gift"></i><br>Plans</div>
<div onclick="nav('depositCard')"><i class="fas fa-wallet"></i><br>Deposit</div>
</div>

<script>
// STORAGE KEYS
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
const KEY_USER_PLANS='verbose_plans_';
let currentUser=localStorage.getItem(KEY_USER)||null;
let plans=[];

// CREATE PLANS
for(let i=1;i<=7;i++){
plans.push({id:i,name:'Special '+i,invest:200*i,multiplier:2,total:200*i*2,days:20+Math.floor(Math.random()*51),offer:true});
}
for(let i=8;i<=32;i++){
let invest=3000 + (i-8)*1000; let days=20+Math.floor(Math.random()*51);
plans.push({id:i,name:'Plan '+(i-7),invest:invest,multiplier:2.5,total:invest*2.5,days:days,offer:false});
}
for(let i=33;i<=37;i++){
plans.push({id:i,name:'Coming Soon',invest:0,multiplier:0,total:0,days:0,offer:false});
}

function fmt(n){return Number(n).toLocaleString();}

// AUTH
function doAuth(){
const mode=document.getElementById('authMode').value;
const u=document.getElementById('inputUser').value.trim();
const p=document.getElementById('inputPass').value.trim();
const ref=document.getElementById('referralInput').value.trim();
if(!u||!p){alert('Enter username & password'); return;}
const credKey='verbose_cred_'+u;
if(mode==='signup'){
if(localStorage.getItem(credKey)){alert('Username exists');return;}
localStorage.setItem(credKey,p);
localStorage.setItem(KEY_BAL+u,'0');
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
function copyReferral(){const link=document.getElementById('referralLink');link.select();document.execCommand('copy');alert('Referral copied!');}
function doLogout(){localStorage.removeItem(KEY_USER);currentUser=null;nav('loginCard');}

// NAV
function nav(cardId){
['loginCard','dashboardCard','plansCard','depositCard'].forEach(id=>document.getElementById(id).classList.add('hidden'));
document.getElementById(cardId).classList.remove('hidden');
renderDashboard(); fillDeposit();
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
const container=document.getElementById('plansList'); container.innerHTML='';
plans.forEach(plan=>{
const div=document.createElement('div'); div.className='plan';
const daily=Math.round(plan.total/plan.days);
div.innerHTML=`<div class="meta"><strong>${plan.name}</strong><br>Invest: Rs ${fmt(plan.invest)} · Total: Rs ${fmt(plan.total)} · Days: ${plan.days} · Daily: Rs ${fmt(daily)}</div>
${plan.offer?`<div id="countdown_${plan.id}" style="font-size:12px;color:var(--muted);margin-top:2px;">Loading...</div>`:''}
<button onclick="buyPlan(${plan.id})">Buy Now</button>`;
container.appendChild(div);

if(plan.offer){
let end=Date.now()+24*3600*1000;
let el=document.getElementById('countdown_'+plan.id);
let interval=setInterval(()=>{
let diff=Math.floor((end-Date.now())/1000);
if(diff<=0){el.innerText='Offer ended'; clearInterval(interval); return;}
let h=Math.floor(diff/3600), m=Math.floor((diff%3600)/60), s=diff%60;
el.innerText=`${h}h:${m}m:${s}s`;
},1000);
}
});
}

// BUY PLAN
function buyPlan(id){
if(!currentUser){alert('Login first'); return;}
const plan=plans.find(p=>p.id===id);
document.getElementById('depositAmount').value=plan.invest;
updateDepositNumber(); nav('depositCard'); alert(`Plan ${plan.name} selected. Submit your deposit.`);}
function updateDepositNumber(){const method=document.getElementById('depositMethod').value; document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';}
function fillDeposit(){if(!currentUser) return; document.getElementById('depositAmount').value=''; updateDepositNumber();}

// DEPOSIT
function submitDeposit(){
if(!currentUser){alert('Login first'); return;}
const tx=document.getElementById('depositTx').value.trim();
const proof=document.getElementById('depositProof').files[0];
const amount=Number(document.getElementById('depositAmount').value)||0;
if(!tx||!proof||!amount){alert('All fields required'); return;}
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
bal+=amount;
localStorage.setItem(KEY_BAL+currentUser,bal);
alert('Deposit submitted! Balance updated.');
document.getElementById('depositTx').value='';
document.getElementById('depositProof').value='';
nav('dashboardCard');
}
</script>
</body>
</html>
