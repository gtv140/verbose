<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEON DASH App</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Orbitron',sans-serif;}
body, html{height:100%;background:#050010;color:#fff;overflow-x:hidden;}
.hidden{display:none;}
.muted{opacity:0.7;font-size:13px;}
.bg-gradient{position:fixed;inset:0;z-index:-2;background:linear-gradient(135deg,#0f0030,#1b004a,#061022);background-size:400% 400%;animation:grad 20s ease infinite;}
@keyframes grad{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
.app-header{display:flex;justify-content:space-between;align-items:center;padding:15px;background:rgba(0,0,0,0.4);backdrop-filter:blur(12px);}
.app-header h1{color:#0ff;font-size:26px;text-shadow:0 0 14px #0ff;}
.menu-btn{font-size:26px;color:#0ff;cursor:pointer;}
.icon-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(90px,1fr));gap:15px;padding:20px;}
.icon-box{background:rgba(0,0,0,0.35);padding:18px;text-align:center;border-radius:20px;box-shadow:0 0 15px #0ff;transition:0.3s;cursor:pointer;}
.icon-box:hover{transform:scale(1.1);box-shadow:0 0 28px #0ff;}
.icon-box i{font-size:30px;margin-bottom:6px;color:#0ff;}
.icon-box p{font-size:12px;color:#cfe;}
.auth-wrap{display:flex;flex-direction:column;align-items:center;justify-content:center;height:90vh;padding:35px;background:rgba(0,0,0,0.35);border-radius:16px;margin:30px;}
.auth-wrap h2{text-align:center;margin-bottom:18px;}
input,select{width:100%;padding:12px;margin-top:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.06);background:rgba(0,0,0,0.25);color:#fff;outline:none;transition:0.2s;}
input:focus,select:focus{border-color:#0ff;}
button.primary{background:#0ff;color:#000;padding:12px 15px;border-radius:12px;border:none;cursor:pointer;font-weight:700;margin-top:12px;transition:0.2s;}
button.primary:hover{background:#0ae;}
button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:12px;border-radius:10px;color:#fff;cursor:pointer;margin-left:8px;transition:0.2s;}
button.ghost:hover{background:rgba(0,255,240,0.1);}
.footer-menu{position:fixed;bottom:0;left:0;width:100%;background:rgba(0,0,0,0.5);display:flex;justify-content:space-around;padding:12px 0;backdrop-filter:blur(12px);}
.footer-menu div{text-align:center;color:#fff;font-size:12px;cursor:pointer;}
.footer-menu i{display:block;font-size:22px;margin-bottom:4px;color:#0ff;}
.notif{position:fixed;right:18px;bottom:18px;background:#0ff;color:#000;padding:14px 18px;border-radius:14px;font-weight:700;box-shadow:0 12px 35px rgba(0,255,240,0.25);z-index:99;}
.card{background:rgba(255,255,255,0.02);border:1px solid rgba(0,255,255,0.06);padding:20px;border-radius:16px;box-shadow:0 10px 36px rgba(0,0,0,0.6);backdrop-filter:blur(6px);margin:18px;transition:0.3s;}
.card:hover{transform:translateY(-5px);box-shadow:0 15px 40px rgba(0,255,240,0.18);}
.plan-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:15px;margin-top:15px;}
.plan{border-radius:14px;padding:16px;border:1px solid rgba(0,255,240,0.06);background:linear-gradient(180deg,rgba(0,255,240,0.02),transparent);position:relative;transition:transform .18s;cursor:pointer;}
.plan:hover{transform:translateY(-6px);box-shadow:0 15px 40px rgba(0,255,240,0.12);}
.badge{position:absolute;top:10px;right:10px;background:#ffea00;color:#000;padding:5px 7px;border-radius:10px;font-weight:800;font-size:11px;}
table{width:100%;border-collapse:collapse;margin-top:12px;}
th,td{padding:10px;border-bottom:1px solid rgba(255,255,255,0.03);font-size:13px;color:#0ff;}
</style>
</head>
<body>

<div class="bg-gradient"></div>

<!-- LOGIN -->
<div id="authView" class="auth-wrap">
<h2>Login / Signup</h2>
<input id="authUser" placeholder="Username">
<input id="authPass" placeholder="Password" type="password">
<div style="display:flex;margin-top:12px;">
<button class="primary" onclick="doLogin()">Login</button>
<button class="ghost" onclick="doSignup()">Signup</button>
</div>
<div class="muted small" style="margin-top:12px;">Admin: <strong>AdminKhan</strong> / <strong>SuperSecret123</strong></div>
</div>

<!-- DASHBOARD -->
<div id="appView" class="hidden">
<div class="app-header">
<h1>NEON DASH</h1>
<i class="fa fa-bars menu-btn"></i>
</div>

<div class="icon-grid" id="dashboardIcons">
<!-- Icons will be generated dynamically -->
</div>

<div class="footer-menu" id="footerMenu"></div>

<!-- Main Views -->
<div id="mainView" class="card hidden"></div>

<div id="toastRoot"></div>
</div>

<script>
// ---------- Admin ----------
const ADMIN={user:'AdminKhan',pass:'SuperSecret123'};
const plans=[]; for(let i=1;i<=25;i++){plans.push({id:i,name:`Plan ${i}`,days:20+i,invest:100*i,totalProfit:100*i*(1+i*0.5)});}

// ---------- Users ----------
function getUsers(){return JSON.parse(localStorage.getItem('dash_users')||'[]');}
function setUsers(u){localStorage.setItem('dash_users',JSON.stringify(u));}
function getCurrent(){return JSON.parse(localStorage.getItem('dash_current')||'null');}
function setCurrent(u){localStorage.setItem('dash_current',JSON.stringify(u));}

// ---------- Toast ----------
function showToast(text){const n=document.createElement('div');n.className='notif';n.innerText=text;document.body.appendChild(n);setTimeout(()=>n.remove(),2200);}

// ---------- Login / Signup ----------
function doSignup(){
const u=document.getElementById('authUser').value.trim();
const p=document.getElementById('authPass').value;
if(!u||!p){showToast('Enter username & password');return;}
let users=getUsers();
if(users.find(x=>x.user===u)){showToast('Username exists');return;}
users.push({user:u,pass:p,balance:0,active:[],profit:0,tx:[]});
setUsers(users); setCurrent({user:u});
showToast('Signup successful'); afterLogin();
}

function doLogin(){
const u=document.getElementById('authUser').value.trim();
const p=document.getElementById('authPass').value;
if(!u||!p){showToast('Enter username & password');return;}
if(u===ADMIN.user&&p===ADMIN.pass){setCurrent({user:u,admin:true});showToast('Admin logged in'); afterLogin(); return;}
let users=getUsers();
const found=users.find(x=>x.user===u&&x.pass===p);
if(!found){showToast('Invalid credentials');return;}
setCurrent(found);showToast('Login successful');afterLogin();
}

function doLogout(){localStorage.removeItem('dash_current');document.getElementById('appView').classList.add('hidden');document.getElementById('authView').classList.remove('hidden');showToast('Logged out');}

function afterLogin(){
document.getElementById('authView').classList.add('hidden');
document.getElementById('appView').classList.remove('hidden');
generateIcons();
generateFooter();
}

// ---------- Dashboard Icons ----------
const iconList=[
{icon:'fa-wallet',label:'Wallet',action:showWallet},
{icon:'fa-briefcase',label:'Plans',action:showPlans},
{icon:'fa-money-bill-wave',label:'Deposit',action:showDeposit},
{icon:'fa-credit-card',label:'Withdraw',action:showWithdraw},
{icon:'fa-file-invoice',label:'Transactions',action:showTransactions},
{icon:'fa-user',label:'Profile',action:showProfile},
{icon:'fa-comment',label:'Chat',action:()=>showToast('Chat coming soon')},
{icon:'fa-medal',label:'Leaderboard',action:()=>showToast('Leaderboard coming soon')},
{icon:'fa-gift',label:'Rewards',action:()=>showToast('Rewards coming soon')},
{icon:'fa-list-check',label:'Tasks',action:()=>showToast('Tasks coming soon')},
{icon:'fa-cog',label:'Settings',action:()=>showToast('Settings coming soon')},
{icon:'fa-bell',label:'Notifications',action:()=>showToast('Notifications coming soon')},
{icon:'fa-question-circle',label:'Help',action:()=>showToast('Help coming soon')},
{icon:'fa-right-from-bracket',label:'Logout',action:doLogout},
];

function generateIcons(){
const container=document.getElementById('dashboardIcons'); container.innerHTML='';
iconList.forEach(i=>{
let div=document.createElement('div'); div.className='icon-box';
div.innerHTML=`<i class="fa ${i.icon}"></i><p>${i.label}</p>`;
div.onclick=i.action;
container.appendChild(div);
});
}

function generateFooter(){
const footer=document.getElementById('footerMenu'); footer.innerHTML='';
iconList.slice(0,5).forEach(i=>{
let div=document.createElement('div'); div.innerHTML=`<i class="fa ${i.icon}"></i>${i.label}`; div.onclick=i.action;
footer.appendChild(div);
});
}

// ---------- Main Views ----------
function showView(html){const mv=document.getElementById('mainView'); mv.innerHTML=html; mv.classList.remove('hidden');}
function hideView(){document.getElementById('mainView').classList.add('hidden');}

// ---------- Wallet ----------
function showWallet(){
let cur=getCurrent();
showView(`<h2>Wallet</h2><p>Balance: ${cur.balance||0} PKR</p>`);
}

// ---------- Plans ----------
function showPlans(){
let html='<h2>Plans</h2><div class="plan-grid">';
plans.forEach(p=>{html+=`<div class="plan"><h4>${p.name}</h4><p>Invest: ${p.invest}</p><p>Days: ${p.days}</p><p>Total Profit: ${p.totalProfit}</p><div class="badge">24H OFFER</div></div>`;});
html+='</div>'; showView(html);
}

// ---------- Deposit ----------
function showDeposit(){
let html='<h2>Deposit</h2><label>Choose Plan</label><select id="depositPlan">';
plans.forEach(p=>{html+=`<option value="${p.id}">${p.name} - ${p.invest} PKR</option>`;});
html+='</select><label>Amount</label><input id="depositAmount" readonly><button class="primary" onclick="submitDeposit()">Deposit</button>';
showView(html); updateDepositAmount();
}

function updateDepositAmount(){
const sel=document.getElementById('depositPlan'); if(!sel) return;
const planId=parseInt(sel.value); const plan=plans.find(p=>p.id===planId);
document.getElementById('depositAmount').value=plan?plan.invest:0;
}

function submitDeposit(){
const sel=document.getElementById('depositPlan'); const planId=parseInt(sel.value); const plan=plans.find(p=>p.id===planId);
if(!plan) return;
let cur=getCurrent(); cur.balance=(cur.balance||0)+plan.invest; cur.active=cur.active||[]; cur.active.push(plan); cur.tx=cur.tx||[];
cur.tx.push({type:'Deposit',amount:plan.invest,plan:plan.name,time:new Date().toLocaleString()});
setCurrent(cur);
let users=getUsers(); const idx=users.findIndex(u=>u.user===cur.user); if(idx>=0) users[idx]=cur; setUsers(users);
showToast(`Deposited ${plan.invest} PKR`); showWallet();
}

// ---------- Withdraw ----------
function showWithdraw(){
showView(`<h2>Withdraw</h2><label>Amount</label><input type="number" id="withdrawAmount"><button class="primary" onclick="submitWithdraw()">Withdraw</button>`);
}

function submitWithdraw(){
let amt=parseInt(document.getElementById('withdrawAmount').value);
let cur=getCurrent();
if(amt>cur.balance){showToast('Insufficient balance'); return;}
cur.balance-=amt; cur.tx.push({type:'Withdraw',amount:amt,plan:'-',time:new Date().toLocaleString()});
setCurrent(cur);
let users=getUsers(); const idx=users.findIndex(u=>u.user===cur.user); if(idx>=0) users[idx]=cur; setUsers(users);
showToast(`Withdrawn ${amt} PKR`); showWallet();
}

// ---------- Transactions ----------
function showTransactions(){
let cur=getCurrent(); let txs=cur.tx||[];
let html='<h2>Transactions</h2><table><tr><th>Type</th><th>Amount</th><th>Plan</th><th>Time</th></tr>';
txs.forEach(t=>{html+=`<tr><td>${t.type}</td><td>${t.amount}</td><td>${t.plan}</td><td>${t.time}</td></tr>`;});
html+='</table>'; showView(html);
}

// ---------- Profile ----------
function showProfile(){
let cur=getCurrent();
showView(`<h2>Profile</h2><p>Username: ${cur.user}</p><p>Balance: ${cur.balance||0} PKR</p>`);
}

</script>
</body>
</html>
