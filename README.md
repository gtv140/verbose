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
<thead><tr><th>Type</th><th>Amount</th><th>Plan</th><th>Days</th><th>Total Profit</th><th>Status</th></tr></thead>
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
<thead><tr><th>User</th><th>Type</th><th>Amount</th><th>Plan</th><th>Days</th><th>Total Profit</th><th>Status</th><th>Action</th></tr></thead>
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

// Plans: 25 plans
const plans=[
{ id:1, name:'Plan 1', days:25, invest:250, dailyProfit:48, totalProfit:1200, offer:true },
{ id:2, name:'Plan 2', days:27, invest:500, dailyProfit:89, totalProfit:2400, offer:true },
{ id:3, name:'Plan 3', days:29, invest:750, dailyProfit:124, totalProfit:3600, offer:true },
{ id:4, name:'Plan 4', days:31, invest:1000, dailyProfit:155, totalProfit:4800, offer:true },
{ id:5, name:'Plan 5', days:33, invest:1500, dailyProfit:218, totalProfit:7200, offer:true },
{ id:6, name:'Plan 6', days:35, invest:2000, dailyProfit:143, totalProfit:5000, offer:false },
{ id:7, name:'Plan 7', days:37, invest:2500, dailyProfit:169, totalProfit:6250, offer:false },
{ id:8, name:'Plan 8', days:39, invest:3000, dailyProfit:192, totalProfit:7500, offer:false },
{ id:9, name:'Plan 9', days:41, invest:3500, dailyProfit:213, totalProfit:8750, offer:false },
{ id:10, name:'Plan 10', days:43, invest:4000, dailyProfit:232, totalProfit:10000, offer:false },
{ id:11, name:'Plan 11', days:45, invest:5000, dailyProfit:278, totalProfit:12500, offer:false },
{ id:12, name:'Plan 12', days:47, invest:6000, dailyProfit:319, totalProfit:15000, offer:false },
{ id:13, name:'Plan 13', days:49, invest:7000, dailyProfit:357, totalProfit:17500, offer:false },
{ id:14, name:'Plan 14', days:51, invest:8000, dailyProfit:392, totalProfit:20000, offer:false },
{ id:15, name:'Plan 15', days:53, invest:9000, dailyProfit:424, totalProfit:22500, offer:false },
{ id:16, name:'Plan 16', days:55, invest:10000, dailyProfit:455, totalProfit:25000, offer:false },
{ id:17, name:'Plan 17', days:57, invest:12000, dailyProfit:526, totalProfit:30000, offer:false },
{ id:18, name:'Plan 18', days:59, invest:14000, dailyProfit:593, totalProfit:35000, offer:false },
{ id:19, name:'Plan 19', days:61, invest:16000, dailyProfit:655, totalProfit:40000, offer:false },
{ id:20, name:'Plan 20', days:63, invest:18000, dailyProfit:714, totalProfit:45000, offer:false },
{ id:21, name:'Plan 21', days:65, invest:20000, dailyProfit:769, totalProfit:50000, offer:false },
{ id:22, name:'Plan 22', days:67, invest:25000, dailyProfit:932, totalProfit:62500, offer:false },
{ id:23, name:'Plan 23', days:69, invest:30000, dailyProfit:1087, totalProfit:75000, offer:false },
{ id:24, name:'Plan 24', days:70, invest:40000, dailyProfit:1428, totalProfit:100000, offer:false },
{ id:25, name:'Plan 25', days:70, invest:60000, dailyProfit:2142, totalProfit:150000, offer:false }
];

// Elements
const navButtons=document.querySelectorAll('nav button[data-tab]');
navButtons.forEach(btn=>btn.addEventListener('click',()=>showTab(btn.dataset.tab)));
document.getElementById('openAuth').addEventListener('click',()=>showTab('authSection'));
const logoutBtn=document.getElementById('logoutBtn');
logoutBtn.addEventListener('click',simpleLogout);

// Helper Functions
function saveAll(){localStorage.setItem('verbose_users',JSON.stringify(users));localStorage.setItem('verbose_current',JSON.stringify(currentUser));localStorage.setItem('verbose_tx',JSON.stringify(transactions));}
function showNotif(msg){const n=document.createElement('div');n.className='notif'; n.textContent=msg; document.body.appendChild(n); setTimeout(()=>n.remove(),3000);}

// Tab switching
function showTab(tab){
    document.querySelectorAll('main section').forEach(s=>s.classList.remove('active'));
    const el=document.getElementById(tab);
    if(el) el.classList.add('active');
    navButtons.forEach(b=>b.classList.remove('active'));
    const btn=document.querySelector(`nav button[data-tab="${tab}"]`);
    if(btn) btn.classList.add('active');
}

// Render plans
function renderPlans(){
    const grid=document.getElementById('planGrid');
    const select=document.getElementById('planSelect');
    grid.innerHTML=''; select.innerHTML='';
    plans.forEach(p=>{
        const card=document.createElement('div'); card.className='plan-card';
        card.innerHTML=`<h4>${p.name}</h4>
        <div>Days: ${p.days}</div>
        <div>Invest: ${p.invest} PKR</div>
        <div>Daily Profit: ${p.dailyProfit} PKR</div>
        <div>Total Profit: ${p.totalProfit} PKR</div>
        ${p.offer?'<div class="badge">Offer</div>':''}`;
        grid.appendChild(card);
        const opt=document.createElement('option'); opt.value=p.id; opt.textContent=`${p.name} (${p.invest} PKR)`; select.appendChild(opt);
    });
    onPlanChange();
}
function onPlanChange(){
    const pid=parseInt(document.getElementById('planSelect').value);
    const plan=plans.find(p=>p.id===pid);
    if(plan) document.getElementById('amountInput').value=plan.invest;
    onMethodChange();
}
function onMethodChange(){
    const method=document.getElementById('payMethod').value;
    const number=(method==='jazzcash')?'03001234567':'03111234567';
    document.getElementById('payNumber').value=number;
}
function copyCurrentNumber(){navigator.clipboard.writeText(document.getElementById('payNumber').value);showNotif('Number Copied');}

