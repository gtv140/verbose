<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>VERBOSE — Premium Earning Platform</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
<style>
:root{--neon:#00ffff;--bg1:#0f0c29;--bg2:#302b63;--bg3:#24243e;}
*{box-sizing:border-box;}
html,body{height:100%;margin:0;font-family:'Orbitron',sans-serif;background:#000;color:#fff;overflow-x:hidden;}
body{background:linear-gradient(270deg,var(--bg1),var(--bg2),var(--bg3));background-size:600% 600%;animation:gradientBG 18s ease infinite;}
@keyframes gradientBG{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
#bgCanvas{position:fixed;left:0;top:0;width:100%;height:100%;z-index:-1;pointer-events:none;}
#app{max-width:1200px;margin:18px auto;padding:18px;}
header{display:flex;align-items:center;justify-content:space-between;gap:12px;}
header h1{margin:0;color:var(--neon);text-shadow:0 0 12px var(--neon);}
.small{font-size:13px;color:#cfe;}
.card{background:rgba(0,255,255,0.04);border:1px solid rgba(0,255,255,0.12);padding:14px;border-radius:12px;box-shadow:0 6px 20px rgba(0,0,0,0.6);backdrop-filter:blur(6px);}
.layout{display:grid;grid-template-columns:320px 1fr;gap:18px;margin-top:18px;}
nav{position:sticky;top:18px;height:fit-content;display:flex;flex-direction:column;gap:8px;}
nav button{display:flex;align-items:center;gap:10px;padding:10px;border-radius:10px;border:none;background:linear-gradient(90deg,rgba(0,255,255,0.06),rgba(0,255,255,0.03));color:var(--neon);cursor:pointer;}
nav button.active{box-shadow:0 0 18px rgba(0,255,255,0.18);transform:scale(1.01);}
main section{display:none;}
main section.active{display:block;}
input,select,textarea{width:100%;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.08);background:rgba(0,0,0,0.35);color:#fff;outline:none;}
button.primary{background:var(--neon);color:#000;font-weight:700;border-radius:10px;padding:10px 12px;border:none;cursor:pointer;}
button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);color:#fff;padding:10px;border-radius:8px;}
.plan-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:12px;margin-top:10px;}
.plan-card{border-radius:12px;padding:12px;background:linear-gradient(180deg, rgba(0,255,255,0.03), rgba(0,255,255,0.02));border:1px solid rgba(0,255,255,0.06);position:relative;transition:transform .18s,box-shadow .18s;}
.plan-card:hover{transform:translateY(-8px);box-shadow:0 12px 40px rgba(0,255,255,0.06);}
.badge{position:absolute;top:10px;right:10px;background:linear-gradient(90deg,#ffea00,#ff9900);color:#000;padding:6px 8px;border-radius:8px;font-weight:800;box-shadow:0 6px 20px rgba(255,150,0,0.15);}
.muted{color:#bfe;}
.row{display:flex;gap:10px;align-items:center;flex-wrap:wrap;}
.notif{position:fixed;right:18px;bottom:18px;background:var(--neon);color:#000;padding:12px 18px;border-radius:12px;font-weight:700;box-shadow:0 10px 30px rgba(0,255,255,0.12);}
@media(max-width:900px){.layout{grid-template-columns:1fr}}
table{width:100%;border-collapse:collapse;}
th,td{padding:8px;border-bottom:1px solid rgba(255,255,255,0.03);text-align:left;font-size:13px;}
.success{color:#0f0;}
.center{display:flex;justify-content:center;align-items:center;gap:8px;}
.smallbtn{padding:6px 10px;border-radius:8px;border:none;cursor:pointer;}
#userBalance{font-weight:800;color:var(--neon);font-size:18px;margin:0 12px;}
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
<div id="welcome" class="small muted"></div>
<button id="openAuth" class="primary">Login / Signup</button>
<div id="userBalance"></div>
<button id="logoutBtn" class="primary" style="display:none;background:linear-gradient(90deg,#ff4d4d,#ff1a1a);">Logout</button>
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
<button data-tab="adminPanel">🛠 Admin Panel</button>
<button data-tab="about">📑 Company</button>
</nav>
</aside>
<main>
<section id="authSection" class="card active">
<h2 style="margin-top:0">Login / Signup</h2>
<div class="field"><input id="inpUser" placeholder="Username" /></div>
<div class="field"><input id="inpPass" type="password" placeholder="Password" /></div>
<div class="row" style="margin-top:8px">
<button class="primary" onclick="simpleLogin()">Login</button>
<button class="ghost" onclick="simpleSignup()">Signup</button>
</div>
</section>
<section id="dashboard" class="card">
<h2>Dashboard</h2>
<div class="row" style="margin-top:8px;flex-wrap:wrap;">
<div class="card" style="padding:12px;">
<div>Balance</div>
<div id="balDisplay" style="font-weight:800;color:var(--neon);font-size:18px">0 PKR</div>
</div>
<div class="card" style="padding:12px;">
<div>Total Profit</div>
<div id="profitDisplay" style="font-weight:800;color:var(--neon);font-size:18px">0 PKR</div>
</div>
</div>
<div style="margin-top:12px">
<h4>Active Plans</h4>
<div id="activePlans" class="small muted">No active plans</div>
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
<div style="margin-top:8px"><input id="txId" placeholder="Transaction ID (optional)" /></div>
<div style="margin-top:8px"><input id="proof" type="file" /></div>
<div style="display:flex;gap:10px;margin-top:10px">
<button class="primary" onclick="submitDeposit()">Submit Deposit</button>
<button class="ghost" onclick="showTab('dashboard')">Back</button>
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
<button class="ghost" onclick="showTab('dashboard')">Back</button>
</div>
</section>
<section id="transactions" class="card">
<h2 style="margin-top:0">Transactions</h2>
<div style="overflow:auto;max-height:320px">
<table id="txTable">
<thead><tr><th>Type</th><th>Amount</th><th>Plan</th><th>Time</th><th>Status</th></tr></thead>
<tbody></tbody>
</table>
</div>
</section>
<section id="adminPanel" class="card">
<h2 style="margin-top:0">Admin Panel</h2>
<div class="small muted">Manage users & transactions.</div>
<div style="margin-top:8px">
<h4>Users</h4>
<table id="userTable" style="width:100%;border-collapse:collapse">
<thead><tr><th>Username</th><th>Balance</th><th>Active Plans</th><th>Action</th></tr></thead>
<tbody></tbody>
</table>
</div>
<div style="margin-top:12px">
<h4>Transactions</h4>
<table id="adminTxTable" style="width:100%;border-collapse:collapse">
<thead><tr><th>User</th><th>Type</th><th>Amount</th><th>Plan</th><th>Time</th><th>Status</th><th>Action</th></tr></thead>
<tbody></tbody>
</table>
</div>
</section>
<section id="about" class="card">
<h2 style="margin-top:0">About VERBOSE</h2>
<p class="small muted">
VERBOSE ek premium earning platform hai jo modern digital finance ko neon style me present karta hai.
Owner: <strong>John Wilson</strong>. Launch Date: <strong>17 Nov 2025</strong>.
Mission: Financial freedom, transparent plans, aur easy UX.
</p>
</section>
</main>
</div>
<div id="notifRoot"></div>
<script>
// Neon Background
const canvas=document.getElementById('bgCanvas'),ctx=canvas.getContext('2d');
let W=canvas.width=innerWidth,H=canvas.height=innerHeight;
window.addEventListener('resize',()=>{W=canvas.width=innerWidth;H=canvas.height=innerHeight;});
const particles=[];
for(let i=0;i<200;i++){particles.push({x:Math.random()*W,y:Math.random()*H,r:Math.random()*2+0.5,vx:(Math.random()-0.5)*0.8,vy:(Math.random()-0.5)*0.8,hue:180+Math.random()*60});}
function drawBG(){ctx.clearRect(0,0,W,H);particles.forEach(p=>{ctx.beginPath();ctx.fillStyle=`hsla(${p.hue},100%,60%,0.12)`;ctx.shadowBlur=14;ctx.shadowColor=`hsla(${p.hue},100%,60%,0.3)`;ctx.fillRect(p.x,p.y,p.r*2,p.r*2);p.x+=p.vx;p.y+=p.vy;if(p.x<0)p.x=W;if(p.x>W)p.x=0;if(p.y<0)p.y=H;if(p.y>H)p.y=0;});requestAnimationFrame(drawBG);}
drawBG();

// Admin Credentials
const adminCreds={user:'AdminKhan',pass:'SuperSecret123'};
let users=JSON.parse(localStorage.getItem('verbose_users')||'[]');
let currentUser=JSON.parse(localStorage.getItem('verbose_current')||'null');
let transactions=JSON.parse(localStorage.getItem('verbose_tx')||'[]');

// Plans: 25 total, 5 special offers
const plans=[];
const specialIds=[1,2,3,4,5];
for(let i=1;i<=25;i++){
  let days=Math.max(25+i,90); // minimum 90
  let invest=250*i;
  let totalProfit = specialIds.includes(i)? invest*4.8 : invest*2.5; // special 24hr offer or normal 2.5x
  let dailyProfit = Math.round(totalProfit/days);
  plans.push({id:i,name:`Plan ${i}`,invest,days,totalProfit,dailyProfit,offer:specialIds.includes(i)});
}

// Elements
const navButtons=document.querySelectorAll('nav button[data-tab]');
navButtons.forEach(btn=>btn.addEventListener('click',()=>showTab(btn.dataset.tab)));
document.getElementById('openAuth').addEventListener('click',()=>showTab('authSection'));
const logoutBtn=document.getElementById('logoutBtn');
logoutBtn.addEventListener('click',simpleLogout);

// Helper Functions
function saveAll(){localStorage.setItem('verbose_users',JSON.stringify(users));localStorage.setItem('verbose_current',JSON.stringify(currentUser));localStorage.setItem('verbose_tx',JSON.stringify(transactions));}
function showNotif(msg){const n=document.createElement('div');n.className='notif';n.innerText=msg;document.getElementById('notifRoot').appendChild(n);setTimeout(()=>n.remove(),2500);}
function showTab(tab){navButtons.forEach(b=>b.classList.remove('active'));document.querySelector(`nav button[data-tab="${tab}"]`)?.classList.add('active');document.querySelectorAll('main section').forEach(s=>s.classList.remove('active'));document.getElementById(tab)?.classList.add('active');}
function copyText(txt){navigator.clipboard.writeText(txt);showNotif('Copied!');}
function renderPlans(){const grid=document.getElementById('planGrid');grid.innerHTML='';plans.forEach(p=>{const div=document.createElement('div');div.className='plan-card';div.innerHTML=`<h4>${p.name}</h4><div>Invest: ${p.invest} PKR</div><div>Days: ${p.days}</div><div>Daily Profit: ${p.dailyProfit} PKR</div><div>Total Profit: ${p.totalProfit} PKR</div>${p.offer?'<div class="badge">Offer</div>':''}`;grid.appendChild(div);});}
function renderPlanDropdown(){const sel=document.getElementById('planSelect');sel.innerHTML='';plans.forEach(p=>{const opt=document.createElement('option');opt.value=p.id;opt.innerText=`${p.name} — ${p.invest} PKR`;sel.appendChild(opt);});onPlanChange();}
function onPlanChange(){const planId=parseInt(document.getElementById('planSelect').value);const plan=plans.find(p=>p.id===planId);if(plan) document.getElementById('amountInput').value=plan.invest;onMethodChange();}
function onMethodChange(){const method=document.getElementById('payMethod').value;document.getElementById('payNumber').value=(method==='jazzcash')?'03705519562':'03379827882';}
function copyCurrentNumber(){copyText(document.getElementById('payNumber').value);}

// Login / Signup
function simpleSignup(){const u=document.getElementById('inpUser').value.trim();const p=document.getElementById('inpPass').value;if(!u||!p){showNotif('Enter username & password');return;}if(users.find(x=>x.user===u)){showNotif('Username exists');return;}users.push({user:u,pass:p,balance:0,activePlans:[],profit:0});saveAll();showNotif('Signup successful!');simpleLogin(true);}
function simpleLogin(skipNotif=false){const u=document.getElementById('inpUser').value.trim();const p=document.getElementById('inpPass').value;if(!u||!p){showNotif('Enter username & password');return;}if(u===adminCreds.user && p===adminCreds.pass){currentUser={user:u,admin:true};saveAll();showNotif('Admin logged in!');renderDashboard();return;}const user = users.find(x=>x.user===u && x.pass===p);if(!user){showNotif('Invalid username or password'); return;} 
currentUser = {...user, admin:false}; 
saveAll(); 
if(!skipNotif) showNotif('Login successful!'); 
renderDashboard();
}

function simpleLogout(){
  currentUser = null;
  saveAll();
  document.getElementById('welcome').innerText = '';
  document.getElementById('userBalance').innerText = '';
  logoutBtn.style.display = 'none';
  showTab('authSection');
}

// Render Dashboard
function renderDashboard(){
  if(!currentUser) return;
  document.getElementById('welcome').innerText = 'Welcome, '+currentUser.user;
  document.getElementById('userBalance').innerText = 'Balance: '+currentUser.balance+' PKR';
  logoutBtn.style.display = 'inline-block';
  renderActivePlans();
  renderPlans();
  renderPlanDropdown();
  showTab('dashboard');
}

// Active Plans
function renderActivePlans(){
  if(!currentUser || !currentUser.activePlans || currentUser.activePlans.length==0){
    document.getElementById('activePlans').innerText='No active plans';
    return;
  }
  const ap = currentUser.activePlans.map(p=>`${p.name} — ${p.days} days, Daily: ${p.dailyProfit} PKR`).join('\n');
  document.getElementById('activePlans').innerText = ap;
}

// Deposit
function submitDeposit(){
  if(!currentUser) return showNotif('Login first');
  const planId = parseInt(document.getElementById('planSelect').value);
  const plan = plans.find(p=>p.id===planId);
  if(!plan) return;
  const deposit = {user:currentUser.user,type:'Deposit',amount:plan.invest,plan:plan.name,time:new Date().toLocaleString(),status:'Pending'};
  transactions.push(deposit);
  currentUser.balance += plan.invest; // auto add for simulation
  currentUser.activePlans.push({...plan});
  saveAll();
  showNotif('Deposit submitted & plan activated!');
  renderDashboard();
}

// Withdrawal
function submitWithdrawal(){
  if(!currentUser) return showNotif('Login first');
  const amt=parseFloat(document.getElementById('withdrawAmount').value);
  if(isNaN(amt) || amt<=0) return showNotif('Enter valid amount');
  if(amt>currentUser.balance) return showNotif('Insufficient balance');
  const method = document.getElementById('withdrawMethod').value;
  currentUser.balance -= amt;
  const wd={user:currentUser.user,type:'Withdrawal',amount:amt,plan:'-',time:new Date().toLocaleString(),status:'Processed',method};
  transactions.push(wd);
  saveAll();
  showNotif('Withdrawal processed!');
  renderDashboard();
}

// Render transactions table
function renderTransactions(){
  const tbody = document.getElementById('txTable').querySelector('tbody');
  tbody.innerHTML = '';
  transactions.filter(t=>!currentUser.admin || t.user==currentUser.user).forEach(t=>{
    const tr=document.createElement('tr');
    tr.innerHTML=`<td>${t.type}</td><td>${t.amount}</td><td>${t.plan}</td><td>${t.time}</td><td>${t.status}</td>`;
    tbody.appendChild(tr);
  });
}

// Admin Panel
function renderAdmin(){
  if(!currentUser || !currentUser.admin) return;
  const tbody = document.getElementById('userTable').querySelector('tbody');
  tbody.innerHTML='';
  users.forEach(u=>{
    const tr=document.createElement('tr');
    tr.innerHTML=`<td>${u.user}</td><td>${u.balance}</td><td>${u.activePlans.length}</td><td><button class="smallbtn" onclick="deleteUser('${u.user}')">Delete</button></td>`;
    tbody.appendChild(tr);
  });
  const txbody = document.getElementById('adminTxTable').querySelector('tbody');
  txbody.innerHTML='';
  transactions.forEach(tx=>{
    const tr=document.createElement('tr');
    tr.innerHTML=`<td>${tx.user}</td><td>${tx.type}</td><td>${tx.amount}</td><td>${tx.plan}</td><td>${tx.time}</td><td>${tx.status}</td><td><button class="smallbtn" onclick="deleteTx('${tx.user}','${tx.time}')">Delete</button></td>`;
    txbody.appendChild(tr);
  });
}

function deleteUser(username){
  users = users.filter(u=>u.user!==username);
  transactions = transactions.filter(t=>t.user!==username);
  saveAll();
  renderAdmin();
  showNotif('User deleted');
}

function deleteTx(user,time){
  transactions = transactions.filter(t=>!(t.user===user && t.time===time));
  saveAll();
  renderAdmin();
  showNotif('Transaction deleted');
}

// Initial Render
if(currentUser) renderDashboard();
renderPlans();
renderPlanDropdown();
renderTransactions();
</script>
</body>
</html>
