<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>NEON EARN — Premium Platform</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
<style>
:root{
    --neon:#00fff7;
    --accent:#ff3eff;
    --bg1:#0a0a0a;
    --bg2:#1a1a1a;
}
*{box-sizing:border-box;}
body{margin:0;font-family:'Orbitron',sans-serif;background:linear-gradient(120deg,var(--bg1),var(--bg2));color:#fff;overflow-x:hidden;}
h1,h2,h3,h4,h5,h6{margin:0;}
header{display:flex;justify-content:space-between;align-items:center;padding:15px;background:#111;border-bottom:1px solid var(--neon);}
header h1{color:var(--neon);text-shadow:0 0 8px var(--neon);}
button{cursor:pointer;transition:0.2s;}
button:hover{opacity:0.85;transform:scale(1.02);}
.card{background:rgba(0,255,255,0.05);border:1px solid rgba(0,255,255,0.2);padding:15px;margin:15px 0;border-radius:12px;text-align:center;box-shadow:0 0 15px rgba(0,255,255,0.3);}
input,select{padding:10px;border-radius:8px;border:none;background:rgba(0,0,0,0.4);color:#fff;width:100%;margin:5px 0;outline:none;}
button.primary{background:var(--neon);color:#000;padding:10px 15px;border-radius:8px;border:none;font-weight:700;}
button.accent{background:var(--accent);color:#000;padding:8px 12px;border-radius:6px;border:none;font-weight:600;}
nav{display:flex;gap:10px;flex-wrap:wrap;margin:10px 0;}
section{display:none;}
section.active{display:block;}
.notif{position:fixed;bottom:20px;right:20px;padding:12px 18px;border-radius:10px;background:var(--neon);color:#000;font-weight:700;box-shadow:0 0 20px rgba(0,255,255,0.3);}
.plan-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:12px;margin-top:10px;}
.plan-card{padding:12px;border-radius:10px;background:rgba(255,255,255,0.05);border:1px solid var(--neon);text-align:center;transition:0.3s;}
.plan-card:hover{box-shadow:0 0 20px var(--neon);transform:translateY(-5px);}
</style>
</head>
<body>

<header>
<h1>NEON EARN</h1>
<div id="topActions">
    <span id="welcome"></span>
    <button id="openAuth" class="primary">Login / Signup</button>
    <button id="logoutBtn" class="primary" style="display:none;">Logout</button>
</div>
</header>

<div style="padding:15px; max-width:900px;margin:auto;">

<nav>
<button data-tab="dashboard" class="accent">Dashboard</button>
<button data-tab="plans" class="accent">Plans</button>
<button data-tab="deposit" class="accent">Deposit</button>
<button data-tab="withdrawal" class="accent">Withdraw</button>
<button data-tab="transactions" class="accent">Transactions</button>
</nav>

<section id="authSection" class="active card">
<h2>Login / Signup</h2>
<input id="inpUser" placeholder="Username">
<input id="inpPass" type="password" placeholder="Password">
<div style="display:flex;gap:10px;margin-top:10px;">
<button class="primary" onclick="simpleLogin()">Login</button>
<button class="primary" onclick="simpleSignup()">Signup</button>
</div>
</section>

<section id="dashboard" class="card">
<h2>Dashboard</h2>
<div class="plan-card">Balance: <span id="balDisplay">0 PKR</span></div>
<div class="plan-card">Total Profit: <span id="profitDisplay">0 PKR</span></div>
<div class="plan-card">Active Plans: <span id="activePlans">None</span></div>
</section>

<section id="plans" class="card">
<h2>Available Plans</h2>
<div class="plan-grid" id="planGrid"></div>
</section>

<section id="deposit" class="card">
<h2>Deposit</h2>
<select id="planSelect" onchange="onPlanChange()"></select>
<select id="payMethod" onchange="onMethodChange()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="payNumber" readonly>
<button class="accent" onclick="copyCurrentNumber()">Copy Number</button>
<button class="primary" onclick="submitDeposit()">Submit Deposit</button>
</section>

<section id="withdrawal" class="card">
<h2>Withdrawal</h2>
<input id="withdrawAccount" placeholder="Account Number / Username">
<input id="withdrawAmount" placeholder="Amount PKR" type="number">
<button class="primary" onclick="submitWithdrawal()">Request Withdrawal</button>
</section>

<section id="transactions" class="card">
<h2>Transactions</h2>
<div style="overflow:auto;max-height:250px">
<table style="width:100%;color:#0ff;border-collapse:collapse;">
<thead><tr><th>Type</th><th>Amount</th><th>Plan</th><th>Status</th></tr></thead>
<tbody id="txTable"></tbody>
</table>
</div>
</section>

<div id="notifRoot"></div>

<script>
// --- Neon Utils ---
function showNotif(msg){const n=document.createElement('div');n.className='notif';n.textContent=msg;document.body.appendChild(n);setTimeout(()=>n.remove(),2000);}
function showTab(tab){document.querySelectorAll('section').forEach(s=>s.classList.remove('active'));document.getElementById(tab).classList.add('active');}

// --- Users & Auth ---
const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
let users=JSON.parse(localStorage.getItem('neon_users')||'[]');
let currentUser=JSON.parse(localStorage.getItem('neon_current')||'null');
let transactions=JSON.parse(localStorage.getItem('neon_tx')||'[]');
const plans=[
{ id:1,name:'Starter',invest:500,days:7,dailyProfit:80,totalProfit:560 },
{ id:2,name:'Pro',invest:1000,days:10,dailyProfit:120,totalProfit:1200 },
{ id:3,name:'Elite',invest:2000,days:14,dailyProfit:250,totalProfit:3500 }
];

function updateUI(){
    if(currentUser){
        document.getElementById('welcome').textContent=`Hello, ${currentUser.username}`;
        document.getElementById('openAuth').style.display='none';
        document.getElementById('logoutBtn').style.display='inline-block';
        document.getElementById('balDisplay').textContent=currentUser.balance||0;
        document.getElementById('activePlans').textContent=currentUser.activePlans.map(p=>p.name).join(', ')||'None';
    }else{
        document.getElementById('welcome').textContent='';
        document.getElementById('openAuth').style.display='inline-block';
        document.getElementById('logoutBtn').style.display='none';
    }
}

function simpleSignup(){
    const u=document.getElementById('inpUser').value.trim();
    const p=document.getElementById('inpPass').value.trim();
    if(!u||!p){showNotif('Enter username & password');return;}
    if(users.find(x=>x.username===u)){showNotif('User exists');return;}
    const newUser={username:u,password:p,balance:0,activePlans:[]};
    users.push(newUser);
    localStorage.setItem('neon_users',JSON.stringify(users));
    currentUser=newUser;
    localStorage.setItem('neon_current',JSON.stringify(currentUser));
    updateUI();showNotif('Signup successful');
}

function simpleLogin(){
    const u=document.getElementById('inpUser').value.trim();
    const p=document.getElementById('inpPass').value.trim();
    const usr=users.find(x=>x.username===u && x.password===p);
    if(!usr){showNotif('Invalid credentials');return;}
    currentUser=usr;
    localStorage.setItem('neon_current',JSON.stringify(currentUser));
    updateUI();showNotif('Login successful');
}

document.getElementById('logoutBtn').addEventListener('click',()=>{
    currentUser=null;
    localStorage.removeItem('neon_current');
    updateUI();
    showNotif('Logged out');
});

// --- Plans ---
const planGrid=document.getElementById('planGrid');
const planSelect=document.getElementById('planSelect');
function renderPlans(){
    planGrid.innerHTML='';planSelect.innerHTML='';
    plans.forEach(p=>{
        // Grid cards
        const div=document.createElement('div');div.className='plan-card';
        div.innerHTML=`<strong>${p.name}</strong><div>Invest: ${p.invest}</div><div>Days: ${p.days}</div><div>Total: ${p.totalProfit}</div><button class="primary" onclick="selectPlan(${p.id})">Select</button>`;
        planGrid.appendChild(div);
        // Dropdown
        const opt=document.createElement('option');opt.value=p.id;opt.textContent=p.name;planSelect.appendChild(opt);
    });
    onPlanChange();
}
function selectPlan(id){planSelect.value=id;onPlanChange();showTab('deposit');}
function onPlanChange(){const p=plans.find(x=>x.id==planSelect.value);document.getElementById('payNumber').value=depositNumbers[document.getElementById('payMethod').value];}
function onMethodChange(){document.getElementById('payNumber').value=depositNumbers[document.getElementById('payMethod').value];}
function copyCurrentNumber(){navigator.clipboard.writeText(document.getElementById('payNumber').value);showNotif('Number copied');}

// --- Deposit ---
function submitDeposit(){
    if(!currentUser){showNotif('Login first');return;}
    const plan=plans.find(x=>x.id==planSelect.value);
    const deposit={user:currentUser.username,type:'Deposit',amount:plan.invest,plan:plan.name,status:'Pending'};
    transactions.push(deposit);
    currentUser.activePlans.push(plan);
    users=users.map(u=>u.username===currentUser.username?currentUser:u);
    localStorage.setItem('neon_users',JSON.stringify(users));
    localStorage.setItem('neon_tx',JSON.stringify(transactions));
    localStorage.setItem('neon_current',JSON.stringify(currentUser));
    updateUI();renderTx();showNotif('Deposit submitted');
}

// --- Withdrawal ---
function submitWithdrawal(){
    const amt=parseInt(document.getElementById('withdrawAmount').value);
    if(!currentUser || !amt || amt>currentUser.balance){showNotif('Invalid or insufficient balance');return;}
    transactions.push({user:currentUser.username,type:'Withdrawal',amount:amt,plan:'-',status:'Pending'});
    currentUser.balance-=amt;
    users=users.map(u=>u.username===currentUser.username?currentUser:u);
    localStorage.setItem('neon_users',JSON.stringify(users));
    localStorage.setItem('neon_tx',JSON.stringify(transactions));
    localStorage.setItem('neon_current',JSON.stringify(currentUser));
    updateUI();renderTx();showNotif('Withdrawal requested');
}

// --- Transactions ---
function renderTx(){
    const tbody=document.getElementById('txTable');tbody.innerHTML='';
    transactions.filter(t=>currentUser && t.user===currentUser.username).forEach(tx=>{
        const tr=document.createElement('tr');tr.innerHTML=`<td>${tx.type}</td><td>${tx.amount}</td><td>${tx.plan}</td><td>${tx.status}</td>`;tbody.appendChild(tr);
    });
}

// --- Init ---
renderPlans();updateUI();renderTx();
document.querySelectorAll('nav button').forEach(b=>b.addEventListener('click',()=>showTab(b.dataset.tab)));

</script>
</body>
</html>