// Login / Signup
function simpleLogin(){
    const u=document.getElementById('inpUser').value.trim();
    const p=document.getElementById('inpPass').value.trim();
    if(u===adminCreds.user && p===adminCreds.pass){currentUser={user:u,admin:true,balance:0,plans:[]};saveAll();showNotif('Admin Logged In');updateUI();return;}
    const usr=users.find(x=>x.user===u && x.pass===p);
    if(usr){currentUser=usr;saveAll();showNotif('Logged In');updateUI();}else showNotif('Invalid credentials');
}
function simpleSignup(){
    const u=document.getElementById('inpUser').value.trim();
    const p=document.getElementById('inpPass').value.trim();
    if(!u||!p){showNotif('Enter username & password');return;}
    if(users.find(x=>x.user===u)){showNotif('Username exists');return;}
    const newUser={user:u,pass:p,balance:0,plans:[]};
    users.push(newUser);currentUser=newUser;saveAll();showNotif('Signup Successful');updateUI();
}
function simpleLogout(){currentUser=null;saveAll();updateUI();showTab('authSection');}

// Update UI
function updateUI(){
    document.getElementById('welcome').textContent=currentUser?`Welcome, ${currentUser.user}`:'';
    document.getElementById('userBalance').textContent=currentUser?`${currentUser.balance} PKR`:'';
    logoutBtn.style.display=currentUser?'inline-block':'none';
    renderActivePlans();
    renderTransactions();
    renderAdmin();
    renderPlans();
}

// Deposit
function submitDeposit(){
    if(!currentUser || currentUser.admin){showNotif('Login as user');return;}
    const pid=parseInt(document.getElementById('planSelect').value);
    const plan=plans.find(p=>p.id===pid);
    const amount=parseFloat(document.getElementById('amountInput').value);
    if(!plan || amount<=0){showNotif('Invalid plan');return;}
    // Add to user plans
    currentUser.plans.push({planId:plan.id,start:Date.now(),days:plan.days,dailyProfit:plan.dailyProfit,totalProfit:plan.totalProfit});
    currentUser.balance+=0; // No instant add
    transactions.push({user:currentUser.user,type:'Deposit',amount:amount,plan:plan.name,days:plan.days,totalProfit:plan.totalProfit,status:'Completed'});
    saveAll(); showNotif('Deposit Successful'); updateUI(); showTab('dashboard');
}

// Withdrawal
function submitWithdrawal(){
    if(!currentUser || currentUser.admin){showNotif('Login as user');return;}
    const amt=parseFloat(document.getElementById('withdrawAmount').value);
    if(amt<=0 || amt>currentUser.balance){showNotif('Invalid amount');return;}
    currentUser.balance-=amt;
    transactions.push({user:currentUser.user,type:'Withdrawal',amount:amt,plan:'-',days:'-',totalProfit:'-',status:'Completed'});
    saveAll(); showNotif('Withdrawal Requested'); updateUI(); showTab('dashboard');
}

// Render Active Plans
function renderActivePlans(){
    const container=document.getElementById('activePlans'); container.innerHTML='';
    if(!currentUser || currentUser.admin || currentUser.plans.length===0){container.textContent='No active plans'; return;}
    currentUser.plans.forEach(p=>{
        const pl=plans.find(x=>x.id===p.planId);
        const div=document.createElement('div');
        div.innerHTML=`<strong>${pl.name}</strong> | Days: ${p.days} | Total Profit: ${p.totalProfit} PKR`;
        container.appendChild(div);
    });
}

// Render Transactions
function renderTransactions(){
    const tbody=document.querySelector('#txTable tbody'); tbody.innerHTML='';
    if(!currentUser) return;
    transactions.filter(t=>t.user===currentUser.user).forEach(t=>{
        const tr=document.createElement('tr');
        tr.innerHTML=`<td>${t.type}</td><td>${t.amount}</td><td>${t.plan}</td><td>${t.days}</td><td>${t.totalProfit}</td><td>${t.status}</td>`;
        tbody.appendChild(tr);
    });
}

// Admin panel
function renderAdmin(){
    if(!currentUser || !currentUser.admin) return;
    const userT=document.querySelector('#userTable tbody'); userT.innerHTML='';
    users.forEach(u=>{
        const tr=document.createElement('tr');
        tr.innerHTML=`<td>${u.user}</td><td>${u.balance}</td><td>${u.plans.length}</td><td><button class="smallbtn" onclick="alert('Manage User')">Action</button></td>`;
        userT.appendChild(tr);
    });
    const txT=document.querySelector('#adminTxTable tbody'); txT.innerHTML='';
    transactions.forEach(t=>{
        const tr=document.createElement('tr');
        tr.innerHTML=`<td>${t.user}</td><td>${t.type}</td><td>${t.amount}</td><td>${t.plan}</td><td>${t.days}</td><td>${t.totalProfit}</td><td>${t.status}</td><td><button class="smallbtn" onclick="alert('Manage Tx')">Action</button></td>`;
        txT.appendChild(tr);
    });
}

// Init
updateUI();
</script>
</body>
</html> 
