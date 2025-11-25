<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE App Premium</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
/* ---------- Reset & Base ---------- */
*{margin:0;padding:0;box-sizing:border-box;font-family:'Orbitron',sans-serif;}
body, html{height:100%;background:#0a0015;color:#fff;overflow-x:hidden;transition:all 0.3s;}
.hidden{display:none;}
.muted{opacity:0.7;font-size:13px;}

/* ---------- Background & Particles ---------- */
.bg-gradient{position:fixed;inset:0;z-index:-2;background:linear-gradient(270deg,#0f0530,#1b0a4a,#061022);background-size:800% 800%;animation:grad 25s ease infinite;filter:blur(28px) saturate(120%);}
@keyframes grad{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}

/* ---------- Header ---------- */
.app-header{display:flex;justify-content:space-between;align-items:center;padding:15px;background:#111;box-shadow:0 2px 15px rgba(0,255,240,0.3);}
.app-header h1{color:#0ff;font-size:24px;text-shadow:0 0 14px #0ff;}
.menu-btn{font-size:24px;color:#0ff;cursor:pointer;}

/* ---------- Dashboard Icons ---------- */
.icon-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(100px,1fr));gap:18px;padding:20px;}
.icon-box{background:#111;padding:22px;text-align:center;border-radius:20px;box-shadow:0 0 15px #0ff;transition:0.3s;cursor:pointer;}
.icon-box:hover{transform:scale(1.08);box-shadow:0 0 28px #0ff;}
.icon-box i{font-size:30px;margin-bottom:8px;color:#0ff;}
.icon-box p{font-size:14px;color:#cfe;}

/* ---------- Cards ---------- */
.card{background:rgba(255,255,255,0.02);border:1px solid rgba(0,255,255,0.06);padding:20px;border-radius:16px;box-shadow:0 10px 36px rgba(0,0,0,0.6);backdrop-filter:blur(6px);margin:18px;transition:0.3s;}
.card:hover{transform:translateY(-5px);box-shadow:0 15px 40px rgba(0,255,240,0.18);}

/* ---------- Inputs & Buttons ---------- */
input,select{width:100%;padding:12px;margin-top:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.06);background:rgba(0,0,0,0.35);color:#fff;outline:none;transition:0.2s;}
input:focus,select:focus{border-color:#0ff;}
button.primary{background:#0ff;color:#000;padding:12px 15px;border-radius:12px;border:none;cursor:pointer;font-weight:700;margin-top:12px;transition:0.2s;}
button.primary:hover{background:#0ae;}
button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:12px;border-radius:10px;color:#fff;cursor:pointer;margin-left:8px;transition:0.2s;}
button.ghost:hover{background:rgba(0,255,240,0.1);}

/* ---------- Footer Menu ---------- */
.footer-menu{position:fixed;bottom:0;left:0;width:100%;background:#111;display:flex;justify-content:space-around;padding:12px 0;box-shadow:0 -2px 15px rgba(0,255,240,0.3);}
.footer-menu div{text-align:center;color:#fff;font-size:12px;cursor:pointer;}
.footer-menu i{display:block;font-size:24px;margin-bottom:4px;color:#0ff;}

/* ---------- Plans ---------- */
.plan-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:15px;margin-top:15px;}
.plan{border-radius:14px;padding:16px;border:1px solid rgba(0,255,240,0.06);background:linear-gradient(180deg,rgba(0,255,240,0.02),transparent);position:relative;transition:transform .18s;cursor:pointer;}
.plan:hover{transform:translateY(-6px);box-shadow:0 15px 40px rgba(0,255,240,0.12);}
.badge{position:absolute;top:10px;right:10px;background:#ffea00;color:#000;padding:5px 7px;border-radius:10px;font-weight:800;font-size:11px;}

/* ---------- Table ---------- */
table{width:100%;border-collapse:collapse;margin-top:12px;}
th,td{padding:10px;border-bottom:1px solid rgba(255,255,255,0.03);font-size:13px;color:#0ff;}

/* ---------- Toast ---------- */
.notif{position:fixed;right:18px;bottom:18px;background:#0ff;color:#000;padding:14px 18px;border-radius:14px;font-weight:700;box-shadow:0 12px 35px rgba(0,255,240,0.25);z-index:99;}

/* ---------- Auth ---------- */
.auth-wrap{display:flex;flex-direction:column;align-items:center;justify-content:center;padding:35px;border-radius:16px;box-shadow:0 12px 36px rgba(0,255,240,0.25);}
.auth-wrap h2{text-align:center;margin-bottom:18px;}
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
<div style="display:flex;margin-top:12px;">
<button class="primary" onclick="doLogin()">Login</button>
<button class="ghost" onclick="doSignup()">Signup</button>
</div>
<div class="muted small" style="margin-top:12px;">Admin: <strong>AdminKhan</strong> / <strong>SuperSecret123</strong></div>
</div>

<!-- Dashboard -->
<div id="appView" class="hidden">
<div class="icon-grid">
<div class="icon-box" onclick="navigate('wallet')"><i class="fa fa-wallet"></i><p>Wallet</p></div>
<div class="icon-box" onclick="navigate('plans')"><i class="fa fa-briefcase"></i><p>Plans</p></div>
<div class="icon-box" onclick="navigate('deposit')"><i class="fa fa-money-bill-wave"></i><p>Deposit</p></div>
<div class="icon-box" onclick="navigate('withdraw')"><i class="fa fa-credit-card"></i><p>Withdraw</p></div>
<div class="icon-box" onclick="navigate('transactions')"><i class="fa fa-file-invoice"></i><p>Transactions</p></div>
<div class="icon-box" onclick="navigate('profile')"><i class="fa fa-user"></i><p>Profile</p></div>
<div class="icon-box" onclick="navigate('about')"><i class="fa fa-info-circle"></i><p>About</p></div>
<div class="icon-box" onclick="navigate('settings')"><i class="fa fa-cog"></i><p>Settings</p></div>
<div class="icon-box" onclick="navigate('notifications')"><i class="fa fa-bell"></i><p>Notifications</p></div>
<div class="icon-box" onclick="navigate('help')"><i class="fa fa-question-circle"></i><p>Help</p></div>
<div class="icon-box" onclick="doLogout()"><i class="fa fa-right-from-bracket"></i><p>Logout</p></div>
</div>

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
<p class="muted small">VERBOSE ek modern premium earning platform hai, app style dashboard aur neon theme ke sath. Launch: 25 Nov 2025.</p>
</div>

<!-- Footer -->
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

// ---------- Admin, Plans, Storage ----------
const ADMIN={user:'AdminKhan',pass:'SuperSecret123'};
const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
const plans=[]; for(let i=1;i<=25;i++){plans.push({id:i,name:`Plan ${i}`,days:20+i,invest:100*i,totalProfit:100*i*(1+i*0.5),offer:true,dailyProfit:Math.round((100*i*(1+i*0.5))/(20+i))});}

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
function afterLogin(){const cur=getCurrent();if(!cur)return;document.getElementById('authView').classList.add('hidden');document.getElementById('appView').classList.remove('hidden');renderPlans();renderDepositPlans();renderTransactions();renderProfile();}
function doSignup(){const u=document.getElementById('authUser').value.trim();const p=document.getElementById('authPass').value;if(!u||!p){showToast('Enter username & password');return;}const users=getUsers();if(users.find(x=>x.user===u)){showToast('Username exists');return;}users.push({user:u,pass:p,balance:0,active:[],profit:0,admin:false});setUsers(users);setCurrent({user:u,admin:false});showToast('Signup successful');afterLogin();}
function doLogin(){const u=document.getElementById('authUser').value.trim();const p=document.getElementById('authPass').value;if(!u||!p){showToast('Enter username & password');return;}if(u===ADMIN.user&&p===ADMIN.pass){setCurrent({user:ADMIN.user,admin:true});showToast('Admin logged in');afterLogin();return;}const users=getUsers();const found=users.find(x=>x.user===u&&x.pass===p);if(!found){showToast('Invalid credentials');return;}setCurrent({user:found.user,admin:false});showToast('Login successful');afterLogin();}
function doLogout(){localStorage.removeItem('verbose_current');document.getElementById('appView').classList.add('hidden');document.getElementById('authView').classList.remove('hidden');showToast('Logged out');}

// ---------- Navigation ----------
function navigate(view){const views=['plansView','depositView','withdrawView','txView','aboutView','profileView','settingsView','notificationsView','helpView'];views.forEach(v=>{let el=document.getElementById(v); if(el) el.classList.add('hidden');});if(view==='plans') document.getElementById('plansView').classList.remove('hidden');if(view==='deposit') document.getElementById('depositView').classList.remove('hidden');if(view==='withdraw') document.getElementById('withdrawView').classList.remove('hidden');if(view==='transactions'){ document.getElementById('txView').classList.remove('hidden'); renderTransactions();}if(view==='about') document.getElementById('aboutView').classList.remove('hidden');if(view==='profile'){ document.getElementById('profileView').classList.remove('hidden'); renderProfile();}

// ---------- Plans & Deposit ----------
function renderPlans(){const grid=document.getElementById('planGrid');grid.innerHTML='';plans.forEach(p=>{let div=document.createElement('div');div.className='plan';div.innerHTML=`<h4>${p.name}</h4><p>Invest: ${p.invest} PKR</p><p>Days: ${p.days}</p><p>Total: ${p.totalProfit}</p><div class="badge">24H OFFER</div>`;grid.appendChild(div);});}
function renderDepositPlans(){const select=document.getElementById('depositPlan');select.innerHTML='';plans.forEach(p=>{const opt=document.createElement('option');opt.value=p.id;opt.innerText=`${p.name} - ${p.invest} PKR`;select.appendChild(opt);});updateDepositAmount();}
function updateDepositAmount(){const planId=parseInt(document.getElementById('depositPlan').value);const plan=plans.find(p=>p.id===planId);document.getElementById('depositAmount').value = plan ? plan.invest : '';updateDepositNumber();}
function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value = depositNumbers[method];}

// ---------- Copy ----------
function copyText(text){navigator.clipboard.writeText(text);showToast('Copied to clipboard');}

// ---------- Deposit ----------
function submitDeposit(){
  const cur = getCurrent(); if(!cur){showToast('Login first'); return;}
  const planId = parseInt(document.getElementById('depositPlan').value);
  const amount = parseFloat(document.getElementById('depositAmount').value);
  const method = document.getElementById('depositMethod').value;
  const txId = document.getElementById('depositTxId').value.trim();
  if(!txId){showToast('Enter Transaction ID'); return;}
  const tx = getTx(); 
  tx.push({type:'Deposit',user:cur.user,plan:planId,amount:amount,method:method,txId:txId,status:'Pending',time:new Date().toLocaleString()});
  setTx(tx); showToast('Deposit submitted'); renderTransactions();
}

// ---------- Withdraw ----------
function submitWithdraw(){
  const cur = getCurrent(); if(!cur){showToast('Login first'); return;}
  const amount = parseFloat(document.getElementById('withdrawAmount').value);
  const method = document.getElementById('withdrawMethod').value;
  if(isNaN(amount) || amount<=0){showToast('Enter valid amount'); return;}
  const tx = getTx();
  tx.push({type:'Withdraw',user:cur.user,amount:amount,method:method,status:'Pending',time:new Date().toLocaleString()});
  setTx(tx); showToast('Withdraw requested'); renderTransactions();
}

// ---------- Transactions ----------
function renderTransactions(){
  const cur = getCurrent(); if(!cur) return;
  const txTable = document.getElementById('txTableBody'); txTable.innerHTML='';
  const tx = getTx().filter(t=>t.user===cur.user);
  tx.forEach(t=>{
    let row = document.createElement('tr');
    row.innerHTML = `<td>${t.type}</td><td>${t.amount || '-'}</td><td>${t.plan || '-'}</td><td>${t.days || '-'}</td><td>${t.totalProfit || '-'}</td><td>${t.time}</td><td>${t.status}</td>`;
    txTable.appendChild(row);
  });
}

// ---------- Profile ----------
function renderProfile(){
  const cur = getCurrent(); if(!cur) return;
  document.getElementById('profileName').value = cur.user;
  const users = getUsers(); 
  const user = users.find(u=>u.user===cur.user);
  document.getElementById('profileStats').innerText = `Balance: ${user.balance || 0} PKR | Profit: ${user.profit || 0} PKR`;
}
function updateProfile(){
  const cur = getCurrent(); if(!cur) return;
  const users = getUsers();
  const user = users.find(u=>u.user===cur.user);
  user.user = document.getElementById('profileName').value.trim() || user.user;
  setUsers(users);
  setCurrent({user:user.user,admin:user.admin});
  showToast('Profile updated'); renderProfile();
}

// ---------- Init ----------
afterLogin();
</script>
</body>
</html>
