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
<div style="display:flex;justify-content:space-between;">
<div>
<div id="welcomeText" style="font-weight:800;color:var(--neon)">Welcome —</div>
<div id="memberSince" class="muted">Member since —</div>
</div>
<div style="text-align:right;">
<div class="muted">Balance</div>
<div style="font-weight:900;font-size:18px">Rs <span id="balanceText">0</span></div>
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

</div>

<!-- NAV -->
<div class="nav">
<div onclick="nav('dashboardCard')">🏠 Home</div>
<div onclick="nav('plansCard')">📦 Plans</div>
</div>

<script>
const KEY_USER='verbose_user',KEY_BAL='verbose_bal_';
let currentUser=localStorage.getItem(KEY_USER)||null;
let plans=[]; let offerIntervals={};

// generate 7 special offers
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
localStorage.setItem(credKey,p); localStorage.setItem(KEY_BAL+u,'0');}
else{if(localStorage.getItem(credKey)!==p){alert('Wrong username/password');return;}}
localStorage.setItem(KEY_USER,u); currentUser=u; afterLoginUI();
}

function afterLoginUI(){nav('dashboardCard'); renderDashboard(); renderPlans(); startOffers();}

function doLogout(){localStorage.removeItem(KEY_USER);currentUser=null;nav('loginCard');Object.values(offerIntervals).forEach(i=>clearInterval(i));}

// NAV
function nav(cardId){['loginCard','dashboardCard','plansCard'].forEach(id=>document.getElementById(id).classList.add('hidden'));document.getElementById(cardId).classList.remove('hidden');renderDashboard();}

// DASHBOARD
function renderDashboard(){if(!currentUser)return;document.getElementById('welcomeText').innerText='Welcome, '+currentUser;
document.getElementById('memberSince').innerText='Member since: '+new Date().toLocaleDateString();
document.getElementById('balanceText').innerText=fmt(Number(localStorage.getItem(KEY_BAL+currentUser)||0));}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList');container.innerHTML='';
plans.forEach(plan=>{
const div=document.createElement('div');div.className='plan';
div.innerHTML=`<div class="meta"><div style="font-weight:800">${plan.name}</div>
<div class="muted" style="margin-top:4px">Invest: Rs ${fmt(plan.invest)} · Total: Rs ${fmt(plan.total)}</div>
${plan.offer?`<div class="countdown" id="countdown_${plan.id}">Loading timer...</div>`:''}</div>
<div class="actions">${plan.offer?'<button class="btn" onclick="buyPlan('+plan.id+')">Buy Now</button>':''}</div>`;container.appendChild(div);
});
}

// Offer countdown
function startOffers(){plans.forEach(plan=>{if(!plan.offer)return;
const key='verbose_offer_'+plan.id;let endTs=Number(localStorage.getItem(key)||0);if(!endTs||endTs<Date.now()){endTs=Date.now()+24*3600*1000;localStorage.setItem(key,endTs);}
const el=document.getElementById('countdown_'+plan.id);if(!el)return;
function tick(){const diff=Math.floor((endTs-Date.now())/1000);if(diff<=0){el.innerText='Offer ended';clearInterval(offerIntervals[plan.id]);return;}
const h=Math.floor(diff/3600),m=Math.floor((diff%3600)/60),s=diff%60;el.innerText=`${String(h).padStart(2,'0')}h:${String(m).padStart(2,'0')}m:${String(s).padStart(2,'0')}s`;}
tick();offerIntervals[plan.id]=setInterval(tick,1000);});}

// Buy plan -> credit balance
function buyPlan(id){if(!currentUser){alert('Login first');return;}
const plan=plans.find(p=>p.id===id);if(!plan)return;let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);bal+=plan.invest;localStorage.setItem(KEY_BAL+currentUser,bal);alert(`Plan ${plan.name} purchased! Rs ${fmt(plan.invest)} added.`);renderDashboard();}
</script>
</body>
</html>
