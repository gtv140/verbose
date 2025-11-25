<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE Premium App</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
/* ---------- Reset & Base ---------- */
*{margin:0;padding:0;box-sizing:border-box;font-family:'Orbitron',sans-serif;}
body, html{height:100%;background:#050012;color:#fff;overflow-x:hidden;transition:all 0.3s;}
.hidden{display:none;}
.muted{opacity:0.7;font-size:13px;}

/* ---------- Background ---------- */
.bg-gradient{position:fixed;inset:0;z-index:-2;background:linear-gradient(270deg,#0f0530,#1b0a4a,#061022);background-size:800% 800%;animation:grad 20s ease infinite;filter:blur(28px) saturate(120%);}
@keyframes grad{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}

/* ---------- Header ---------- */
.app-header{display:flex;justify-content:space-between;align-items:center;padding:15px;background:#111;box-shadow:0 2px 12px rgba(0,255,240,0.3);}
.app-header h1{color:#0ff;font-size:22px;text-shadow:0 0 12px #0ff;}
.menu-btn{font-size:22px;color:#0ff;cursor:pointer;}

/* ---------- Grid & Cards ---------- */
.icon-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(100px,1fr));gap:15px;padding:20px;}
.icon-box{background:#111;padding:20px;text-align:center;border-radius:15px;box-shadow:0 0 12px #0ff;transition:0.3s;cursor:pointer;}
.icon-box:hover{transform:scale(1.05);box-shadow:0 0 24px #0ff;}
.icon-box i{font-size:28px;margin-bottom:8px;color:#0ff;}
.icon-box p{font-size:14px;color:#cfe;}
.card{background:rgba(255,255,255,0.03);border:1px solid rgba(0,255,255,0.06);padding:18px;border-radius:14px;box-shadow:0 8px 30px rgba(0,0,0,0.6);backdrop-filter:blur(6px);margin:15px;transition:0.3s;}
.card:hover{transform:translateY(-4px);box-shadow:0 12px 36px rgba(0,255,240,0.15);}

/* ---------- Inputs & Buttons ---------- */
input,select{width:100%;padding:10px;margin-top:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:rgba(0,0,0,0.35);color:#fff;outline:none;transition:0.2s;}
input:focus,select:focus{border-color:#0ff;}
button.primary{background:#0ff;color:#000;padding:10px 12px;border-radius:10px;border:none;cursor:pointer;font-weight:700;margin-top:10px;transition:0.2s;}
button.primary:hover{background:#0ae;}
button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:10px;border-radius:8px;color:#fff;cursor:pointer;margin-left:8px;transition:0.2s;}
button.ghost:hover{background:rgba(0,255,240,0.1);}

/* ---------- Footer Menu ---------- */
.footer-menu{position:fixed;bottom:0;left:0;width:100%;background:#111;display:flex;justify-content:space-around;padding:10px 0;box-shadow:0 -2px 12px rgba(0,255,240,0.3);}
.footer-menu div{text-align:center;color:#fff;font-size:12px;cursor:pointer;}
.footer-menu i{display:block;font-size:22px;margin-bottom:4px;color:#0ff;}

/* ---------- Plans ---------- */
.plan-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:12px;margin-top:12px;}
.plan{border-radius:12px;padding:12px;border:1px solid rgba(0,255,240,0.06);background:linear-gradient(180deg,rgba(0,255,240,0.02),transparent);position:relative;transition:transform .18s;cursor:pointer;}
.plan:hover{transform:translateY(-6px);box-shadow:0 12px 36px rgba(0,255,240,0.1);}
.badge{position:absolute;top:10px;right:10px;background:#ffea00;color:#000;padding:4px 6px;border-radius:8px;font-weight:800;font-size:11px;}

/* ---------- Table ---------- */
table{width:100%;border-collapse:collapse;margin-top:10px;}
th,td{padding:8px;border-bottom:1px solid rgba(255,255,255,0.03);font-size:13px;color:#0ff;}

/* ---------- Toast ---------- */
.notif{position:fixed;right:18px;bottom:18px;background:#0ff;color:#000;padding:12px 16px;border-radius:12px;font-weight:700;box-shadow:0 10px 30px rgba(0,255,240,0.2);z-index:99;}

/* ---------- Auth ---------- */
.auth-wrap{display:flex;flex-direction:column;align-items:center;justify-content:center;padding:30px;border-radius:14px;box-shadow:0 8px 30px rgba(0,255,240,0.2);}
.auth-wrap h2{text-align:center;margin-bottom:15px;}
</style>
</head>
<body>

<div class="bg-gradient"></div>
<canvas id="particles"></canvas>

<div class="app-header">
<h1>VERBOSE</h1>
<i class="fa fa-bars menu-btn"></i>
</div>

<!-- Login / Signup -->
<div id="authView" class="card auth-wrap">
<h2>Login / Signup</h2>
<input id="authUser" placeholder="Username">
<input id="authPass" placeholder="Password" type="password">
<div style="display:flex;margin-top:10px;">
<button class="primary" onclick="doLogin()">Login</button>
<button class="ghost" onclick="doSignup()">Signup</button>
</div>
<div class="muted small" style="margin-top:10px;">Admin: <strong>AdminKhan</strong> / <strong>SuperSecret123</strong></div>
</div>

<!-- App Dashboard -->
<div id="appView" class="hidden">
<div class="icon-grid" id="dashboardIcons"></div>

<!-- Plans -->
<div id="plansView" class="card hidden">
<h2>Plans</h2>
<div class="plan-grid" id="planGrid"></div>
</div>

<!-- Deposit -->
<div id="depositView" class="card hidden">
<h2>Deposit</h2>
<label>Choose Plan</label>
<select id="depositPlan" onchange="updateDepositAmount()"></select>
<label>Amount</label>
<input id="depositAmount" readonly>
<label>Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="depositNumber" readonly>
<button class="ghost" onclick="copyText(document.getElementById('depositNumber').value)">Copy</button>
<label>Transaction ID</label>
<input id="depositTxId" placeholder="TX ID">
<label>Upload Proof</label>
<input type="file" id="depositProof">
<button class="primary" onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- Withdraw -->
<div id="withdrawView" class="card hidden">
<h2>Withdraw</h2>
<label>Amount PKR</label>
<input id="withdrawAmount" type="number">
<label>Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<button class="primary" onclick="submitWithdraw()">Request Withdraw</button>
</div>

<!-- Transactions -->
<div id="txView" class="card hidden">
<h2>Transactions</h2>
<table>
<thead><tr><th>Type</th><th>Amount</th><th>Plan</th><th>Days</th><th>Total Profit</th><th>Time</th><th>Status</th></tr></thead>
<tbody id="txTableBody"></tbody>
</table>
</div>

<!-- Profile -->
<div id="profileView" class="card hidden">
<h2>Profile</h2>
<label>Display Name</label>
<input id="profileName">
<label>Profile Picture</label>
<input type="file" id="profilePic">
<div id="profileStats" class="muted small"></div>
<button class="primary" onclick="updateProfile()">Update Profile</button>
</div>

<!-- About -->
<div id="aboutView" class="card hidden">
<h2>About VERBOSE</h2>
<p class="muted small">VERBOSE ek modern premium earning platform hai neon style dashboard ke sath. Launch: 25 Nov 2025.</p>
</div>

<div class="footer-menu">
<div onclick="navigate('wallet')"><i class="fa fa-wallet"></i>Wallet</div>
<div onclick="navigate('plans')"><i class="fa fa-briefcase"></i>Plans</div>
<div onclick="navigate('deposit')"><i class="fa fa-money-bill-wave"></i>Deposit</div>
<div onclick="navigate('withdraw')"><i class="fa fa-credit-card"></i>Withdraw</div>
<div onclick="navigate('transactions')"><i class="fa fa-file-invoice"></i>Transactions</div>
</div>

<div id="toastRoot"></div>

<script>
// ---------- Particles ----------
const canvas=document.getElementById('particles'); const ctx=canvas.getContext('2d');
function resizeCanvas(){canvas.width=innerWidth;canvas.height=innerHeight;}
resizeCanvas(); addEventListener('resize',resizeCanvas);
const particles=[]; for(let i=0;i<140;i++){particles.push({x:Math.random()*innerWidth,y:Math.random()*innerHeight,r:Math.random()*1.6+0.6,vx:(Math.random()-0.5)*0.6,vy:(Math.random()-0.5)*0.6,h:180+Math.random()*80});}
function drawParticles(){ctx.clearRect(0,0,canvas.width,canvas.height);for(const p of particles){ctx.beginPath();ctx.fillStyle=`hsla(${p.h},100%,60%,0.12)`;ctx.shadowBlur=12;ctx.shadowColor=`hsla(${p.h},100%,60%,0.14)`;ctx.fillRect(p.x,p.y,p.r*2,p.r*2);p.x+=p.vx;p.y+=p.vy;if(p.x<0)p.x=canvas.width;if(p.x>canvas.width)p.x=0;if(p.y<0)p.y=canvas.height;if(p.y>canvas.height)p.y=0;}requestAnimationFrame(drawParticles);}
drawParticles();

// ---------- Admin & Storage ----------
const ADMIN={user:'AdminKhan',pass:'SuperSecret123'};
const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
const plans=[]; for(let i=1;i<=25;i++){plans.push({id:i,name:`Plan ${i}`,days:20+i,invest:100*i,totalProfit:100*i*(1+i*0.5),offer:true,dailyProfit:Math.round((100*i*(1+i*0.5))/(20+i))});}
const dashboardIcons=[
{icon:'fa-wallet',name:'Wallet',view:'wallet'},
{icon:'fa-briefcase',name:'Plans',view:'plans'},
{icon:'fa-money-bill-wave',name:'Deposit',view:'deposit'},
{icon:'fa-credit-card',name:'Withdraw',view:'withdraw'},
{icon:'fa-file-invoice',name:'Transactions',view:'transactions'},
{icon:'fa-user',name:'Profile',view:'profile'},
{icon:'fa-info-circle',name:'About',view:'about'},
{icon:'fa-cog',name:'Settings',view:'settings'},
{icon:'fa-bell',name:'Notifications',view:'notifications'},
{icon:'fa-question-circle',name:'Help',view:'help'},
{icon:'fa-right-from-bracket',name:'Logout',view:'logout'}
];

function getUsers(){return JSON.parse(localStorage.getItem('verbose_users')||'[]');}
function setUsers(u){localStorage.setItem('verbose_users',JSON.stringify(u));}
function getTx(){return JSON.parse(localStorage.getItem('verbose_tx')||'[]');}
function setTx(t){localStorage.setItem('verbose_tx',JSON.stringify(t));}
function getCurrent(){return JSON.parse(localStorage.getItem('verbose_current')||'null');}
function setCurrent(u){localStorage.setItem('verbose_current',JSON.stringify(u));}

// ---------- Toast ----------
function showToast(text){const n=document.createElement('div');n.className='notif';n.innerText=text;document.body.appendChild(n);setTimeout(()=>n.remove(),2200);}

// ---------- Init Admin ----------
(function initUsers(){let u=getUsers();if(!u.find(x=>x.user===ADMIN.user)){u.push({user:ADMIN.user,pass:ADMIN.pass,balance:0,active:[],profit:0,admin:true});setUsers(u);}})();

// ---------- Auth ----------
function afterLogin(){const cur=getCurrent();if(!cur)return;document.getElementById('authView').classList.add('hidden');document.getElementById('appView').classList.remove('hidden');renderDashboard();renderPlans();renderDepositPlans();renderTransactions();renderProfile();}
function doSignup(){const u=document.getElementById('authUser').value.trim();const p=document.getElementById('authPass').value;if(!u||!p){showToast('Enter username & password');return;}const users=getUsers();if(users.find(x=>x.user===u)){showToast('Username exists');return;}users.push({user:u,pass:p,balance:0,active:[],profit:0,admin:false});setUsers(users);setCurrent({user:u,admin:false});showToast('Signup successful');afterLogin();}
function doLogin(){const u=document.getElementById('authUser').value.trim();const p=document.getElementById('authPass').value;if(!u||!p){showToast('Enter username & password');return;}if(u===ADMIN.user&&p===ADMIN.pass){setCurrent({user:ADMIN.user,admin:true});showToast('Admin logged in');afterLogin();return;}const users=getUsers();const found=users.find(x=>x.user===u&&x.pass===p);if(!found){showToast('Invalid credentials');return;}setCurrent({user:found.user,admin:false});showToast('Login successful');afterLogin();}
function doLogout(){localStorage.removeItem('verbose_current');document.getElementById('appView').classList.add('hidden');document.getElementById('authView').classList.remove('hidden');showToast('Logged out');}

// ---------- Navigation ----------
function navigate(view){
const views=['plansView','depositView','withdrawView','txView','aboutView','profileView','settingsView','notificationsView','helpView'];views.forEach(v=>{let el=document.getElementById(v); if(el) el.classList.add('hidden');});
if(view==='plans') document.getElementById('plansView').classList.remove('hidden');
if(view==='deposit') document.getElementById('depositView').classList.remove('hidden');
if(view==='withdraw') document.getElementById('withdrawView').classList.remove('hidden');
if(view==='transactions'){ document.getElementById('txView').classList.remove('hidden'); renderTransactions();}
if(view==='about') document.getElementById('aboutView').classList.remove('hidden');
if(view==='profile'){ document.getElementById('profileView').classList.remove('hidden'); renderProfile();}
if(view==='logout'){doLogout();}
}

// ---------- Dashboard ----------
function renderDashboard(){
const grid=document.getElementById('dashboardIcons');grid.innerHTML='';
dashboardIcons.forEach(d=>{const div=document.createElement('div');div.className='icon-box';div.innerHTML=`<i class="fa ${d.icon}"></i><p>${d.name}</p>`;div.onclick=()=>navigate(d.view);grid.appendChild(div);});
}

// ---------- Plans ----------
function renderPlans(){const grid=document.getElementById('planGrid');grid.innerHTML='';plans.forEach(p=>{let div=document.createElement('div');div.className='plan';div.innerHTML=`<h4>${p.name}</h4><p>Invest: ${p.invest} PKR</p><p>Days: ${p.days}</p><p>Total: ${p.totalProfit}</p><div class="badge">24H OFFER</div>`;grid.appendChild(div);});}
function renderDepositPlans(){const select=document.getElementById('depositPlan');select.innerHTML='';plans.forEach(p=>{const opt=document.createElement('option');opt.value=p.id;opt.innerText=`${p.name} - ${p.invest} PKR`;select.appendChild(opt);});updateDepositAmount();}
function updateDepositAmount(){const planId=parseInt(document.getElementById('depositPlan').value);const plan=plans.find(p=>p.id===planId);document.getElementById('depositAmount').value = plan ? plan.invest : '';updateDepositNumber();}
function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value = depositNumbers[method];}
function copyText(txt){navigator.clipboard.writeText(txt);showToast('Copied to clipboard');}
function submitDeposit(){showToast('Deposit submitted');}
function submitWithdraw(){showToast('Withdraw requested');}
function renderTransactions(){const tbody=document.getElementById('txTableBody');tbody.innerHTML='';const tx=getTx();tx.forEach(t=>{const tr=document.createElement('tr');tr.innerHTML=`<td>${t.type}</td><td>${t.amount}</td><td>${t.plan}</td><td>${t.days}</td><td>${t.total}</td><td>${t.time}</td><td>${t.status}</td>`;tbody.appendChild(tr);});}
function renderProfile(){const cur=getCurrent();if(!cur) return;document.getElementById('profileName').value = cur.user;
document.getElementById('profileStats').innerText = `Balance: ${cur.balance || 0} PKR | Profit: ${cur.profit || 0}`;}

function updateProfile() {
    const cur = getCurrent();
    if (!cur) return;
    const users = getUsers();
    const userIndex = users.findIndex(u => u.user === cur.user);
    if (userIndex < 0) return;
    const newName = document.getElementById('profileName').value.trim();
    if (newName) users[userIndex].user = newName;
    setUsers(users);
    setCurrent({...cur, user:newName});
    showToast('Profile updated');
    renderDashboard();
}

// ---------- Init ----------
window.onload = function() {
    const current = getCurrent();
    if (current) afterLogin();
    else document.getElementById('authView').classList.remove('hidden');
}

// ---------- Copy Helper ----------
function copyText(txt){navigator.clipboard.writeText(txt);showToast('Copied to clipboard');}

// ---------- Deposit & Withdraw ----------
function submitDeposit(){
    const planId = document.getElementById('depositPlan').value;
    const amount = document.getElementById('depositAmount').value;
    const method = document.getElementById('depositMethod').value;
    const txId = document.getElementById('depositTxId').value.trim();
    if(!txId){showToast('Enter TX ID'); return;}
    const txList = getTx();
    txList.push({type:'Deposit',amount,plan:planId,days:'-',total:'-',time:new Date().toLocaleString(),status:'Pending'});
    setTx(txList);
    showToast('Deposit Submitted');
    renderTransactions();
}

function submitWithdraw(){
    const amount = document.getElementById('withdrawAmount').value;
    const method = document.getElementById('withdrawMethod').value;
    if(!amount || amount <= 0){showToast('Enter valid amount'); return;}
    const txList = getTx();
    txList.push({type:'Withdraw',amount,plan:'-',days:'-',total:'-',time:new Date().toLocaleString(),status:'Requested'});
    setTx(txList);
    showToast('Withdraw Requested');
    renderTransactions();
}
</script>

</body>
</html>
