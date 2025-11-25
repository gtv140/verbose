<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>VERBOSE Premium App Panel</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
  :root{
    --primary:#00fff0;
    --accent:#ffea00;
    --bg:#0d0d0d;
    --card-bg:#1a1a1a;
    --glass: rgba(255,255,255,0.04);
  }
  *{box-sizing:border-box;}
  body{margin:0;font-family:'Orbitron',sans-serif;background:var(--bg);color:#fff;overflow-x:hidden;}
  .app-header{display:flex;justify-content:space-between;align-items:center;padding:15px;background:#111;box-shadow:0 2px 10px rgba(0,0,0,0.5);}
  .app-header h1{font-size:20px;margin:0;color:var(--primary);}
  .menu-btn{font-size:22px;color:var(--primary);cursor:pointer;}
  .icon-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:15px;padding:15px;}
  .icon-box{background:var(--card-bg);padding:20px;text-align:center;border-radius:15px;transition:0.3s;cursor:pointer;}
  .icon-box:hover{transform:scale(1.05);background:#222;}
  .icon-box i{font-size:28px;color:var(--primary);margin-bottom:8px;}
  .icon-box p{margin:0;font-size:14px;color:#fff;}
  .footer-menu{position:fixed;bottom:0;left:0;width:100%;background:#111;display:flex;justify-content:space-around;padding:10px 0;box-shadow:0 -2px 10px rgba(0,0,0,0.5);}
  .footer-menu div{text-align:center;font-size:12px;color:#fff;cursor:pointer;}
  .footer-menu i{display:block;font-size:22px;margin-bottom:4px;color:var(--primary);}
  .hidden{display:none;}
  input,select,textarea,button{outline:none;}
  input,select,textarea{padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.1);background:#111;color:#fff;width:100%;}
  button{cursor:pointer;}
  .primary{background:var(--primary);color:#000;border:none;padding:10px 12px;border-radius:10px;font-weight:700;}
  .ghost{background:transparent;border:1px solid rgba(255,255,255,0.1);padding:10px;border-radius:8px;color:#fff;}
  .card{background:var(--glass);border-radius:12px;padding:15px;margin-bottom:12px;}
  table{width:100%;border-collapse:collapse;}
  th,td{padding:8px;border-bottom:1px solid rgba(255,255,255,0.05);font-size:13px;color:#eee;}
  .muted{color:#aaa;}
</style>
</head>
<body>
<div class="app-header">
  <h1>VERBOSE</h1>
  <i class="fa fa-bars menu-btn" onclick="toggleMenu()"></i>
</div>

<div class="icon-grid" id="mainIcons">
  <div class="icon-box" onclick="navigate('dashboard')"><i class="fa fa-chart-line"></i><p>Dashboard</p></div>
  <div class="icon-box" onclick="navigate('plans')"><i class="fa fa-briefcase"></i><p>Plans</p></div>
  <div class="icon-box" onclick="navigate('deposit')"><i class="fa fa-wallet"></i><p>Deposit</p></div>
  <div class="icon-box" onclick="navigate('withdrawal')"><i class="fa fa-credit-card"></i><p>Withdrawal</p></div>
  <div class="icon-box" onclick="navigate('transactions')"><i class="fa fa-receipt"></i><p>Transactions</p></div>
  <div class="icon-box" onclick="navigate('referral')"><i class="fa fa-users"></i><p>Referral</p></div>
  <div class="icon-box" onclick="navigate('about')"><i class="fa fa-info-circle"></i><p>About</p></div>
</div>

<div class="footer-menu">
  <div onclick="navigate('dashboard')"><i class="fa fa-home"></i>Home</div>
  <div onclick="navigate('plans')"><i class="fa fa-briefcase"></i>Plans</div>
  <div onclick="navigate('account')"><i class="fa fa-user"></i>Account</div>
</div><div id="authView" class="card">
  <h2>Login / Signup</h2>
  <input id="authUser" placeholder="Username" />
  <input id="authPass" type="password" placeholder="Password" style="margin-top:8px;" />
  <div style="margin-top:10px;">
    <button class="primary" onclick="doLogin()">Login</button>
    <button class="ghost" onclick="doSignup()">Signup</button>
  </div>
  <p class="muted" style="margin-top:10px;">Admin: <strong>AdminKhan</strong> / <strong>SuperSecret123</strong></p>
</div>

<div id="appView" class="hidden">
  <div id="dashboardView" class="card hidden">
    <h2>Dashboard</h2>
    <div class="card"><p>Balance: <span id="balDisplay">0 PKR</span></p></div>
    <div class="card"><p>Profit: <span id="profitDisplay">0 PKR</span></p></div>
    <div class="card"><p>Active Plans: <span id="activePlans">None</span></p></div>
  </div>

  <div id="plansView" class="card hidden">
    <h2>Plans</h2>
    <div id="planGrid"></div>
  </div>

  <div id="depositView" class="card hidden">
    <h2>Deposit</h2>
    <select id="depositPlan" onchange="updateDepositAmount()"></select>
    <input id="depositAmount" readonly style="margin-top:8px;"/>
    <select id="depositMethod" onchange="updateDepositNumber()">
      <option value="jazzcash">JazzCash</option>
      <option value="easypaisa">EasyPaisa</option>
    </select>
    <input id="depositNumber" readonly style="margin-top:8px;" />
    <button class="ghost" onclick="copyText(document.getElementById('depositNumber').value)">Copy</button>
    <input id="depositTx" placeholder="Transaction ID" style="margin-top:8px;" />
    <button class="primary" onclick="submitDeposit()" style="margin-top:10px;">Submit Deposit</button>
  </div>

  <div id="withdrawView" class="card hidden">
    <h2>Withdrawal</h2>
    <select id="withdrawMethod">
      <option value="jazzcash">JazzCash</option>
      <option value="easypaisa">EasyPaisa</option>
    </select>
    <input id="withdrawAccount" placeholder="Account Number / Username" style="margin-top:8px;" />
    <input id="withdrawAmount" placeholder="Amount PKR" style="margin-top:8px;" />
    <button class="primary" onclick="submitWithdraw()" style="margin-top:10px;">Request Withdrawal</button>
  </div><div id="txView" class="card hidden">
  <h2>Transactions</h2>
  <table>
    <thead><tr><th>Type</th><th>Amount</th><th>Plan</th><th>Days</th><th>Total Profit</th><th>Status</th></tr></thead>
    <tbody id="txTableBody"></tbody>
  </table>
</div>

<div id="referralView" class="card hidden">
  <h2>Referral</h2>
  <p class="muted">Invite friends & earn 5% bonus.</p>
  <input id="referralCode" placeholder="Your referral code" readonly />
</div>

<div id="aboutView" class="card hidden">
  <h2>About VERBOSE</h2>
  <p class="muted">VERBOSE ek premium earning platform hai jo modern finance aur neon app-style UI me present hota hai. Owner: John Wilson. Launch: 17 Nov 2025.</p>
</div>

<script>
const ADMIN = {user:'AdminKhan',pass:'SuperSecret123'};
const depositNumbers = {jazzcash:'03705519562',easypaisa:'03379827882'};
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

for(const p of plans){ p.dailyProfit=Math.round(p.totalProfit/p.days); }

function getUsers(){ return JSON.parse(localStorage.getItem('verbose_users')||'[]'); }
function setUsers(u){ localStorage.setItem('verbose_users',JSON.stringify(u)); }
function getTx(){ return JSON.parse(localStorage.getItem('verbose_tx')||'[]'); }
function setTx(t){ localStorage.setItem('verbose_tx',JSON.stringify(t)); }
function getCurrent(){ return JSON.parse(localStorage.getItem('verbose_current')||'null'); }
function setCurrent(u){ localStorage.setItem('verbose_current',JSON.stringify(u)); }

function showToast(text){ alert(text); }

function doSignup(){
  const u=document.getElementById('authUser').value.trim();
  const p=document.getElementById('authPass').value;
  if(!u||!p){showToast('Enter username & password'); return;}
  const users=getUsers();
  if(users.find(x=>x.user===u)){showToast('Username exists'); return;}
  users.push({user:u,pass:p,balance:0,profit:0,active:[],admin:false});
  setUsers(users);
  setCurrent({user:u,admin:false});
  showToast('Signup successful — logged in');
  afterLogin();
}

function doLogin(){
  const u=document.getElementById('authUser').value.trim();
  const p=document.getElementById('authPass').value;
  if(!u||!p){showToast('Enter username & password'); return;}
  if(u===ADMIN.user && p===ADMIN.pass){setCurrent({user:ADMIN.user,admin:true});showToast('Admin logged in'); afterLogin(); return;}
  const users=getUsers();
  const found=users.find(x=>x.user===u && x.pass===p);
  if(!found){showToast('Invalid credentials'); return;}
  setCurrent({user:found.user,admin:false});
  showToast('Login successful');
  afterLogin();
}

function afterLogin(){
  document.getElementById('authView').classList.add('hidden');
  document.getElementById('appView').classList.remove('hidden');
  renderCommon();
}

function navigate(tab){
  const views={
    dashboard:'dashboardView',
    plans:'plansView',
    deposit:'depositView',
    withdrawal:'withdrawView',
    transactions:'txView',
    referral:'referralView',
    about:'aboutView'
  };
  for(const v of Object.values(views)) document.getElementById(v).classList.add('hidden');
  if(views[tab]) document.getElementById(views[tab]).classList.remove('hidden');
}

function renderCommon(){
  const cur=getCurrent();
  if(!cur) return;
  document.getElementById('balDisplay').innerText=cur.balance||0+' PKR';
  document.getElementById('profitDisplay').innerText=cur.profit||0+' PKR';
  document.getElementById('activePlans').innerText=(cur.active?.length||0);
  document.getElementById('referralCode').value=cur.user+'_REF';
  const planSelect=document.getElementById('depositPlan');
  planSelect.innerHTML='';
  plans.forEach(p=>{const o=document.createElement('option');o.value=p.id;o.innerText=p.name+' ('+p.invest+' PKR)';planSelect.appendChild(o);});
  updateDepositAmount();
  updateDepositNumber();
}

function updateDepositAmount(){
  const planId=document.getElementById('depositPlan').value;
  const plan=plans.find(p=>p.id==planId);
  if(plan) document.getElementById('depositAmount').value=plan.invest+' PKR';
}

function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=depositNumbers[method]||'';
}

function copyText(txt){navigator.clipboard.writeText(txt);showToast('Copied!');}

function submitDeposit(){
  const cur=getCurrent(); if(!cur) return;
  const planId=document.getElementById('depositPlan').value;
  const plan=plans.find(p=>p.id==planId); if(!plan) return;
  const tx={user:cur.user,type:'Deposit',amount:plan.invest,plan:plan.name,days:plan.days,totalProfit:plan.totalProfit,status:'Pending',time:new Date().toLocaleString()};
  const txs=getTx(); txs.push(tx); setTx(txs);
  showToast('Deposit submitted');
}

function submitWithdraw(){
  const cur=getCurrent(); if(!cur) return;
  const amt=parseInt(document.getElementById('withdrawAmount').value)||0;
  if(amt<=0){showToast('Enter valid amount'); return;}
  const tx={user:cur.user,type:'Withdrawal',amount:amt,plan:'-',days:'-',totalProfit:'-',status:'Pending',time:new Date().toLocaleString()};
  const txs=getTx(); txs.push(tx); setTx(txs);
  showToast('Withdrawal requested');
}

function toggleMenu(){alert('Menu toggle for app style');}
</script>
</body>
</html>
