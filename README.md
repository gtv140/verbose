<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>VERBOSE Premium App</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
body, html{margin:0;padding:0;height:100%;font-family:'Orbitron',sans-serif;background:#050012;color:#fff;overflow-x:hidden;}
.bg-gradient{position:fixed;inset:0;z-index:-2;background:linear-gradient(270deg,#0f0530,#1b0a4a,#061022);background-size:800% 800%;animation:grad 18s ease infinite;filter:blur(28px) saturate(120%);opacity:0.9;}
@keyframes grad{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
#particles{position:fixed;inset:0;z-index:-1;pointer-events:none;}
.app-header{display:flex;justify-content:space-between;align-items:center;padding:15px;background:#111;box-shadow:0 2px 12px rgba(0,255,240,0.3);}
.app-header h1{margin:0;color:#0ff;text-shadow:0 0 12px #0ff;font-size:20px;}
.menu-btn{font-size:22px;color:#0ff;cursor:pointer;}
.icon-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:15px;padding:20px;}
.icon-box{background:#111;padding:20px;text-align:center;border-radius:15px;box-shadow:0 0 12px #0ff;transition:0.3s;cursor:pointer;}
.icon-box:hover{transform:scale(1.05);box-shadow:0 0 24px #0ff;}
.icon-box i{font-size:30px;margin-bottom:10px;color:#0ff;}
.icon-box p{margin:0;font-size:14px;color:#cfe;}
.footer-menu{position:fixed;bottom:0;left:0;width:100%;background:#111;display:flex;justify-content:space-around;padding:10px 0;box-shadow:0 -2px 12px rgba(0,255,240,0.3);}
.footer-menu div{text-align:center;color:#fff;font-size:12px;cursor:pointer;}
.footer-menu i{display:block;font-size:22px;margin-bottom:5px;color:#0ff;}
.card{background:rgba(255,255,255,0.03);border:1px solid rgba(0,255,255,0.06);padding:14px;border-radius:12px;box-shadow:0 8px 30px rgba(0,0,0,0.6);backdrop-filter: blur(6px);margin-bottom:20px;}
input,select{width:100%;padding:10px;margin-top:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:rgba(0,0,0,0.35);color:#fff;outline:none;}
button.primary{background:#0ff;color:#000;padding:10px 12px;border-radius:10px;border:none;cursor:pointer;font-weight:700;margin-top:10px;}
button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:10px;border-radius:8px;color:#fff;cursor:pointer;margin-left:8px;}
.hidden{display:none;}
.muted{color:#bfe;opacity:.9;}
.plan-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(200px,1fr));gap:12px;margin-top:12px;}
.plan{border-radius:12px;padding:12px;border:1px solid rgba(0,255,240,0.06);background:linear-gradient(180deg,rgba(0,255,240,0.02),transparent);position:relative;transition:transform .18s;cursor:pointer;}
.plan:hover{transform:translateY(-6px);box-shadow:0 12px 36px rgba(0,255,240,0.1);}
.badge{position:absolute;top:10px;right:10px;background:#ffea00;color:#000;padding:4px 6px;border-radius:8px;font-weight:800;font-size:11px;}
table{width:100%;border-collapse:collapse;margin-top:10px;}
th,td{padding:8px;border-bottom:1px solid rgba(255,255,255,0.03);font-size:13px;color:#0ff;}
.notif{position:fixed;right:18px;bottom:18px;background:#0ff;color:#000;padding:12px 16px;border-radius:12px;font-weight:700;box-shadow:0 10px 30px rgba(0,255,240,0.2);z-index:99;}
.auth-wrap{display:flex;flex-direction:column;align-items:center;justify-content:center;padding:30px;}
</style>
</head>
<body>

<div class="bg-gradient"></div>
<canvas id="particles"></canvas>

<div class="app-header">
<h1>VERBOSE</h1>
<i class="fa fa-bars menu-btn" onclick="toggleMenu()"></i>
</div>

<div id="authView" class="card auth-wrap">
<h2>Login / Signup</h2>
<input id="authUser" placeholder="Username"/>
<input id="authPass" placeholder="Password" type="password"/>
<div style="display:flex;margin-top:10px;">
<button class="primary" onclick="doLogin()">Login</button>
<button class="ghost" onclick="doSignup()">Signup</button>
</div>
<div class="muted small" style="margin-top:10px;">Admin: <strong>AdminKhan</strong> / <strong>SuperSecret123</strong></div>
</div>

<div id="appView" class="hidden">
<div class="icon-grid">
<div class="icon-box" onclick="navigate('wallet')"><i class="fa fa-wallet"></i><p>Wallet</p></div>
<div class="icon-box" onclick="navigate('plans')"><i class="fa fa-briefcase"></i><p>Plans</p></div>
<div class="icon-box" onclick="navigate('deposit')"><i class="fa fa-money-bill-wave"></i><p>Deposit</p></div>
<div class="icon-box" onclick="navigate('withdraw')"><i class="fa fa-credit-card"></i><p>Withdraw</p></div>
<div class="icon-box" onclick="navigate('transactions')"><i class="fa fa-file-invoice"></i><p>Transactions</p></div>
<div class="icon-box" onclick="navigate('profile')"><i class="fa fa-user"></i><p>Profile</p></div>
<div class="icon-box" onclick="navigate('about')"><i class="fa fa-info-circle"></i><p>About</p></div>
<div class="icon-box" onclick="doLogout()"><i class="fa fa-right-from-bracket"></i><p>Logout</p></div>
</div>

<div id="plansView" class="card hidden">
<h2>Plans</h2>
<div class="plan-grid" id="planGrid"></div>
</div>

<div id="depositView" class="card hidden">
<h2>Deposit</h2>
<label>Choose Plan</label>
<select id="depositPlan" onchange="updateDepositAmount()"></select>
<label>Amount</label>
<input id="depositAmount" readonly/>
<label>Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="depositNumber" readonly/>
<button class="ghost" onclick="copyText(document.getElementById('depositNumber').value)">Copy</button>
<label>Transaction ID</label>
<input id="depositTxId" placeholder="TX ID"/>
<label>Upload Proof</label>
<input type="file" id="depositProof"/>
<button class="primary" onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="withdrawView" class="card hidden">
<h2>Withdraw</h2>
<label>Amount PKR</label>
<input id="withdrawAmount" type="number"/>
<label>Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<button class="primary" onclick="submitWithdraw()">Request Withdraw</button>
</div>

<div id="txView" class="card hidden">
<h2>Transactions</h2>
<table>
<thead><tr><th>Type</th><th>Amount</th><th>Plan</th><th>Days</th><th>Total Profit</th><th>Time</th><th>Status</th></tr></thead>
<tbody id="txTableBody"></tbody>
</table>
</div>

<div id="profileView" class="card hidden">
<h2>Profile</h2>
<label>Display Name</label>
<input id="profileName"/>
<label>Profile Picture</label>
<input type="file" id="profilePic"/>
<div id="profileStats" class="muted small"></div>
<button class="primary" onclick="updateProfile()">Update Profile</button>
</div>

<div id="aboutView" class="card hidden">
<h2>About VERBOSE</h2>
<p class="muted small">VERBOSE ek premium earning platform hai modern neon style me. Launch: 25 Nov 2025.</p>
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
const canvas=document.getElementById('particles');
const ctx=canvas.getContext('2d');
function resizeCanvas(){canvas.width=innerWidth;canvas.height=innerHeight;}
resizeCanvas();addEventListener('resize',resizeCanvas);
const particles=[];for(let i=0;i<140;i++){particles.push({x:Math.random()*innerWidth,y:Math.random()*innerHeight,r:Math.random()*1.6+0.6,vx:(Math.random()-0.5)*0.6,vy:(Math.random()-0.5)*0.6,h:180+Math.random()*80});}
function drawParticles(){ctx.clearRect(0,0,canvas.width,canvas.height);for(const p of particles){ctx.beginPath();ctx.fillStyle=`hsla(${p.h},100%,60%,0.12)`;ctx.shadowBlur=12;ctx.shadowColor=`hsla(${p.h},100%,60%,0.14)`;ctx.fillRect(p.x,p.y,p.r*2,p.r*2);p.x+=p.vx;p.y+=p.vy;if(p.x<0)p.x=canvas.width;if(p.x>canvas.width)p.x=0;if(p.y<0)p.y=canvas.height;if(p.y>canvas.height)p.y=0;}requestAnimationFrame(drawParticles);}
drawParticles();

// ---------- Users & Plans ----------
const ADMIN={user:'AdminKhan',pass:'SuperSecret123'};
const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
const plans=[
{id:1,name:'CloudNode',days:25,invest:250,totalProfit:1200,offer:true},
{id:2,name:'SkyBoost',days:28,invest:500,totalProfit:2500,offer:true},
{id:3,name:'NeoMax',days:30,invest:750,totalProfit:3750,offer:true},
{id:4,name:'StarGain',days:33,invest:1000,totalProfit:4000,offer:true},
{id:5,name:'CryptoFlow',days:35,invest:1500,totalProfit:7700,offer:true},
{id:6,name:'HyperGain',days:36,invest:2000,totalProfit:8800,offer:true},
{id:7,name:'FutureOne',days:40,invest:10000,totalProfit:15000,offer:true},
{id:8,name:'MegaProfit',days:42,invest:12000,totalProfit:18000,offer:true},
{id:9,name:'UltraPlan',days:45,invest:15000,totalProfit:22000,offer:true},
{id:10,name:'InfinityX',days:50,invest:20000,totalProfit:30000,offer:true},
// Coming soon plans
{id:11,name:'NextGen',days:55,invest:25000,totalProfit:40000,offer:true,comingSoon:true},
{id:12,name:'AlphaPrime',days:60,invest:30000,totalProfit:45000,offer:true,comingSoon:true},
{id:13,name:'BetaBoost',days:65,invest:35000,totalProfit:50000,offer:true,comingSoon:true},
{id:14,name:'GammaFlow',days:70,invest:40000,totalProfit:55000,offer:true,comingSoon:true},
{id:15,name:'DeltaMax',days:75,invest:45000,totalProfit:60000,offer:true,comingSoon:true},
{id:16,name:'OmegaPlus',days:80,invest:50000,totalProfit:65000,offer:true,comingSoon:true},
{id:17,name:'ZetaUltra',days:85,invest:60000,totalProfit:75000,offer:true,comingSoon:true}
];
for(const p of plans)p.dailyProfit=Math.round(p.totalProfit/p.days);

// ---------- Storage ----------
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
function afterLogin(){
const cur=getCurrent();if(!cur)return;
document.getElementById('authView').classList.add('hidden');
document.getElementById('appView').classList.remove('hidden');
renderPlans();
renderDepositPlans();
renderProfile();
renderTransactions();
}

function doSignup(){
const u=document.getElementById('authUser').value.trim();
const p=document.getElementById('authPass').value;
if(!u||!p){showToast('Enter username & password');return;}
const users=getUsers();
if(users.find(x=>x.user===u)){showToast('Username exists');return;}
users.push({user:u,pass:p,balance:0,active:[],profit:0,admin:false});
setUsers(users);
setCurrent({user:u,admin:false});
showToast('Signup successful — logged in');
afterLogin();
}

function doLogin(){
const u=document.getElementById('authUser').value.trim();
const p=document.getElementById('authPass').value;
if(!u||!p){showToast('Enter username & password');return;}
if(u===ADMIN.user&&p===ADMIN.pass){setCurrent({user:ADMIN.user,admin:true});showToast('Admin logged in');afterLogin();return;}
const users=getUsers();
const found=users.find(x=>x.user===u&&x.pass===p);
if(!found){showToast('Invalid credentials');return;}
setCurrent({user:found.user,admin:false});
showToast('Login successful');
afterLogin();
}

function doLogout(){
localStorage.removeItem('verbose_current');
document.getElementById('appView').classList.add('hidden');
document.getElementById('authView').classList.remove('hidden');
showToast('Logged out');
}

// ---------- Navigation ----------
function navigate(view){
const views=['plansView','depositView','withdrawView','txView','aboutView','profileView'];
views.forEach(v=>document.getElementById(v).classList.add('hidden'));
if(view==='plans')document.getElementById('plansView').classList.remove('hidden');
if(view==='deposit')document.getElementById('depositView').classList.remove('hidden');
if(view==='withdraw')document.getElementById('withdrawView').classList.remove('hidden');
if(view==='transactions')document.getElementById('txView').classList.remove('hidden');
if(view==='about')document.getElementById('aboutView').classList.remove('hidden');
if(view==='profile')document.getElementById('profileView').classList.remove('hidden');
}

// ---------- Render Plans ----------
function renderPlans(){
const grid=document.getElementById('planGrid');grid.innerHTML='';
plans.forEach(p=>{
let div=document.createElement('div');div.className='plan';
div.innerHTML=`<h4>${p.name}</h4><p>Invest: ${p.invest} PKR</p><p>Days: ${p.days}</p><p>Total: ${p.totalProfit}</p>`;
if(p.offer)div.innerHTML+=`<div class="badge">${p.comingSoon?'Coming Soon':'24H OFFER'}</div>`;
grid.appendChild(div);
});
}

function renderDepositPlans(){
const select=document.getElementById('depositPlan');select.innerHTML='';
plans.forEach(p=>{if(p.comingSoon)return;const opt=document.createElement('option');opt.value=p.id;opt.innerText=`${p.name} - ${p.invest} PKR`;select.appendChild(opt);});
updateDepositAmount();
updateDepositNumber();
}

// ---------- Deposit ----------
function updateDepositAmount(){
const pid=document.getElementById('depositPlan').value;
const plan=plans.find(p=>p.id==pid);
document.getElementById('depositAmount').value=plan?plan.invest:'';
}

function updateDepositNumber(){
const method=document.getElementById('depositMethod').value;
document.getElementById('depositNumber').value=depositNumbers[method]||'';
}

function copyText(val){
navigator.clipboard.writeText(val);
showToast('Copied!');
}

function submitDeposit(){
const pid=document.getElementById('depositPlan').value;
const txId=document.getElementById('depositTxId').value.trim();
const cur=getCurrent();
if(!cur){showToast('Login first');return;}
if(!txId){showToast('Enter TX ID');return;}
const plan=plans.find(p=>p.id==pid);
const tx=getTx();
tx.push({type:'Deposit',user:cur.user,amount:plan.invest,plan:plan.name,days:plan.days,totalProfit:plan.totalProfit,time:new Date().toLocaleString(),status:'Pending'});
setTx(tx);
showToast('Deposit submitted');
document.getElementById('depositTxId').value='';
renderTransactions();
}

// ---------- Withdraw ----------
function submitWithdraw(){
const amt=parseFloat(document.getElementById('withdrawAmount').value);
const method=document.getElementById('withdrawMethod').value;
const cur=getCurrent();
if(!cur){showToast('Login first');return;}
if(!amt || amt<=0){showToast('Enter valid amount');return;}
const users=getUsers();
const u=users.find(x=>x.user===cur.user);
if(u.balance<amt){showToast('Insufficient balance');return;}
u.balance-=amt;
setUsers(users);
const tx=getTx();
tx.push({type:'Withdraw',user:cur.user,amount:amt,plan:'-',days:'-',totalProfit:'-',time:new Date().toLocaleString(),status:'Pending'});
setTx(tx);
showToast('Withdraw requested');
renderTransactions();
document.getElementById('withdrawAmount').value='';
}

// ---------- Transactions ----------
function renderTransactions(){
const tbody=document.getElementById('txTableBody');tbody.innerHTML='';
const tx=getTx();const cur=getCurrent();if(!cur)return;
tx.filter(t=>t.user===cur.user).forEach(t=>{
const tr=document.createElement('tr');
tr.innerHTML=`<td>${t.type}</td><td>${t.amount}</td><td>${t.plan}</td><td>${t.days}</td><td>${t.totalProfit}</td><td>${t.time}</td><td>${t.status}</td>`;
tbody.appendChild(tr);
});
}

// ---------- Profile ----------
function renderProfile(){
const cur=getCurrent();if(!cur)return;
const users=getUsers();const u=users.find(x=>x.user===cur.user);
document.getElementById('profileName').value=u.user;
document.getElementById('profileStats').innerText=`Balance: ${u.balance} PKR | Total Profit: ${u.profit} PKR`;
}

function updateProfile(){
const name=document.getElementById('profileName').value.trim();
const users=getUsers();const cur=getCurrent();if(!cur)return;
const u=users.find(x=>x.user===cur.user);
if(name)u.user=name;
setUsers(users);
setCurrent({user:u.user,admin:u.admin||false});
showToast('Profile updated');
renderProfile();
}

// ---------- Init ----------
afterLogin();
</script>
</body>
</html>
