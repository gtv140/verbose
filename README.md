<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Premium App Panel</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
body {margin:0;padding:0;font-family:Arial,sans-serif;background:#0d0d0d;color:#fff;}
.app-header {display:flex;justify-content:space-between;align-items:center;padding:15px;background:#111;box-shadow:0 2px 10px rgba(0,0,0,0.5);}
.app-header h1 {font-size:20px;margin:0;}
.menu-btn {font-size:22px;cursor:pointer;}
.icon-grid {display:grid;grid-template-columns:repeat(3,1fr);gap:15px;padding:20px;}
.icon-box {background:#1a1a1a;padding:20px;text-align:center;border-radius:15px;box-shadow:0 0 10px #000;transition:.3s;cursor:pointer;}
.icon-box:hover {transform:scale(1.05);background:#222;}
.icon-box i {font-size:30px;margin-bottom:10px;color:#0af;}
.icon-box p {margin:0;font-size:14px;}
.footer-menu {position:fixed;bottom:0;left:0;width:100%;background:#111;display:flex;justify-content:space-around;padding:10px 0;box-shadow:0 -2px 10px rgba(0,0,0,0.5);}
.footer-menu div {text-align:center;color:#fff;font-size:12px;}
.footer-menu i {display:block;font-size:22px;margin-bottom:5px;}
.card {background:#1a1a1a;padding:15px;border-radius:15px;margin:15px;}
.hidden {display:none;}
input,select{width:100%;padding:10px;border-radius:8px;border:none;background:#222;color:#fff;margin-top:5px;}
button{padding:10px 12px;border-radius:8px;border:none;cursor:pointer;font-weight:700;}
.primary{background:#0af;color:#000;}
.ghost{background:transparent;border:1px solid #0af;color:#0af;}
.notif {position:fixed;right:15px;bottom:15px;background:#0af;color:#000;padding:10px 15px;border-radius:12px;font-weight:700;box-shadow:0 5px 20px rgba(0,255,255,0.3);}
.table-container{overflow:auto;max-height:200px;margin-top:10px;}
table{width:100%;border-collapse:collapse;}
th,td{padding:8px;border-bottom:1px solid #444;font-size:12px;text-align:center;}
</style>
</head>
<body>

<div class="app-header">
<h1>Premium Panel</h1>
<i class="fa fa-bars menu-btn" id="menuToggle"></i>
</div>

<div id="authCard" class="card">
<h3>Login / Signup</h3>
<input id="authUser" placeholder="Username">
<input id="authPass" placeholder="Password" type="password">
<div style="margin-top:10px;display:flex;gap:10px;">
<button class="primary" onclick="doLogin()">Login</button>
<button class="ghost" onclick="doSignup()">Signup</button>
</div>
<div style="margin-top:10px;font-size:12px;color:#aaa;">Admin: <strong>AdminKhan</strong> / <strong>SuperSecret123</strong></div>
</div>

<div id="appView" class="hidden">
<div class="icon-grid">
<div class="icon-box" onclick="navigate('wallet')"><i class="fa fa-wallet"></i><p>Wallet</p></div>
<div class="icon-box" onclick="navigate('plans')"><i class="fa fa-bolt"></i><p>Plans</p></div>
<div class="icon-box" onclick="navigate('deposit')"><i class="fa fa-arrow-up"></i><p>Deposit</p></div>
<div class="icon-box" onclick="navigate('withdraw')"><i class="fa fa-arrow-down"></i><p>Withdraw</p></div>
<div class="icon-box" onclick="navigate('transactions')"><i class="fa fa-list"></i><p>Transactions</p></div>
<div class="icon-box" id="adminBtn" onclick="navigate('admin')"><i class="fa fa-cog"></i><p>Admin</p></div>
</div>

<div id="walletView" class="card hidden">
<h3>Wallet</h3>
<div>Balance: <span id="balDisplay">0 PKR</span></div>
<div>Profit: <span id="profitDisplay">0 PKR</span></div>
<div>Referral Code: <span id="refCode"></span></div>
</div>

<div id="plansView" class="card hidden">
<h3>Plans</h3>
<div id="planGrid"></div>
</div>

<div id="depositView" class="card hidden">
<h3>Deposit</h3>
<select id="depositPlan" onchange="updateDepositAmount()"></select>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="depositNumber" readonly>
<button class="ghost" onclick="copyText(document.getElementById('depositNumber').value)">Copy Number</button>
<input id="depositAmount" readonly>
<input id="depositTx" placeholder="Transaction ID (optional)">
<button class="primary" onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="withdrawView" class="card hidden">
<h3>Withdraw</h3>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="withdrawAccount" placeholder="Account Number">
<input id="withdrawAmount" type="number" placeholder="Amount PKR">
<button class="primary" onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<div id="transactionsView" class="card hidden">
<h3>Transactions</h3>
<div class="table-container">
<table>
<thead><tr><th>Type</th><th>Amount</th><th>Plan</th><th>Status</th><th>Time</th></tr></thead>
<tbody id="txTable"></tbody>
</table>
</div>
</div>

<div id="adminView" class="card hidden">
<h3>Admin Panel</h3>
<div class="table-container">
<table>
<thead><tr><th>User</th><th>Balance</th><th>Profit</th></tr></thead>
<tbody id="userTable"></tbody>
</table>
</div>
</div>

<div class="footer-menu">
<div onclick="navigate('wallet')"><i class="fa fa-wallet"></i>Wallet</div>
<div onclick="navigate('plans')"><i class="fa fa-bolt"></i>Plans</div>
<div onclick="navigate('deposit')"><i class="fa fa-arrow-up"></i>Deposit</div>
<div onclick="navigate('withdraw')"><i class="fa fa-arrow-down"></i>Withdraw</div>
<div onclick="navigate('transactions')"><i class="fa fa-list"></i>Tx</div>
</div>

<div id="toastRoot"></div>

<script>
const ADMIN={user:'AdminKhan',pass:'SuperSecret123'};
const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
const plans=[
{id:1,name:'Plan 1',days:25,invest:250,totalProfit:1200},
{id:2,name:'Plan 2',days:28,invest:500,totalProfit:2500},
{id:3,name:'Plan 3',days:30,invest:750,totalProfit:3750},
{id:4,name:'Plan 4',days:33,invest:1000,totalProfit:4000},
{id:5,name:'Plan 5',days:35,invest:1500,totalProfit:7700}];
for(const p of plans){p.dailyProfit=Math.round(p.totalProfit/p.days);}
function getUsers(){return JSON.parse(localStorage.getItem('app_users')||'[]');}
function setUsers(u){localStorage.setItem('app_users',JSON.stringify(u));}
function getTx(){return JSON.parse(localStorage.getItem('app_tx')||'[]');}
function setTx(t){localStorage.setItem('app_tx',JSON.stringify(t));}
function getCurrent(){return JSON.parse(localStorage.getItem('app_current')||'null');}
function setCurrent(u){localStorage.setItem('app_current',JSON.stringify(u));}

/* TOAST */
function showToast(msg){const n=document.createElement('div');n.className='notif';n.innerText=msg;document.body.appendChild(n);setTimeout(()=>n.remove(),2200);}

/* NAV */
function hideAll(){document.getElementById('walletView').classList.add('hidden');document.getElementById('plansView').classList.add('hidden');document.getElementById('depositView').classList.add('hidden');document.getElementById('withdrawView').classList.add('hidden');document.getElementById('transactionsView').classList.add('hidden');document.getElementById('adminView').classList.add('hidden');}
function navigate(view){hideAll();if(view==='wallet')document.getElementById('walletView').classList.remove('hidden');else if(view==='plans')document.getElementById('plansView').classList.remove('hidden');else if(view==='deposit')document.getElementById('depositView').classList.remove('hidden');else if(view==='withdraw')document.getElementById('withdrawView').classList.remove('hidden');else if(view==='transactions')document.getElementById('transactionsView').classList.remove('hidden');else if(view==='admin'){const cur=getCurrent();if(cur?.admin)document.getElementById('adminView').classList.remove('hidden');else showToast('Admin only');} renderCommon();}

/* AUTH */
(function initUsers(){let u=getUsers();if(!u.find(x=>x.user===ADMIN.user)){u.push({user:ADMIN.user,pass:ADMIN.pass,balance:0,profit:0,active:[],admin:true,referralCode:'admin_ref',referred:[]});setUsers(u);}})();
function doSignup(){const u=prompt('Username');const p=prompt('Password');if(!u||!p){showToast('Enter both');return;}let users=getUsers();if(users.find(x=>x.user===u)){showToast('Exists');return;}users.push({user:u,pass:p,balance:0,profit:0,active:[],admin:false,referralCode:u+'_ref',referred:[]});setUsers(users);setCurrent({user:u,admin:false});showToast('Signup success');renderCommon();}
function doLogin(){const u=prompt('Username');const p=prompt('Password');if(!u||!p){showToast('Enter both');return;}if(u===ADMIN.user&&p===ADMIN.pass){setCurrent({user:u,admin:true});showToast('Admin logged in');renderCommon();return;}let users=getUsers();const f=users.find(x=>x.user===u&&x.pass===p);if(!f){showToast('Invalid');return;}setCurrent({user:f.user,admin:false});showToast('Login success');renderCommon();}
function doLogout(){localStorage.removeItem('app_current');document.getElementById('appView').classList.add('hidden');document.getElementById('authCard').classList.remove('hidden');showToast('Logged out');}

/* PLANS */
function renderPlans(){const pg=document.getElementById('planGrid');pg.innerHTML='';plans.forEach(p=>{const d=document.createElement('div');d.className='card';d.innerHTML=`<b>${p.name}</b><div>Invest: ${p.invest} PKR</div><div>Days: ${p.days}</div><div>Total: ${p.totalProfit}</div><div>Daily: ${p.dailyProfit}</div><button class="primary" onclick="selectDeposit(${p.id})">Select</button>`;pg.appendChild(d);});}
function selectDeposit(planId){document.getElementById('depositPlan').value=planId;updateDepositAmount();navigate('deposit');}

/* DEPOSIT */
function updateDepositAmount(){const pid=parseInt(document.getElementById('depositPlan').value);const plan=plans.find(p=>p.id===pid);if(plan)document.getElementById('depositAmount').value=plan.invest;}
function updateDepositNumber(){const m=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=depositNumbers[m];}
function submitDeposit(){const cur=getCurrent();if(!cur){showToast('Login first');return;}const pid=parseInt(document.getElementById('depositPlan').value);const plan=plans.find(p=>p.id===pid);let tx=getTx();tx.push({user:cur.user,type:'Deposit',amount:plan.invest,plan:plan.name,status:'Completed',time:new Date().toLocaleString()});setTx(tx);let users=getUsers();const u=users.find(x=>x.user===cur.user);u.balance+=plan.invest;setUsers(users);showToast('Deposit added');renderCommon();}

/* WITHDRAW */
function submitWithdraw(){const cur=getCurrent();if(!cur){showToast('Login first');return;}const amt=parseFloat(document.getElementById('withdrawAmount').value);if(!amt||amt<=0){showToast('Enter valid amount');return;}let users=getUsers();const u=users.find(x=>x.user===cur.user);if(u.balance<amt){showToast('Insufficient balance');return;}u.balance-=amt;setUsers(users);let tx=getTx();tx.push({user:cur.user,type:'Withdraw',amount:amt,plan:'-',status:'Completed',time:new Date().toLocaleString()});setTx(tx);showToast('Withdrawal success');renderCommon();}

/* TRANSACTIONS */
function renderTx(){const tx=getTx();const tb=document.getElementById('txTable');tb.innerHTML='';tx.forEach(t=>{const r=document.createElement('tr');r.innerHTML=`<td>${t.type}</td><td>${t.amount}</td><td>${t.plan}</td><td>${t.status}</td><td>${t.time}</td>`;tb.appendChild(r);});}

/* COPY */
function copyText(text){navigator.clipboard.writeText(text);showToast('Copied');}

/* COMMON RENDER */
function renderCommon(){
const cur=getCurrent();
if(cur){document.getElementById('authCard').classList.add('hidden');document.getElementById('appView').classList.remove('hidden');renderPlans();updateDepositNumber();updateDepositAmount();renderTx();document.getElementById('balDisplay').innerText=getUsers().find(x=>x.user===cur.user).balance+' PKR';document.getElementById('profitDisplay').innerText=getUsers().find(x=>x.user===cur.user).profit+' PKR';document.getElementById('refCode').innerText=getUsers().find(x=>x.user===cur.user).referralCode;document.getElementById('adminBtn').style.display=cur.admin?'block':'none';}else{document.getElementById('authCard').classList.remove('hidden');document.getElementById('appView').classList.add('hidden');}
}

/* INIT */
document.getElementById('depositPlan').innerHTML=plans.map(p=>`<option value="${p.id}">${p.name}</option>`).join('');
renderCommon();
</script>
</body>
</html>
