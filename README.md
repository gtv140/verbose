<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Verbose Premium Panel</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
:root{
  --neon:#00fff0;
  --accent:#ffea00;
  --bg:#0d0d0d;
  --glass: rgba(255,255,255,0.04);
  --glass-border: rgba(0,255,240,0.08);
}
body {
  margin:0; font-family:'Orbitron',sans-serif; background:var(--bg); color:#fff;
}
.app-header{
  display:flex; justify-content:space-between; align-items:center;
  padding:15px; background:#111; box-shadow:0 2px 10px rgba(0,0,0,0.5);
}
.app-header h1{margin:0; font-size:20px;}
.menu-btn{font-size:22px; cursor:pointer;}
.icon-grid{
  display:grid; grid-template-columns:repeat(3,1fr); gap:15px; padding:20px;
}
.icon-box{
  background:#1a1a1a; padding:20px; text-align:center; border-radius:15px; box-shadow:0 0 10px #000; transition:.3s;
}
.icon-box:hover{transform:scale(1.05); background:#222;}
.icon-box i{font-size:30px; margin-bottom:10px; color:var(--neon);}
.icon-box p{margin:0; font-size:14px;}
.footer-menu{
  position:fixed; bottom:0; left:0; width:100%;
  background:#111; display:flex; justify-content:space-around; padding:10px 0;
  box-shadow:0 -2px 10px rgba(0,0,0,0.5);
}
.footer-menu div{text-align:center; color:#fff; font-size:12px;}
.footer-menu i{display:block; font-size:22px; margin-bottom:5px; color:var(--neon);}

/* Cards, modals, forms */
.card{
  background:var(--glass); border:1px solid var(--glass-border);
  border-radius:12px; padding:14px; margin-bottom:12px;
  backdrop-filter:blur(6px);
}
input, select{width:100%; padding:10px; border-radius:8px; border:1px solid rgba(255,255,255,0.06); background:rgba(0,0,0,0.35); color:#fff; outline:none; margin-top:8px;}
button.primary{background:var(--neon); color:#000; border:none; padding:10px 12px; border-radius:10px; cursor:pointer; font-weight:700;}
button.ghost{background:transparent; border:1px solid rgba(255,255,255,0.06); padding:10px; border-radius:8px; color:#fff; cursor:pointer;}

/* Transactions table readable */
table{width:100%; border-collapse:collapse; color:#cfe;}
th, td{padding:8px; border-bottom:1px solid rgba(255,255,255,0.08); font-size:13px;}
th{background: rgba(0,255,240,0.08); color: var(--neon); text-align:left;}
td{color:#bfe;}
#txView{overflow-x:auto; background:rgba(0,0,0,0.3); border-radius:12px; padding:12px;}
</style>
</head>
<body>

<div class="app-header">
    <h1>VERBOSE Panel</h1>
    <i class="fa fa-bars menu-btn"></i>
</div>

<div class="icon-grid">
    <div class="icon-box" onclick="navigate('dashboard')"><i class="fa fa-wallet"></i><p>Dashboard</p></div>
    <div class="icon-box" onclick="navigate('plans')"><i class="fa fa-bolt"></i><p>Plans</p></div>
    <div class="icon-box" onclick="navigate('deposit')"><i class="fa fa-money-bill"></i><p>Deposit</p></div>
    <div class="icon-box" onclick="navigate('withdrawal')"><i class="fa fa-hand-holding-dollar"></i><p>Withdraw</p></div>
    <div class="icon-box" onclick="navigate('transactions')"><i class="fa fa-receipt"></i><p>Transactions</p></div>
    <div class="icon-box" onclick="navigate('about')"><i class="fa fa-info-circle"></i><p>About</p></div>
</div>

<div class="footer-menu">
    <div onclick="navigate('dashboard')"><i class="fa fa-home"></i>Home</div>
    <div onclick="navigate('plans')"><i class="fa fa-layer-group"></i>Plans</div>
    <div onclick="navigate('transactions')"><i class="fa fa-user"></i>Account</div>
</div>

<div id="appView"><!-- Dashboard / Plans / Deposit / Withdraw / Transactions / About / Admin -->
<div id="dashboard" class="card">
  <h2>Dashboard</h2>
  <div>Balance: <span id="balDisplay">0 PKR</span></div>
  <div>Profit: <span id="profitDisplay">0 PKR</span></div>
  <div>Active Plans: <span id="activePlans">No active plans</span></div>
  <div>Achievements: <span id="achv">No badges yet</span></div>
</div>

<div id="plansView" class="card hidden">
  <h2>Plans</h2>
  <div id="planGrid" class="icon-grid"></div>
</div>

<div id="depositView" class="card hidden">
  <h2>Deposit</h2>
  <label>Choose Plan</label>
  <select id="depositPlan"></select>
  <label>Amount (auto)</label>
  <input id="depositAmount" readonly>
  <label>Method</label>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <label>Number</label>
  <div style="display:flex;gap:8px;align-items:center">
    <input id="depositNumber" readonly style="flex:1">
    <button class="ghost" onclick="copyText(document.getElementById('depositNumber').value)">Copy</button>
  </div>
  <label>Transaction ID (optional)</label>
  <input id="depositTx">
  <label>Upload Proof</label>
  <input type="file" id="depositProof">
  <button class="primary" onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="withdrawView" class="card hidden">
  <h2>Withdrawal</h2>
  <label>Method</label>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <label>Account Number / Username</label>
  <input id="withdrawAccount">
  <label>Amount PKR</label>
  <input type="number" id="withdrawAmount">
  <button class="primary" onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<div id="txView" class="card hidden">
  <h2>Transactions</h2>
  <table>
    <thead>
      <tr>
        <th>Type</th><th>Amount</th><th>Plan</th><th>Days</th><th>Total Profit</th><th>Time</th><th>Status</th>
      </tr>
    </thead>
    <tbody id="txTableBody"></tbody>
  </table>
</div>

<div id="aboutView" class="card hidden">
  <h2>About VERBOSE</h2>
  <p>VERBOSE ek premium earning platform hai, modern digital finance ke liye app style panel. Owner: John Wilson. Launch Date: 17 Nov 2025.</p>
  <button class="primary" onclick="doLogout()">Logout</button>
</div>

<script>
/* ---------- Users & Storage ---------- */
const ADMIN={user:'AdminKhan',pass:'SuperSecret123'};
const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
const plans=[
  {id:1,name:'Plan 1',days:25,invest:250,totalProfit:1200,offer:true},
  {id:2,name:'Plan 2',days:28,invest:500,totalProfit:2500,offer:true},
  {id:3,name:'Plan 3',days:30,invest:750,totalProfit:3750,offer:true},
  {id:4,name:'Plan 4',days:33,invest:1000,totalProfit:4000,offer:true},
  {id:5,name:'Plan 5',days:35,invest:1500,totalProfit:7700,offer:true},
  {id:6,name:'Plan 6',days:38,invest:2000,totalProfit:5700,offer:false},
  {id:7,name:'Plan 7',days:40,invest:2500,totalProfit:7200,offer:false},
  {id:8,name:'Plan 8',days:43,invest:3000,totalProfit:9000,offer:false},
  {id:9,name:'Plan 9',days:45,invest:3500,totalProfit:10800,offer:false},
  {id:10,name:'Plan 10',days:48,invest:4000,totalProfit:13200,offer:false}
];
for(const p of plans)p.dailyProfit=Math.round(p.totalProfit/p.days);

function getUsers(){return JSON.parse(localStorage.getItem('verbose_users')||'[]');}
function setUsers(u){localStorage.setItem('verbose_users',JSON.stringify(u));}
function getTx(){return JSON.parse(localStorage.getItem('verbose_tx')||'[]');}
function setTx(t){localStorage.setItem('verbose_tx',JSON.stringify(t));}
function getCurrent(){return JSON.parse(localStorage.getItem('verbose_current')||'null');}
function setCurrent(u){localStorage.setItem('verbose_current',JSON.stringify(u));}

/* ---------- Initialize admin ---------- */
(function(){
  let u=getUsers();
  if(!u.find(x=>x.user===ADMIN.user))u.push({user:ADMIN.user,pass:ADMIN.pass,balance:0,active:[],profit:0,admin:true});
  setUsers(u);
})();

/* ---------- Navigation ---------- */
function navigate(view){
  const allViews=['dashboard','plansView','depositView','withdrawView','txView','aboutView'];
  allViews.forEach(v=>document.getElementById(v).classList.add('hidden'));
  if(view==='plans')view='plansView';
  if(view==='deposit')view='depositView';
  if(view==='withdrawal')view='withdrawView';
  if(view==='transactions')view='txView';
  if(view==='about')view='aboutView';
  document.getElementById(view).classList.remove('hidden');
  renderCommon();
}

/* ---------- Render Plans ---------- */
function renderPlans(){
  const grid=document.getElementById('planGrid');
  const sel=document.getElementById('depositPlan');
  grid.innerHTML=''; sel.innerHTML='';
  plans.forEach(p=>{
    const div=document.createElement('div'); div.className='icon-box';
    div.innerHTML=`<i class="fa fa-layer-group"></i><p>${p.name}<br>${p.invest} PKR / ${p.days} days</p>`;
    div.onclick=()=>{document.getElementById('depositPlan').value=p.id; updateDepositNumber();}
    grid.appendChild(div);
    const opt=document.createElement('option'); opt.value=p.id; opt.innerText=p.name; sel.appendChild(opt);
  });
}

/* ---------- Deposit Number ---------- */
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=depositNumbers[method];
}

/* ---------- Copy ---------- */
function copyText(t){navigator.clipboard.writeText(t); alert('Copied: '+t);}

/* ---------- Deposit ---------- */
function submitDeposit(){
  const cur=getCurrent(); if(!cur)return alert('Login first');
  const planId=parseInt(document.getElementById('depositPlan').value);
  const plan=plans.find(p=>p.id===planId);
  const amt=plan.invest;
  const txId=document.getElementById('depositTx').value;
  const tx=getTx();
  tx.push({user:cur.user,type:'Deposit',amount:amt,plan:plan.name,days:plan.days,totalProfit:plan.totalProfit,time:Date.now(),status:'Pending',txId});
  setTx(tx);
  alert('Deposit submitted!');
}

/* ---------- Withdraw ---------- */
function submitWithdraw(){
  const cur=getCurrent(); if(!cur)return alert('Login first');
  const amt=parseInt(document.getElementById('withdrawAmount').value);
  const method=document.getElementById('withdrawMethod').value;
  const acc=document.getElementById('withdrawAccount').value;
  const tx=getTx();
  tx.push({user:cur.user,type:'Withdrawal',amount:amt,plan:'-',days:'-',totalProfit:'-',time:Date.now(),status:'Pending',method,acc});
  setTx(tx);
  alert('Withdrawal requested!');
}

/* ---------- Render Transactions ---------- */
function renderCommon(){
  const tx=getTx();
  const tbody=document.getElementById('txTableBody');
  tbody.innerHTML='';
  tx.forEach(t=>{
    const tr=document.createElement('tr');
    tr.innerHTML=`<td>${t.type}</td><td>${t.amount}</td><td>${t.plan}</td><td>${t.days}</td><td>${t.totalProfit}</td><td>${new Date(t.time).toLocaleString()}</td><td>${t.status}</td>`;
    tbody.appendChild(tr);
  });
}

/* ---------- Logout ---------- */
function doLogout(){localStorage.removeItem('verbose_current');alert('Logged out');location.reload();}

/* ---------- Initialize ---------- */
renderPlans(); updateDepositNumber(); navigate('dashboard');
</script>
</body>
</html>
