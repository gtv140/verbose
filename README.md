<VERBOSE>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>VERBOSE — Premium Earning Platform</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
<style>
:root{--neon:#00ffff;--bg1:#0f0c29;--bg2:#302b63;--bg3:#24243e}
html,body{height:100%;margin:0;font-family:'Orbitron',sans-serif;background:#000;color:#fff;overflow-x:hidden}
body{background:linear-gradient(270deg,var(--bg1),var(--bg2),var(--bg3));background-size:600% 600%;animation:gradientBG 18s ease infinite}
@keyframes gradientBG{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
#app{max-width:1100px;margin:18px auto;padding:18px}
header{display:flex;align-items:center;justify-content:space-between;gap:12px}
header h1{margin:0;color:var(--neon);text-shadow:0 0 8px var(--neon)}
.small{font-size:13px;color:#cfe}
.card{background:rgba(0,255,255,0.04);border:1px solid rgba(0,255,255,0.12);padding:14px;border-radius:12px;box-shadow:0 6px 20px rgba(0,0,0,0.6);backdrop-filter:blur(6px)}
.layout{display:grid;grid-template-columns:320px 1fr;gap:18px;margin-top:18px}
nav{position:sticky;top:18px;height:fit-content;display:flex;flex-direction:column;gap:8px}
nav button{display:flex;align-items:center;gap:10px;padding:10px;border-radius:10px;border:none;background:linear-gradient(90deg,rgba(0,255,255,0.06),rgba(0,255,255,0.03));color:var(--neon);cursor:pointer}
nav button.active{box-shadow:0 0 18px rgba(0,255,255,0.18);transform:scale(1.01)}
main section{display:none}
main section.active{display:block}
input,select{width:100%;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.08);background:rgba(0,0,0,0.35);color:#fff;outline:none}
button.primary{background:var(--neon);color:#000;font-weight:700;border-radius:10px;padding:10px 12px;border:none;cursor:pointer}
button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);color:#fff;padding:10px;border-radius:8px}
.plan-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:12px}
.plan-card{border-radius:12px;padding:12px;background:linear-gradient(180deg, rgba(0,255,255,0.03), rgba(0,255,255,0.02));border:1px solid rgba(0,255,255,0.06);position:relative;transition:transform .18s,box-shadow .18s}
.plan-card:hover{transform:translateY(-8px);box-shadow:0 12px 40px rgba(0,255,255,0.06)}
.badge{position:absolute;top:10px;right:10px;background:linear-gradient(90deg,#ffea00,#ff9900);color:#000;padding:6px 8px;border-radius:8px;font-weight:800;box-shadow:0 6px 20px rgba(255,150,0,0.15)}
.muted{color:#bfe}
.row{display:flex;gap:10px;align-items:center;flex-wrap:wrap}
.notif{position:fixed;right:18px;bottom:18px;background:var(--neon);color:#000;padding:12px 18px;border-radius:12px;font-weight:700;box-shadow:0 10px 30px rgba(0,255,255,0.12)}
#bgCanvas{position:fixed;left:0;top:0;width:100%;height:100%;z-index:-1;pointer-events:none}
@media(max-width:900px){.layout{grid-template-columns:1fr}}
table{width:100%;border-collapse:collapse}
th,td{padding:8px;border-bottom:1px solid rgba(255,255,255,0.03);text-align:left;font-size:13px}
.success{color:#0f0}
</style>
</head>
<body>
<canvas id="bgCanvas"></canvas>

<div id="app">
<header>
<div>
<h1>VERBOSE</h1>
<div class="small">Owner: John Wilson • Launch Date: 17 Nov 2025</div>
</div>
<div id="topActions" class="row">
<div id="welcome" class="small muted">Not logged in</div>
<button id="openAuth" class="primary">Login / Signup</button>
</div>
</header>

<div class="layout">
<aside class="card">
<nav>
<button data-tab="dashboard" class="active">📊 Dashboard</button>
<button data-tab="plans">💼 Plans</button>
<button data-tab="deposit">💰 Deposit</button>
<button data-tab="withdrawal">💸 Withdrawal</button>
<button data-tab="transactions">🧾 Transactions</button>
<button data-tab="about">📑 Company</button>
<button id="logoutBtn" style="margin-top:10px;background:linear-gradient(90deg,#ff4d4d,#ff1a1a);color:#fff;display:none">Logout</button>
</nav>

<hr style="border:none;border-top:1px solid rgba(255,255,255,0.04);margin:12px 0"/>
<div class="small muted">Payment Numbers</div>
<div class="row" style="margin-top:8px">
<div class="card" style="padding:10px;width:100%">
<div style="display:flex;justify-content:space-between;align-items:center">
<div><strong>JazzCash</strong><div class="small muted">03705519562</div></div>
<button class="ghost" onclick="copyText('03705519562')">Copy</button>
</div>
<hr style="border:none;border-top:1px solid rgba(255,255,255,0.03);margin:8px 0"/>
<div style="display:flex;justify-content:space-between;align-items:center">
<div><strong>EasyPaisa</strong><div class="small muted">03379827882</div></div>
<button class="ghost" onclick="copyText('03379827882')">Copy</button>
</div>
</div>
</div>
</aside>

<main>
<section id="authSection" class="card active">
<h2 style="margin-top:0">Login / Signup</h2>
<div class="field"><input id="inpUser" placeholder="Username" /></div>
<div class="field"><input id="inpPass" type="password" placeholder="Password" /></div>
<div class="row">
<button class="primary" onclick="simpleLogin()">Login</button>
<button class="ghost" onclick="simpleSignup()">Signup</button>
</div>
<div class="small muted" style="margin-top:10px">Note: Signup data saved locally (single-user). Refresh will keep you logged in.</div>
</section>

<section id="dashboard" class="card">
<div style="display:flex;justify-content:space-between;align-items:center;gap:12px">
<div>
<h2 style="margin:0">Dashboard</h2>
<div class="small muted" id="dashSub">Overview</div>
</div>
<div class="row">
<div class="card" style="padding:8px;">
<div class="small muted">Balance</div>
<div style="font-weight:800;color:var(--neon);font-size:18px" id="balDisplay">0 PKR</div>
</div>
<button onclick="showTab('deposit')" class="primary">Deposit Now</button>
</div>
</div>
<hr style="border:none;border-top:1px solid rgba(255,255,255,0.04);margin:12px 0"/>
<div style="display:flex;gap:12px;flex-wrap:wrap">
<div class="card" style="flex:1;min-width:220px">
<h4 style="margin:0">Active Plans</h4>
<div id="activePlans" class="small muted" style="margin-top:8px">No active plans</div>
</div>
<div class="card" style="flex:1;min-width:220px">
<h4 style="margin:0">Achievements</h4>
<div id="achv" class="small muted" style="margin-top:8px">No badges yet</div>
</div>
</div>
</section>

<section id="plans" class="card">
<h2 style="margin-top:0">Plans</h2>
<div class="plan-grid" id="planGrid"></div>
</section>

<section id="deposit" class="card">
<h2 style="margin-top:0">Deposit</h2>
<div class="small muted">Select plan, send payment and upload proof.</div>
<div style="margin-top:12px">
<label class="small muted">Choose Plan</label>
<select id="planSelect" onchange="onPlanChange()"></select>
</div>
<div style="display:flex;gap:10px;margin-top:8px;flex-wrap:wrap">
<div style="flex:1">
<label class="small muted">Amount (auto)</label>
<input id="amountInput" readonly />
</div>
<div style="width:160px">
<label class="small muted">Method</label>
<select id="payMethod" onchange="onMethodChange()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
</div>
</div>
<div style="display:flex;gap:10px;margin-top:8px;align-items:center">
<input id="payNumber" readonly style="flex:1" />
<button class="ghost" onclick="copyCurrentNumber()">Copy</button>
</div>
<div style="margin-top:8px">
<input id="txId" placeholder="Transaction ID (optional)" />
</div>
<div style="margin-top:8px">
<input id="proof" type="file" />
</div>
<div style="display:flex;gap:10px;margin-top:10px">
<button class="primary" onclick="submitDeposit()">Submit Deposit</button>
<button class="ghost" onclick="backToDashboard()">Back</button>
</div>
</section>

<section id="withdrawal" class="card">
<h2 style="margin-top:0">Withdrawal</h2>
<div class="small muted">Request withdrawal (simulated).</div>
<div style="margin-top:12px">
<label class="small muted">Method</label>
<select id="withdrawMethod"><option value="jazzcash">JazzCash</option><option value="easypaisa">EasyPaisa</option></select>
</div>
<div style="margin-top:8px"><input id="withdrawAccount" placeholder="Account Number / Username" /></div>
<div style="margin-top:8px"><input id="withdrawAmount" placeholder="Amount PKR" type="number" /></div>
<div style="display:flex;gap:10px;margin-top:10px">
<button class="primary" onclick="submitWithdrawal()">Request Withdrawal</button>
<button class="ghost" onclick="backToDashboard()">Back</button>
</div>
</section>

<section id="transactions" class="card">
<h2 style="margin-top:0">Transactions</h2>
<div style="overflow:auto;max-height:320px">
<table id="txTable"><thead><tr><th>Type</th><th>Amount</th><th>Plan</th><th>Time</th><th>Status</th></tr></thead><tbody></tbody></table>
</div>
</section>

<section id="about" class="card">
<h2 style="margin-top:0">About VERBOSE</h2>
<p class="small muted">VERBOSE ek premium earning platform hai. Owner: <strong>John Wilson</strong>. Launch Date: <strong>17 Nov 2025</strong>.</p>
<div style="display:flex;gap:10px;margin-top:8px"><button class="primary" onclick="backToDashboard()">Back</button></div>
</section>
</main>
</div>
</div>

<div id="notifRoot"></div>

<script>
// neon particles
const canvas = document.getElementById('bgCanvas'), ctx = canvas.getContext('2d');
let W=canvas.width=innerWidth,H=canvas.height=innerHeight;
window.addEventListener('resize',()=>{W=canvas.width=innerWidth;H=canvas.height=innerHeight});
const parts=[];
for(let i=0;i<140;i++){parts.push({x:Math.random()*W,y:Math.random()*H,r:Math.random()*1.6+0.6,vx:(Math.random()-0.5)*0.6,vy:(Math.random()-0.5)*0.6,hue:180+Math.random()*60});}
function drawBG(){ctx.clearRect(0,0,W,H);parts.forEach(p=>{ctx.beginPath();ctx.fillStyle=`hsla(${p.hue},100%,60%,0.12)`;ctx.shadowBlur=12;ctx.shadowColor='rgba(0,255,255,0.15)';ctx.fillRect(p.x,p.y,p.r*2,p.r*2);p.x+=p.vx;p.y+=p.vy;if(p.x<0)p.x=W;if(p.x>W)p.x=0;if(p.y<0)p.y=H;if(p.y>H)p.y=0;});requestAnimationFrame(drawBG);}
drawBG();

// single-user auth
function getSavedUser(){try{return JSON.parse(localStorage.getItem('realUser')||'null');}catch(e){return null;}}
function setSavedUser(u){localStorage.setItem('realUser',JSON.stringify(u));}
function getCurrent(){try{return JSON.parse(localStorage.getItem('currentUser')||'null');}catch(e){return null;}}
function setCurrent(u){localStorage.setItem('currentUser',JSON.stringify(u));}

let realUser=getSavedUser(),currentUser=getCurrent(),transactions=JSON.parse(localStorage.getItem('verbose_tx')||'[]');

const navButtons=document.querySelectorAll('nav button[data-tab]');
navButtons.forEach(b=>b.addEventListener('click',()=>showTab(b.dataset.tab)));
document.getElementById('openAuth').addEventListener('click',()=>showTab('authSection'));
const logoutBtn=document.getElementById('logoutBtn');
logoutBtn.addEventListener('click',()=>{simpleLogout();});

// init view
function initView(){
if(currentUser){document.getElementById('welcome').innerText=`Hi, ${currentUser.username}`;document.getElementById('logoutBtn').style.display='block';showTab('dashboard');updateBalanceUI();}
else{document.getElementById('welcome').innerText='Not logged in';document.getElementById('logoutBtn').style.display='none';showTab('authSection');}
buildPlanGrid();populatePlanSelect();renderTransactions();
}
initView();

// AUTH
function simpleSignup(){
const u=document.getElementById('inpUser').value.trim();
const p=document.getElementById('inpPass').value.trim();
if(!u||!p){showNotif('Fill username & password');return;}
realUser={username:u,password:p,balance:0,activePlans:[]};
setSavedUser(realUser);showNotif('Signup saved. Now login.');document.getElementById('inpPass').value='';
}
function simpleLogin(){
const u=document.getElementById('inpUser').value.trim();
const p=document.getElementById('inpPass').value.trim();
if(!u||!p){showNotif('Enter credentials');return;}
const saved=getSavedUser();
if(!saved){showNotif('No account. Signup first');return;}
if(saved.username===u && saved.password===p){
currentUser=saved;setCurrent(currentUser);document.getElementById('welcome').innerText=`Hi, ${currentUser.username}`;document.getElementById('logoutBtn').style.display='block';updateBalanceUI();showNotif('Login successful');showTab('dashboard');renderTransactions();
}else{showNotif('Invalid credentials');}
}
function simpleLogout(){currentUser=null;localStorage.removeItem('currentUser');document.getElementById('welcome').innerText='Not logged in';document.getElementById('logoutBtn').style.display='none';showNotif('Logged out');showTab('authSection');}

// show tabs
function showTab(id){document.querySelectorAll('main section').forEach(s=>s.classList.remove('active'));document.getElementById(id)?.classList.add('active');navButtons.forEach(b=>b.classList.remove('active'));document.querySelector(`nav button[data-tab="${id}"]`)?document.querySelector(`nav button[data-tab="${id}"]`).classList.add('active'):null;}

// notifications
function showNotif(msg){const n=document.createElement('div');n.className='notif';n.innerText=msg;document.body.appendChild(n);setTimeout(()=>{n.remove();},2500);}

// balance & active plans
function updateBalanceUI(){if(!currentUser) return;document.getElementById('balDisplay').innerText=(currentUser.balance||0)+' PKR';document.getElementById('activePlans').innerText=(currentUser.activePlans&&currentUser.activePlans.length)?currentUser.activePlans.map(x=>x.name).join(', '):'No active plans';realUser=currentUser;setSavedUser(realUser);setCurrent(currentUser);}
function backToDashboard(){showTab('dashboard');}

// plans
const plans=[{name:'Plan 1',invest:250,days:25,totalProfit:900},{name:'Plan 2',invest:500,days:30,totalProfit:2000},{name:'Plan 3',invest:1000,days:35,totalProfit:4500},{name:'Plan 4',invest:1500,days:40,totalProfit:7000}];

// build plans grid
function buildPlanGrid(){
  const container=document.getElementById('planGrid');container.innerHTML='';
  plans.forEach(p=>{
    const div=document.createElement('div');div.className='plan-card';
    div.innerHTML=`<div class="badge">${p.invest} PKR</div>
      <h3>${p.name}</h3>
      <p class="small muted">Duration: ${p.days} days<br>Total Profit: ${p.totalProfit} PKR</p>
      <button class="primary" onclick="selectPlan('${p.name}')">Select Plan</button>`;
    container.appendChild(div);
  });
}

// populate select dropdown
function populatePlanSelect(){
  const sel=document.getElementById('planSelect');sel.innerHTML='';
  plans.forEach(p=>{const opt=document.createElement('option');opt.value=p.name;opt.innerText=p.name;sel.appendChild(opt);});
  onPlanChange();
}

// select plan
function selectPlan(name){document.getElementById('planSelect').value=name;onPlanChange();showTab('deposit');}

// update deposit amount and number
function onPlanChange(){const p=plans.find(x=>x.name===document.getElementById('planSelect').value);if(!p) return;document.getElementById('amountInput').value=p.invest;onMethodChange();}
function onMethodChange(){const method=document.getElementById('payMethod').value;const num=method==='jazzcash'?'03705519562':'03379827882';document.getElementById('payNumber').value=num;}
function copyText(txt){navigator.clipboard.writeText(txt);showNotif('Copied!');}
function copyCurrentNumber(){copyText(document.getElementById('payNumber').value);}

// deposit submit
function submitDeposit(){
  if(!currentUser){showNotif('Login first');return;}
  const planName=document.getElementById('planSelect').value;
  const amount=parseFloat(document.getElementById('amountInput').value);
  if(isNaN(amount)||amount<=0){showNotif('Invalid amount');return;}
  currentUser.balance=(currentUser.balance||0)+amount;
  currentUser.activePlans.push(plans.find(p=>p.name===planName));
  const tx={type:'Deposit',amount,plan:planName,time:new Date().toLocaleString(),status:'Success'};
  transactions.push(tx);localStorage.setItem('verbose_tx',JSON.stringify(transactions));
  updateBalanceUI();renderTransactions();showNotif('Deposit successful');backToDashboard();
}

// withdrawal
function submitWithdrawal(){
  if(!currentUser){showNotif('Login first');return;}
  const amt=parseFloat(document.getElementById('withdrawAmount').value);
  if(isNaN(amt)||amt<=0){showNotif('Enter valid amount');return;}
  if(amt>currentUser.balance){showNotif('Insufficient balance');return;}
  currentUser.balance-=amt;
  const tx={type:'Withdrawal',amount:amt,plan:'-',time:new Date().toLocaleString(),status:'Requested'};
  transactions.push(tx);localStorage.setItem('verbose_tx',JSON.stringify(transactions));
  updateBalanceUI();renderTransactions();showNotif('Withdrawal requested');backToDashboard();
}

// transactions table
function renderTransactions(){
  const tbody=document.querySelector('#txTable tbody');tbody.innerHTML='';
  transactions.slice().reverse().forEach(tx=>{
    const tr=document.createElement('tr');
    tr.innerHTML=`<td>${tx.type}</td><td>${tx.amount}</td><td>${tx.plan}</td><td>${tx.time}</td><td>${tx.status}</td>`;
    tbody.appendChild(tr);
  });
}
</script>
</body>
</html>
