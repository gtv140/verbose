<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>VERBOSE App Style Premium Panel</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
:root{
    --neon:#00fff0;
    --accent:#ffea00;
    --bg:#0d0d0d;
    --card:#1a1a1a;
    --glass: rgba(255,255,255,0.05);
    --glass-border: rgba(0,255,240,0.08);
}
body{
    margin:0;
    font-family:'Orbitron',sans-serif;
    background: linear-gradient(120deg,#050012,#0d0626);
    color:#fff;
}
.hidden{display:none;}
.app-header{
    display:flex;justify-content:space-between;align-items:center;
    padding:15px;background:#111;box-shadow:0 2px 10px rgba(0,0,0,0.5);
}
.app-header h1{font-size:20px;margin:0;}
.menu-btn{font-size:22px;cursor:pointer;}
.icon-grid{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:15px;
    padding:20px;
}
.icon-box{
    background: var(--card);
    padding:20px;
    text-align:center;
    border-radius:15px;
    box-shadow:0 0 10px #000;
    transition:0.3s;
}
.icon-box:hover{
    transform:scale(1.05);
    background:#222;
}
.icon-box i{font-size:30px;margin-bottom:10px;color:var(--neon);}
.icon-box p{margin:0;font-size:14px;}
.footer-menu{
    position:fixed;bottom:0;left:0;width:100%;
    background:#111;display:flex;justify-content:space-around;
    padding:10px 0;box-shadow:0 -2px 10px rgba(0,0,0,0.5);
}
.footer-menu div{text-align:center;font-size:12px;color:#fff;}
.footer-menu i{display:block;font-size:22px;margin-bottom:5px;}
.card{background:var(--glass);border:1px solid var(--glass-border);padding:14px;border-radius:12px;box-shadow:0 8px 30px rgba(0,0,0,0.6);backdrop-filter: blur(6px);margin-bottom:15px;}
.row{display:flex;gap:10px;align-items:center;flex-wrap:wrap;}
.primary{background:var(--neon);color:#000;border:none;padding:10px 12px;border-radius:10px;cursor:pointer;font-weight:700;}
.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:10px;border-radius:8px;color:#fff;cursor:pointer;}
.plan-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:12px;margin-top:12px;}
.plan{border-radius:12px;padding:12px;border:1px solid rgba(0,255,240,0.06);background:linear-gradient(180deg,rgba(0,255,240,0.02),transparent);position:relative;transition:transform .18s;}
.plan:hover{transform:translateY(-8px);box-shadow:0 16px 40px rgba(0,255,240,0.04);}
.badge{position:absolute;top:10px;right:10px;background:var(--accent);color:#000;padding:6px 8px;border-radius:8px;font-weight:800;}
.muted{color:#bfe;opacity:.9;}
table{width:100%;border-collapse:collapse;}
th,td{padding:8px;border-bottom:1px solid rgba(255,255,255,0.03);font-size:13px;}
.notif{position:fixed;right:18px;bottom:18px;background:var(--neon);color:#000;padding:12px 16px;border-radius:12px;font-weight:700;box-shadow:0 10px 30px rgba(0,255,240,0.12);}
.admin-badge{background:#ff4d4d;color:#000;padding:6px 8px;border-radius:8px;font-weight:800;}
</style>
</head>
<body>

<div class="app-header">
    <h1>VERBOSE</h1>
    <i class="fa fa-bars menu-btn"></i>
</div>

<div id="welcomeText" class="muted small" style="margin-left:15px;">Not logged in</div>
<div class="row" style="padding:15px;">
    <button id="openAuthBtn" class="primary">Login / Signup</button>
    <div id="balanceTop" style="font-weight:800;color:var(--neon);margin-left:8px"></div>
    <button id="logoutBtn" class="primary hidden" style="background:linear-gradient(90deg,#ff4d4d,#ff1a1a)">Logout</button>
</div>

<div class="icon-grid">
    <div class="icon-box" onclick="navigate('dashboard')">
        <i class="fa fa-chart-line"></i>
        <p>Dashboard</p>
    </div>
    <div class="icon-box" onclick="navigate('plans')">
        <i class="fa fa-briefcase"></i>
        <p>Plans</p>
    </div>
    <div class="icon-box" onclick="navigate('deposit')">
        <i class="fa fa-wallet"></i>
        <p>Deposit</p>
    </div>
    <div class="icon-box" onclick="navigate('withdrawal')">
        <i class="fa fa-money-bill"></i>
        <p>Withdrawal</p>
    </div>
    <div class="icon-box" onclick="navigate('transactions')">
        <i class="fa fa-list"></i>
        <p>Transactions</p>
    </div>
    <div class="icon-box" onclick="navigate('about')">
        <i class="fa fa-info-circle"></i>
        <p>About</p>
    </div>
    <div class="icon-box" id="adminNavBtn" onclick="navigate('admin')" class="hidden">
        <i class="fa fa-user-shield"></i>
        <p>Admin</p>
    </div>
</div>

<!-- AUTH VIEW -->
<div id="authView" class="card">
    <h2>Login / Signup</h2>
    <input id="authUser" placeholder="Username" />
    <input id="authPass" type="password" placeholder="Password" style="margin-top:8px;"/>
    <div class="row" style="margin-top:10px;">
        <button class="primary" onclick="doLogin()">Login</button>
        <button class="ghost" onclick="doSignup()">Signup</button>
    </div>
    <div class="muted small" style="margin-top:10px;">Signup saved locally. Admin: <strong>AdminKhan</strong> / <strong>SuperSecret123</strong></div>
</div>

<!-- APP VIEW -->
<div id="appView" class="hidden">

<!-- Dashboard -->
<div id="dashboard" class="card hidden">
    <h2>Dashboard</h2>
    <div class="row">
        <div class="card" style="flex:1;">
            <div class="muted small">Balance</div>
            <div id="balDisplay" style="font-weight:800;color:var(--neon);font-size:18px;">0 PKR</div>
        </div>
        <div class="card" style="flex:1;">
            <div class="muted small">Profit</div>
            <div id="profitDisplay" style="font-weight:800;color:var(--neon);font-size:18px;">0 PKR</div>
        </div>
    </div>
    <div class="card" style="margin-top:10px;">
        <h4>Active Plans</h4>
        <div id="activePlans" class="muted small">No active plans</div>
    </div>
    <div class="card" style="margin-top:10px;">
        <h4>Referral Bonus</h4>
        <div id="refBonus" class="muted small">0 PKR</div>
    </div>
</div>

<!-- Plans -->
<div id="plansView" class="card hidden">
    <h2>Plans</h2>
    <div class="plan-grid" id="planGrid"></div>
</div>

<!-- Deposit -->
<div id="depositView" class="card hidden">
    <h2>Deposit</h2>
    <div class="muted small">Choose a plan, send payment, upload proof (simulation)</div>
    <div style="margin-top:12px;">
        <label class="muted small">Choose Plan</label>
        <select id="depositPlan"></select>
    </div>
    <div class="row" style="margin-top:8px;">
        <div style="flex:1;">
            <label class="muted small">Amount</label>
            <input id="depositAmount" readonly/>
        </div>
        <div style="width:160px;">
            <label class="muted small">Method</label>
            <select id="depositMethod" onchange="updateDepositNumber()">
                <option value="jazzcash">JazzCash</option>
                <option value="easypaisa">EasyPaisa</option>
            </select>
        </div>
    </div>
    <div class="row" style="margin-top:10px;">
        <input id="depositNumber" readonly style="flex:1;"/>
        <button class="ghost" onclick="copyText(document.getElementById('depositNumber').value)">Copy</button>
    </div>
    <div style="margin-top:8px;">
        <input id="depositTx" placeholder="Transaction ID (optional)"/>
    </div>
    <div style="margin-top:8px;">
        <input id="depositProof" type="file"/>
    </div>
    <div class="row" style="margin-top:10px;">
        <button class="primary" onclick="submitDeposit()">Submit Deposit</button>
        <button class="ghost" onclick="navigate('dashboard')">Back</button>
    </div>
</div>

<script>
// ---------- DATA ----------
const ADMIN = {user:'AdminKhan',pass:'SuperSecret123'};
const depositNumbers = {jazzcash:'03705519562',easypaisa:'03379827882'};
const plans=[
    {id:1,name:'Plan 1',days:25,invest:250,totalProfit:1200,offer:true},
    {id:2,name:'Plan 2',days:28,invest:500,totalProfit:2500,offer:true},
    {id:3,name:'Plan 3',days:30,invest:750,totalProfit:3750,offer:true},
    {id:4,name:'Plan 4',days:33,invest:1000,totalProfit:4000,offer:true},
    {id:5,name:'Plan 5',days:35,invest:1500,totalProfit:7700,offer:true},
    {id:6,name:'Plan 6',days:38,invest:2000,totalProfit:5700,offer:false},
    {id:7,name:'Plan 7',days:40,invest:2500,totalProfit:7200,offer:false}
];
for(const p of plans) p.dailyProfit=Math.round(p.totalProfit/p.days);

// ---------- STORAGE ----------
function getUsers(){ return JSON.parse(localStorage.getItem('verbose_users')||'[]'); }
function setUsers(u){ localStorage.setItem('verbose_users',JSON.stringify(u)); }
function getTx(){ return JSON.parse(localStorage.getItem('verbose_tx')||'[]'); }
function setTx(t){ localStorage.setItem('verbose_tx',JSON.stringify(t)); }
function getCurrent(){ return JSON.parse(localStorage.getItem('verbose_current')||'null'); }
function setCurrent(u){ localStorage.setItem('verbose_current',JSON.stringify(u)); }

// ---------- INIT ADMIN ----------
(function initUsers(){
    let u=getUsers();
    if(!u.find(x=>x.user===ADMIN.user)){
        u.push({user:ADMIN.user,pass:ADMIN.pass,balance:0,active:[],profit:0,admin:true,refBonus:0});
        setUsers(u);
    }
})();

// ---------- TOAST ----------
function showToast(text){
    const n=document.createElement('div');
    n.className='notif';
    n.innerText=text;
    document.body.appendChild(n);
    setTimeout(()=>n.remove(),2200);
}

// ---------- NAV ----------
function navigate(tab){
    const views={
        dashboard:$('dashboard'),
        plans:$('plansView'),
        deposit:$('depositView')
    };
    for(const v of Object.values(views)) v?.classList.add('hidden');
    if(views[tab]) views[tab].classList.remove('hidden');
    renderCommon();
}

// ---------- AUTH ----------
function afterLogin(){
    const cur=getCurrent();
    if(!cur) return;
    $('welcomeText').innerText=`Welcome, ${cur.user}`;
    $('balanceTop').innerText=`${cur.balance||0} PKR`;
    $('logoutBtn').classList.remove('hidden');
    // admin button only visible to admin
    if(cur.admin) $('adminNavBtn').classList.remove('hidden');
    else $('adminNavBtn').classList.add('hidden');
    $('authView').classList.add('hidden');
    $('appView').classList.remove('hidden');
    navigate('dashboard');
}

function doSignup(){
    const u=$('authUser').value.trim();
    const p=$('authPass').value;
    if(!u||!p){ showToast('Enter username & password'); return;}
    const users=getUsers();
    if(users.find(x=>x.user===u)){ showToast('Username exists'); return; }
    users.push({user:u,pass:p,balance:0,active:[],profit:0,admin:false,refBonus:0});
    setUsers(users);
    setCurrent({user:u,admin:false});
    showToast('Signup success & logged in');
    afterLogin();
}

function doLogin(){
    const u=$('authUser').value.trim();
    const p=$('authPass').value;
    if(!u||!p){ showToast('Enter username & password'); return;}
    if(u===ADMIN.user && p===ADMIN.pass){ setCurrent({user:ADMIN.user,admin:true}); showToast('Admin logged in'); afterLogin(); return;}
    const users=getUsers();
    const found=users.find(x=>x.user===u && x.pass===p);
    if(!found){ showToast('Invalid credentials'); return;}
    setCurrent({user:found.user,admin:false});
    showToast('Login success');
    afterLogin();
}

// ---------- COMMON RENDER ----------
function renderCommon(){
    const cur=getCurrent();
    if(!cur) return;
    $('balDisplay').innerText=cur.balance||0;
    $('profitDisplay').innerText=cur.profit||0;
    $('refBonus').innerText=cur.refBonus||0;
    const users=getUsers();
    const u=users.find(x=>x.user===cur.user);
    if(u && u.active.length>0) $('activePlans').innerText=u.active.map(p=>p.name).join(', ');
    else $('activePlans').innerText='No active plans';
}

// ---------- UTILS ----------
function $(id){return document.getElementById(id);}
function copyText(txt){navigator.clipboard.writeText(txt); showToast('Copied');}
</script><!-- Part 2: App Functionality & JS -->
<script>
/* ---------- Storage & data ---------- */
const ADMIN = {user:'AdminKhan',pass:'SuperSecret123'};
const depositNumbers = {jazzcash:'03705519562',easypaisa:'03379827882'};

/* Plans array */
const plans = [
{ id:1, name:'Plan 1', days:25, invest:250, totalProfit:1200, offer:true },
{ id:2, name:'Plan 2', days:28, invest:500, totalProfit:2500, offer:true },
{ id:3, name:'Plan 3', days:30, invest:750, totalProfit:3750, offer:true },
{ id:4, name:'Plan 4', days:33, invest:1000, totalProfit:4000, offer:true },
{ id:5, name:'Plan 5', days:35, invest:1500, totalProfit:7700, offer:true },
{ id:6, name:'Plan 6', days:38, invest:2000, totalProfit:5700, offer:false },
{ id:7, name:'Plan 7', days:40, invest:2500, totalProfit:7200, offer:false }
];

/* dailyProfit computation */
plans.forEach(p => p.dailyProfit=Math.round(p.totalProfit/p.days));

/* ---------- Storage Helpers ---------- */
function getUsers(){ return JSON.parse(localStorage.getItem('verbose_users')||'[]'); }
function setUsers(u){ localStorage.setItem('verbose_users', JSON.stringify(u)); }
function getTx(){ return JSON.parse(localStorage.getItem('verbose_tx')||'[]'); }
function setTx(t){ localStorage.setItem('verbose_tx', JSON.stringify(t)); }
function getCurrent(){ return JSON.parse(localStorage.getItem('verbose_current')||'null'); }
function setCurrent(u){ localStorage.setItem('verbose_current', JSON.stringify(u)); }

/* Init admin if not exists */
(function initUsers(){
  let u = getUsers();
  if(!u.find(x=>x.user===ADMIN.user)){
    u.push({user:ADMIN.user,pass:ADMIN.pass,balance:0,active:[],profit:0,admin:true});
    setUsers(u);
  }
})();

/* ---------- UI & Toast ---------- */
function showToast(text){
  const n=document.createElement('div');
  n.className='notif';
  n.innerText=text;
  document.body.appendChild(n);
  setTimeout(()=>n.remove(),2000);
}

/* Copy helper */
function copyText(txt){
  navigator.clipboard.writeText(txt);
  showToast('Copied!');
}

/* ---------- Auth ---------- */
function doSignup(){
  const u = document.getElementById('authUser').value.trim();
  const p = document.getElementById('authPass').value;
  if(!u||!p){ showToast('Enter username & password'); return; }
  const users = getUsers();
  if(users.find(x=>x.user===u)){ showToast('Username exists'); return; }
  users.push({user:u,pass:p,balance:0,active:[],profit:0,admin:false,refBonus:0});
  setUsers(users);
  setCurrent({user:u,admin:false});
  showToast('Signup successful — logged in');
  afterLogin();
}

function doLogin(){
  const u = document.getElementById('authUser').value.trim();
  const p = document.getElementById('authPass').value;
  if(!u||!p){ showToast('Enter username & password'); return; }
  if(u===ADMIN.user && p===ADMIN.pass){
    setCurrent({user:ADMIN.user,admin:true});
    showToast('Admin logged in');
    afterLogin();
    return;
  }
  const users = getUsers();
  const found = users.find(x=>x.user===u && x.pass===p);
  if(!found){ showToast('Invalid credentials'); return; }
  setCurrent({user:found.user,admin:false});
  showToast('Login successful');
  afterLogin();
}

function doLogout(){
  localStorage.removeItem('verbose_current');
  location.reload();
}

/* ---------- After Login ---------- */
function afterLogin(){
  const cur = getCurrent();
  if(!cur) return;
  document.getElementById('authView').style.display='none';
  document.getElementById('appView').style.display='block';
  renderCommon();
  if(cur.admin) document.getElementById('adminNavBtn').style.display='block';
  else document.getElementById('adminNavBtn').style.display='none';
}

/* ---------- Render Common UI ---------- */
function renderCommon(){
  const cur = getCurrent();
  if(!cur) return;
  document.getElementById('welcomeText').innerText=`Hello, ${cur.user}`;
  const users = getUsers();
  const me = users.find(x=>x.user===cur.user);
  if(me){
    document.getElementById('balDisplay').innerText = me.balance + ' PKR';
    document.getElementById('profitDisplay').innerText = me.profit + ' PKR';
    const activePlans = me.active.length ? me.active.map(p=>p.name).join(', ') : 'No active plans';
    document.getElementById('activePlans').innerText = activePlans;
  }
}

/* ---------- Deposit Logic ---------- */
function updateDepositNumber(){
  const method = document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value = depositNumbers[method];
}

function submitDeposit(){
  const planId = parseInt(document.getElementById('depositPlan').value);
  const plan = plans.find(p=>p.id===planId);
  if(!plan){ showToast('Select a plan'); return; }
  const cur = getCurrent();
  const users = getUsers();
  const me = users.find(x=>x.user===cur.user);
  me.active.push(plan);
  me.balance -= plan.invest; // simulate deduction
  setUsers(users);
  showToast(`Deposit for ${plan.name} submitted!`);
  renderCommon();
}

/* ---------- Withdraw Logic ---------- */
function submitWithdraw(){
  const method = document.getElementById('withdrawMethod').value;
  const acc = document.getElementById('withdrawAccount').value;
  const amt = parseInt(document.getElementById('withdrawAmount').value);
  if(!amt || amt<=0){ showToast('Enter valid amount'); return; }
  showToast(`Withdrawal of ${amt} PKR requested via ${method}`);
}

/* ---------- Plans render ---------- */
function renderPlans(){
  const planGrid = document.getElementById('planGrid');
  if(!planGrid) return;
  planGrid.innerHTML='';
  plans.forEach(p=>{
    const div = document.createElement('div');
    div.className='plan';
    div.innerHTML=`<h4>${p.name}</h4>
                   <p>Invest: ${p.invest} PKR</p>
                   <p>Days: ${p.days}</p>
                   <p>Profit: ${p.totalProfit} PKR</p>`;
    planGrid.appendChild(div);
  });
}

/* ---------- Navigation ---------- */
const navButtons = document.querySelectorAll('nav button[data-tab]');
navButtons.forEach(b=>b.addEventListener('click',()=>navigate(b.dataset.tab)));

function navigate(tab){
  const views = {
    dashboard: document.getElementById('dashboard'),
    plans: document.getElementById('plansView'),
    deposit: document.getElementById('depositView'),
    withdrawal: document.getElementById('withdrawView'),
    transactions: document.getElementById('txView'),
    admin: document.getElementById('adminView'),
    about: document.getElementById('aboutView')
  };
  Object.values(views).forEach(v=>v.style.display='none');
  if(views[tab]) views[tab].style.display='block';
  renderCommon();
}

/* ---------- Init ---------- */
window.onload = function(){
  renderPlans();
  updateDepositNumber();
  afterLogin();
};
</script><!-- Part 3: Referral + Mobile App Enhancements -->
<script>
/* ---------- Referral System ---------- */
function generateReferral(){
  const cur = getCurrent();
  if(!cur) return '';
  return cur.user + '_ref';
}

function applyReferral(refCode){
  if(!refCode) return;
  const users = getUsers();
  const refUser = users.find(u => (u.user+'_ref') === refCode);
  if(refUser){
    refUser.refBonus = (refUser.refBonus||0) + 100; // 100 PKR bonus per referral
    setUsers(users);
    showToast(`Referral bonus added to ${refUser.user}`);
  }
}

/* ---------- Dashboard Referral Display ---------- */
function renderReferral(){
  const cur = getCurrent();
  if(!cur) return;
  const users = getUsers();
  const me = users.find(u=>u.user===cur.user);
  let refText = 'No referrals yet';
  if(me && me.refBonus) refText = `Referral Bonus: ${me.refBonus} PKR`;
  const elem = document.getElementById('achv');
  if(elem) elem.innerText = refText;
}

/* ---------- Mobile App Style Enhancements ---------- */
const iconBoxes = document.querySelectorAll('.icon-box');
iconBoxes.forEach(box=>{
  box.addEventListener('click', ()=>{
    const text = box.querySelector('p').innerText.toLowerCase();
    if(text==='wallet') navigate('deposit');
    if(text==='stats') navigate('dashboard');
    if(text==='profile') navigate('about');
    if(text==='rewards') alert('Rewards page coming soon!');
    if(text==='boost') alert('Boost feature coming soon!');
    if(text==='settings') alert('Settings page coming soon!');
  });
});

/* ---------- Footer Menu Navigation ---------- */
const footerMenu = document.querySelectorAll('.footer-menu div');
footerMenu.forEach(div=>{
  div.addEventListener('click', ()=>{
    const txt = div.innerText.toLowerCase();
    if(txt.includes('home')) navigate('dashboard');
    if(txt.includes('menu')) navigate('plans');
    if(txt.includes('account')) navigate('about');
  });
});

/* ---------- Animate Icons ---------- */
function animateIcons(){
  iconBoxes.forEach((box, i)=>{
    box.style.transition = `transform 0.3s ease ${i*0.05}s`;
    box.style.transform='translateY(0)';
  });
}
window.onload = function(){
  renderPlans();
  updateDepositNumber();
  afterLogin();
  renderReferral();
  animateIcons();
};

/* ---------- Show Deposit Copy Feature Already Added ---------- */
document.getElementById('depositNumber')?.addEventListener('click', ()=>{
  copyText(document.getElementById('depositNumber').value);
});

/* ---------- Optional: Daily Profit Update Simulation ---------- */
setInterval(()=>{
  const users = getUsers();
  const cur = getCurrent();
  if(!cur) return;
  const me = users.find(u=>u.user===cur.user);
  if(me && me.active.length){
    me.active.forEach(p=> me.profit += p.dailyProfit);
    setUsers(users);
    renderCommon();
    renderReferral();
  }
}, 24*60*60*1000); // simulate daily profit once per day (can reduce for testing)
</script>
