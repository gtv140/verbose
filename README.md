<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>VERBOSE — Premium Earning Platform</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
<style>
:root{--neon:#00ffff;--accent:#ffea00;--bg1:#0f0c29;--bg2:#302b63;--bg3:#24243e;}
*{box-sizing:border-box;}
html,body{height:100%;margin:0;font-family:'Orbitron',sans-serif;background:#000;color:#fff;overflow-x:hidden;}
body{background:linear-gradient(270deg,var(--bg1),var(--bg2),var(--bg3));background-size:600% 600%;animation:gradientBG 20s ease infinite;}
@keyframes gradientBG{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
#bgCanvas{position:fixed;left:0;top:0;width:100%;height:100%;z-index:-1;pointer-events:none;}
#app{max-width:1200px;margin:20px auto;padding:20px;}
header{display:flex;align-items:center;justify-content:space-between;gap:15px;}
header h1{margin:0;color:var(--neon);text-shadow:0 0 15px var(--neon);}
.small{font-size:13px;color:#cfe;}
.card{background:rgba(0,255,255,0.04);border:1px solid rgba(0,255,255,0.12);padding:15px;border-radius:14px;box-shadow:0 6px 25px rgba(0,0,0,0.6);backdrop-filter:blur(6px);}
.layout{display:grid;grid-template-columns:320px 1fr;gap:20px;margin-top:20px;}
nav{position:sticky;top:20px;height:fit-content;display:flex;flex-direction:column;gap:10px;}
nav button{display:flex;align-items:center;gap:10px;padding:12px;border-radius:12px;border:none;background:linear-gradient(90deg,rgba(0,255,255,0.06),rgba(0,255,255,0.03));color:var(--neon);cursor:pointer;transition:0.2s;}
nav button.active{box-shadow:0 0 20px rgba(0,255,255,0.18);transform:scale(1.02);}
main section{display:none;}
main section.active{display:block;}
input,select,textarea{width:100%;padding:12px;border-radius:10px;border:1px solid rgba(255,255,255,0.08);background:rgba(0,0,0,0.35);color:#fff;outline:none;}
button.primary{background:var(--neon);color:#000;font-weight:700;border-radius:10px;padding:10px 15px;border:none;cursor:pointer;transition:0.2s;}
button.primary:hover{opacity:0.9;transform:scale(1.02);}
button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);color:#fff;padding:10px;border-radius:10px;transition:0.2s;}
button.ghost:hover{opacity:0.8;transform:scale(1.02);}
.plan-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:15px;margin-top:15px;}
.plan-card{border-radius:14px;padding:15px;background:linear-gradient(180deg, rgba(0,255,255,0.03), rgba(0,255,255,0.02));border:1px solid rgba(0,255,255,0.06);position:relative;transition:transform .2s,box-shadow .2s;}
.plan-card:hover{transform:translateY(-8px);box-shadow:0 15px 40px rgba(0,255,255,0.06);}
.badge{position:absolute;top:10px;right:10px;background:var(--accent);color:#000;padding:6px 10px;border-radius:10px;font-weight:800;box-shadow:0 6px 20px rgba(255,150,0,0.15);}
.muted{color:#bfe;}
.row{display:flex;gap:10px;align-items:center;flex-wrap:wrap;}
.notif{position:fixed;right:20px;bottom:20px;background:var(--neon);color:#000;padding:12px 18px;border-radius:12px;font-weight:700;box-shadow:0 10px 30px rgba(0,255,255,0.12);}
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

// Admin & Storage
const adminCreds={user:'AdminKhan',pass:'SuperSecret123'};
let users=JSON.parse(localStorage.getItem('verbose_users')||'[]');
let currentUser=JSON.parse(localStorage.getItem('verbose_current')||'null');
let transactions=JSON.parse(localStorage.getItem('verbose_tx')||'[]');

// Plans array (25 plans)
const plans=[
{ id:1, name:'Plan 1', days:25, invest:250, dailyProfit:48, totalProfit:1200, offer:true },
{ id:2, name:'Plan 2', days:28, invest:500, dailyProfit:89, totalProfit:2500, offer:true },
{ id:3, name:'Plan 3', days:30, invest:750, dailyProfit:125, totalProfit:3750, offer:true },
{ id:4, name:'Plan 4', days:33, invest:1000, dailyProfit:155, totalProfit:4000, offer:true },
{ id:5, name:'Plan 5', days:35, invest:1500, dailyProfit:220, totalProfit:7700, offer:true },
{ id:6, name:'Plan 6', days:38, invest:2000, dailyProfit:150, totalProfit:5700, offer:false },
{ id:7, name:'Plan 7', days:40, invest:2500, dailyProfit:180, totalProfit:7200, offer:false },
{ id:8, name:'Plan 8', days:43, invest:3000, dailyProfit:210, totalProfit:9000, offer:false },
{ id:9, name:'Plan 9', days:45, invest:3500, dailyProfit:240, totalProfit:10800, offer:false },
{ id:10, name:'Plan 10', days:48, invest:4000, dailyProfit:275, totalProfit:13200, offer:false },
{ id:11, name:'Plan 11', days:50, invest:5000, dailyProfit:300, totalProfit:15000, offer:false },
{ id:12, name:'Plan 12', days:53, invest:6000, dailyProfit:340, totalProfit:18000, offer:false },
{ id:13, name:'Plan 13', days:55, invest:7000, dailyProfit:380, totalProfit:21000, offer:false },
{ id:14, name:'Plan 14', days:58, invest:8000, dailyProfit:420, totalProfit:24000, offer:false },
{ id:15, name:'Plan 15', days:60, invest:9000, dailyProfit:455, totalProfit:27000, offer:false },
{ id:16, name:'Plan 16', days:63, invest:10000, dailyProfit:500, totalProfit:30000, offer:false },
{ id:17, name:'Plan 17', days:65, invest:12000, dailyProfit:550, totalProfit:33000, offer:false },
{ id:18, name:'Plan 18', days:68, invest:14000, dailyProfit:600, totalProfit:36000, offer:false },
{ id:19, name:'Plan 19', days:70, invest:16000, dailyProfit:650, totalProfit:39000, offer:false },
{ id:20, name:'Plan 20', days:72, invest:18000, dailyProfit:700, totalProfit:42000, offer:false },
{ id:21, name:'Plan 21', days:75, invest:20000, dailyProfit:750, totalProfit:45000, offer:false },
{ id:22, name:'Plan 22', days:77, invest:25000, dailyProfit:930, totalProfit:60000, offer:false },
{ id:23, name:'Plan 23', days:79, invest:30000, dailyProfit:1120, totalProfit:80000, offer:false },
{ id:24, name:'Plan 24', days:80, invest:40000, dailyProfit:1450, totalProfit:100000, offer:false },
{ id:25, name:'Plan 25', days:85, invest:60000, dailyProfit:2120, totalProfit:150000, offer:false }
];

// Deposit numbers
const depositNumbers = {
  jazzcash: '03705519562',
  easypaisa: '03379827882'
};
</script<script>
// --- Utils ---
function showNotif(msg){const n=document.createElement('div');n.className='notif';n.textContent=msg;document.getElementById('notifRoot').appendChild(n);setTimeout(()=>n.remove(),2500);}
function showTab(tab){document.querySelectorAll('main section').forEach(s=>s.classList.remove('active'));document.querySelectorAll('nav button').forEach(b=>b.classList.remove('active'));document.getElementById(tab).classList.add('active');document.querySelector(`nav button[data-tab="${tab}"]`).classList.add('active');}

// --- Auth ---
function updateUI(){
    if(currentUser){
        document.getElementById('welcome').textContent=`Welcome, ${currentUser.username}`;
        document.getElementById('userBalance').textContent=`Balance: ${currentUser.balance || 0} PKR`;
        document.getElementById('openAuth').style.display='none';
        document.getElementById('logoutBtn').style.display='inline-block';
        showTab('dashboard');
    }else{
        document.getElementById('welcome').textContent='';
        document.getElementById('userBalance').textContent='';
        document.getElementById('openAuth').style.display='inline-block';
        document.getElementById('logoutBtn').style.display='none';
        showTab('authSection');
    }
}
function simpleSignup(){
    const u=document.getElementById('inpUser').value.trim();
    const p=document.getElementById('inpPass').value.trim();
    if(!u||!p){showNotif('Fill username & password');return;}
    if(users.find(x=>x.username===u)){showNotif('User exists');return;}
    const newUser={username:u,password:p,balance:0,activePlans:[]};
    users.push(newUser);
    localStorage.setItem('verbose_users',JSON.stringify(users));
    currentUser=newUser;
    localStorage.setItem('verbose_current',JSON.stringify(currentUser));
    updateUI();
    showNotif('Signup successful');
}
function simpleLogin(){
    const u=document.getElementById('inpUser').value.trim();
    const p=document.getElementById('inpPass').value.trim();
    if(u===adminCreds.user && p===adminCreds.pass){currentUser={username:'Admin',isAdmin:true};updateUI();showNotif('Admin logged in');return;}
    const usr=users.find(x=>x.username===u && x.password===p);
    if(!usr){showNotif('Invalid credentials');return;}
    currentUser=usr;
    localStorage.setItem('verbose_current',JSON.stringify(currentUser));
    updateUI();
    showNotif('Login successful');
}
document.getElementById('logoutBtn').addEventListener('click',()=>{currentUser=null;localStorage.removeItem('verbose_current');updateUI();showNotif('Logged out');});
document.getElementById('openAuth').addEventListener('click',()=>showTab('authSection'));

// --- Plans ---
const planGrid=document.getElementById('planGrid');
const planSelect=document.getElementById('planSelect');
function renderPlans(){
    planGrid.innerHTML='';
    planSelect.innerHTML='';
    plans.forEach(p=>{
        // grid
        const div=document.createElement('div');div.className='plan-card';
        div.innerHTML=`<strong>${p.name}</strong> ${p.offer?'<span class="badge">Offer</span>':''}<div>Invest: ${p.invest} PKR</div><div>Days: ${p.days}</div><div>Daily: ${p.dailyProfit} PKR</div><div>Total: ${p.totalProfit} PKR</div><button class="primary" onclick="selectPlan(${p.id})">Select</button>`;
        planGrid.appendChild(div);
        // dropdown
        const opt=document.createElement('option');opt.value=p.id;opt.textContent=p.name;planSelect.appendChild(opt);
    });
    onPlanChange();
}
function selectPlan(id){planSelect.value=id;onPlanChange();showTab('deposit');}
function onPlanChange(){
    const pid=parseInt(planSelect.value);const p=plans.find(x=>x.id===pid);if(!p)return;document.getElementById('amountInput').value=p.invest;onMethodChange();}
function onMethodChange(){const m=document.getElementById('payMethod').value;document.getElementById('payNumber').value=depositNumbers[m];}
function copyCurrentNumber(){navigator.clipboard.writeText(document.getElementById('payNumber').value);showNotif('Number copied');}

// --- Deposit ---
function submitDeposit(){
    if(!currentUser){showNotif('Login first');return;}
    const pid=parseInt(planSelect.value);const plan=plans.find(x=>x.id===pid);
    if(!plan){showNotif('Select plan');return;}
    const deposit={user:currentUser.username,type:'Deposit',amount:plan.invest,plan:plan.name,days:plan.days,totalProfit:plan.totalProfit,status:'Pending',date:Date.now()};
    transactions.push(deposit);
    localStorage.setItem('verbose_tx',JSON.stringify(transactions));
    currentUser.activePlans.push({plan:plan.name,start:Date.now(),days:plan.days,totalProfit:plan.totalProfit});
    users=users.map(u=>u.username===currentUser.username?currentUser:u);
    localStorage.setItem('verbose_users',JSON.stringify(users));
    localStorage.setItem('verbose_current',JSON.stringify(currentUser));
    updateUI();
    showNotif('Deposit submitted');
}

// --- Withdrawal ---
function submitWithdrawal(){
    if(!currentUser){showNotif('Login first');return;}
    const amt=parseInt(document.getElementById('withdrawAmount').value);if(!amt||amt<=0){showNotif('Invalid amount');return;}
    if(amt>currentUser.balance){showNotif('Insufficient balance');return;}
    const wd={user:currentUser.username,type:'Withdrawal',amount:amt,plan:'-',days:'-',totalProfit:'-',status:'Pending',date:Date.now()};
    transactions.push(wd);
    currentUser.balance-=amt;
    users=users.map(u=>u.username===currentUser.username?currentUser:u);
    localStorage.setItem('verbose_users',JSON.stringify(users));
    localStorage.setItem('verbose_tx',JSON.stringify(transactions));
    localStorage.setItem('verbose_current',JSON.stringify(currentUser));
    updateUI();
    showNotif('Withdrawal requested');
}

// --- Transactions Table ---
function renderTx(){
    const tbody=document.querySelector('#txTable tbody');tbody.innerHTML='';
    transactions.filter(t=>currentUser && t.user===currentUser.username).forEach(tx=>{
        const tr=document.createElement('tr');tr.innerHTML=`<td>${tx.type}</td><td>${tx.amount}</td><td>${tx.plan}</td><td>${tx.days}</td><td>${tx.totalProfit}</td><td>${tx.status}</td>`;tbody.appendChild(tr);
    });
}
function renderAdminTx(){
    const tbody=document.querySelector('#adminTxTable tbody');tbody.innerHTML='';
    transactions.forEach(tx=>{
        const tr=document.createElement('tr');tr.innerHTML=`<td>${tx.user}</td><td>${tx.type}</td><td>${tx.amount}</td><td>${tx.plan}</td><td>${tx.days}</td><td>${tx.totalProfit}</td><td>${tx.status}</td><td><button class="smallbtn" onclick="approveTx('${tx.user}','${tx.type}',${tx.amount})">Approve</button></td>`;tbody.appendChild(tr);
    });
}
function approveTx(user,type,amount){
    const tx=transactions.find(t=>t.user===user && t.type===type && t.amount===amount && t.status==='Pending');
    if(!tx)return;
    tx.status='Approved';
    if(type==='Deposit'){const u=users.find(x=>x.username===user);u.balance+=amount;}
    if(type==='Withdrawal'){/* already deducted */ }
    localStorage.setItem('verbose_tx',JSON.stringify(transactions));
    localStorage.setItem('verbose_users',JSON.stringify(users));
    renderTx();renderAdminTx();
    showNotif('Transaction approved');
}

// --- Users Table ---
function renderUsers(){
    const tbody=document.querySelector('#userTable tbody');tbody.innerHTML='';
    users.forEach(u=>{
        const tr=document.createElement('tr');tr.innerHTML=`<td>${u.username}</td><td>${u.balance}</td><td>${u.activePlans.map(p=>p.plan).join(', ') || '-'}</td><td><button class="smallbtn" onclick="deleteUser('${u.username}')">Delete</button></td>`;tbody.appendChild(tr);
    });
}
function deleteUser(uname){users=users.filter(u=>u.username!==uname);localStorage.setItem('verbose_users',JSON.stringify(users));renderUsers();showNotif('User deleted');}

// --- Nav ---
document.querySelectorAll('nav button').forEach(btn=>btn.addEventListener('click',()=>showTab(btn.dataset.tab)));

// --- Init ---
renderPlans();updateUI();renderTx();renderAdminTx();renderUsers();

// Auto dashboard data refresh
setInterval(()=>{if(currentUser){document.getElementById('balDisplay').textContent=currentUser.balance;renderTx();}},3000);

</script>
</body>
</html>
