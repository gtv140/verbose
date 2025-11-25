<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
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
.card{background:rgba(255,255,255,0.03);border:1px solid rgba(0,255,255,0.06);padding:14px;border-radius:12px;box-shadow:0 8px 30px rgba(0,0,0,0.6);backdrop-filter: blur(6px);}
input,select{width:100%;padding:10px;margin-top:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:rgba(0,0,0,0.35);color:#fff;outline:none;}
button.primary{background:#0ff;color:#000;padding:10px 12px;border-radius:10px;border:none;cursor:pointer;font-weight:700;}
button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:10px;border-radius:8px;color:#fff;cursor:pointer;margin-left:8px;}
.hidden{display:none;}
.muted{color:#bfe;opacity:.9;}
.plan-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(200px,1fr));gap:12px;margin-top:12px;}
.plan{border-radius:12px;padding:12px;border:1px solid rgba(0,255,240,0.06);background:linear-gradient(180deg,rgba(0,255,240,0.02),transparent);position:relative;transition:transform .18s;cursor:pointer;}
.plan:hover{transform:translateY(-6px);box-shadow:0 12px 36px rgba(0,255,240,0.1);}
.badge{position:absolute;top:10px;right:10px;background:#ffea00;color:#000;padding:4px 6px;border-radius:8px;font-weight:800;font-size:11px;}
.tx{margin:4px;padding:6px;border-radius:8px;background:#111;color:#fff;}
.auth-wrap{display:flex;flex-direction:column;align-items:center;justify-content:center;padding:30px;}
#dashboard{display:none;}
.profile-img{width:60px;height:60px;border-radius:50%;object-fit:cover;margin-bottom:10px;}
</style>
</head>
<body>

<div class="bg-gradient"></div>
<canvas id="particles"></canvas>

<!-- AUTH -->
<div id="authView" class="card auth-wrap">
<h2>Login / Signup</h2>
<input id="authUser" placeholder="Username"/>
<input id="authPass" placeholder="Password" type="password"/>
<div style="display:flex;margin-top:10px;">
<button class="primary" onclick="doLogin()">Login</button>
<button class="ghost" onclick="doSignup()">Signup</button>
</div>
</div>

<!-- DASHBOARD -->
<div id="dashboard">
<div class="app-header">
<h1>VERBOSE</h1>
<i class="fa fa-right-from-bracket menu-btn" onclick="logout()"></i>
</div>

<p style="padding:0 20px;">Welcome, <strong id="welcomeUser"></strong></p>

<div class="icon-grid">
<div class="icon-box" onclick="showSection('wallet')"><i class="fa fa-wallet"></i><p>Wallet</p></div>
<div class="icon-box" onclick="showSection('plans')"><i class="fa fa-briefcase"></i><p>Plans</p></div>
<div class="icon-box" onclick="showSection('deposit')"><i class="fa fa-money-bill-wave"></i><p>Deposit</p></div>
<div class="icon-box" onclick="showSection('withdraw')"><i class="fa fa-credit-card"></i><p>Withdraw</p></div>
<div class="icon-box" onclick="showSection('transactions')"><i class="fa fa-file-invoice"></i><p>Transactions</p></div>
<div class="icon-box" onclick="showSection('profile')"><i class="fa fa-user"></i><p>Profile</p></div>
</div>

<!-- WALLET -->
<div id="wallet" class="card hidden">
<h3>Wallet</h3>
<p>Balance: <span id="walletBalance"></span> PKR</p>
<p>Daily Profit: <span id="dailyProfit"></span> PKR</p>
</div>

<!-- PLANS -->
<div id="plans" class="card hidden">
<h3>Active Plans</h3>
<div id="plansList" class="plan-grid"></div>
<h3>Coming Soon Plans</h3>
<div id="comingPlans" class="plan-grid"></div>
</div>

<!-- DEPOSIT -->
<div id="deposit" class="card hidden">
<h3>Deposit</h3>
<label>Choose Plan</label>
<select id="depositPlan" onchange="updateDepositAmount()"></select>
<label>Amount</label>
<input id="depositAmount" readonly/>
<label>Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<label>Number</label>
<input id="depositNumber" readonly/>
<button class="ghost" onclick="copyDepositNumber()">Copy Number</button>
<label>Transaction ID</label>
<input id="txID" placeholder="TX ID"/>
<label>Upload Proof</label>
<input id="depositProof" type="file"/>
<button class="primary" onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdraw" class="card hidden">
<h3>Withdraw</h3>
<label>Username</label>
<input id="withdrawUser" readonly/>
<label>Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<label>Account Number</label>
<input id="withdrawNumber"/>
<label>Amount PKR</label>
<input id="withdrawAmount" type="number"/>
<button class="primary" onclick="submitWithdraw()">Submit Withdraw</button>
</div>

<!-- TRANSACTIONS -->
<div id="transactions" class="card hidden">
<h3>Transactions</h3>
<div id="transactionsList"></div>
</div>

<!-- PROFILE -->
<div id="profile" class="card hidden">
<h3>Profile</h3>
<img id="profileImg" src="" class="profile-img"/>
<input type="file" id="profileDP" onchange="uploadDP()"/>
<p>Username: <input id="profileUser" readonly/></p>
</div>

<script>
// ---------- NEON PARTICLES ----------
const canvas=document.getElementById('particles');
const ctx=canvas.getContext('2d');
function resizeCanvas(){canvas.width=innerWidth;canvas.height=innerHeight;}
resizeCanvas();window.addEventListener('resize',resizeCanvas);
const particles=[];
for(let i=0;i<120;i++){particles.push({x:Math.random()*innerWidth,y:Math.random()*innerHeight,r:Math.random()*1.6+0.6,vx:(Math.random()-0.5)*0.6,vy:(Math.random()-0.5)*0.6,h:180+Math.random()*80});}
function drawParticles(){ctx.clearRect(0,0,canvas.width,canvas.height);for(const p of particles){ctx.beginPath();ctx.fillStyle=`hsla(${p.h},100%,60%,0.12)`;ctx.shadowBlur=12;ctx.shadowColor=`hsla(${p.h},100%,60%,0.14)`;ctx.fillRect(p.x,p.y,p.r*2,p.r*2);p.x+=p.vx;p.y+=p.vy;if(p.x<0)p.x=canvas.width;if(p.x>canvas.width)p.x=0;if(p.y<0)p.y=canvas.height;if(p.y>canvas.height)p.y=0;}requestAnimationFrame(drawParticles);}
drawParticles();

// ---------- STORAGE ----------
let currentUser = JSON.parse(localStorage.getItem('verbose_current')||'null');
let users = JSON.parse(localStorage.getItem('verbose_users')||'[]');
let transactions = JSON.parse(localStorage.getItem('verbose_tx')||'[]');
const depositNumbers={jazzcash:'03705519562',easypaisa:'03379827882'};
const plans=[
{id:1,name:'Cloud Node',days:25,invest:250,totalProfit:1200},
{id:2,name:'Sky Vault',days:28,invest:500,totalProfit:2500},
{id:3,name:'Aqua Mine',days:30,invest:750,totalProfit:3750},
{id:4,name:'Solar Peak',days:33,invest:1000,totalProfit:4000},
{id:5,name:'Lunar Base',days:35,invest:1500,totalProfit:7700},
{id:6,name:'Quantum Hub',days:38,invest:2000,totalProfit:12000,coming:true},
{id:7,name:'Nebula Core',days:40,invest:2500,totalProfit:15000,coming:true},
{id:8,name:'Stellar Forge',days:43,invest:3000,totalProfit:18000,coming:true},
{id:9,name:'Galactic Tower',days:45,invest:3500,totalProfit:21000,coming:true},
{id:10,name:'Cosmo Vault',days:48,invest:4000,totalProfit:25000,coming:true}];

// ---------- UTIL ----------
function saveUsers(){localStorage.setItem('verbose_users',JSON.stringify(users));}
function saveTx(){localStorage.setItem('verbose_tx',JSON.stringify(transactions));}
function saveCurrent(){localStorage.setItem('verbose_current',JSON.stringify(currentUser));}
function showToast(msg){alert(msg);}
function afterLogin(){
document.getElementById('authView').style.display='none';
document.getElementById('dashboard').style.display='block';
document.getElementById('welcomeUser').innerText=currentUser.user;
document.getElementById('profileUser').value=currentUser.user;
document.getElementById('withdrawUser').value=currentUser.user;
renderWallet(); renderPlans(); renderComingPlans(); populateDepositPlans(); updateDepositNumber();
}
if(currentUser){afterLogin();}

// ---------- AUTH ----------
function doSignup(){
  const u=document.getElementById('authUser').value.trim();
  const p=document.getElementById('authPass').value.trim();
  if(!u||!p){showToast('Enter username & password'); return;}
  if(users.find(x=>x.user===u)){showToast('Username exists'); return;}
  const user={user:u,pass:p,balance:0,active:[],profit:0,dp:''};
  users.push(user); saveUsers(); currentUser=user; saveCurrent(); afterLogin();
}
function doLogin(){
  const u=document.getElementById('authUser').value.trim();
  const p=document.getElementById('authPass').value.trim();
  const user = users.find(x=>x.user===u && x.pass===p);
  if(!user){showToast('Invalid credentials'); return;}
  currentUser=user; saveCurrent(); afterLogin();
}
function logout(){currentUser=null; localStorage.removeItem('verbose_current'); document.getElementById('dashboard').style.display='none'; document.getElementById('authView').style.display='block';}

// ---------- NAV ----------
function showSection(sec){['wallet','plans','deposit','withdraw','transactions','profile'].forEach(s=>document.getElementById(s).style.display='none');document.getElementById(sec).style.display='block';}

// ---------- WALLET ----------
function renderWallet(){document.getElementById('walletBalance').innerText=currentUser.balance; document.getElementById('dailyProfit').innerText=currentUser.profit;}

// ---------- PLANS ----------
function renderPlans(){const container=document.getElementById('plansList');container.innerHTML='';plans.filter(p=>!p.coming).forEach(p=>{const div=document.createElement('div');div.className='plan';div.innerHTML=`<h4>${p.name}</h4><p>Invest: ${p.invest} PKR</p><p>Days: ${p.days}</p><p>Total: ${p.totalProfit}</p>`;container.appendChild(div);});}
function renderComingPlans(){const container=document.getElementById('comingPlans');container.innerHTML='';plans.filter(p=>p.coming).forEach(p=>{const div=document.createElement('div');div.className='plan';div.innerHTML=`<h4>${p.name} (Coming Soon)</h4><p>Invest: ${p.invest} PKR</p><p>Days: ${p.days}</p><p>Total: ${p.totalProfit}</p>`;container.appendChild(div);});}

// ---------- DEPOSIT ----------
function populateDepositPlans(){const sel=document.getElementById('depositPlan'); sel.innerHTML=''; plans.forEach(p=>{if(!p.coming) sel.innerHTML+=`<option value="${p.id}">${p.name}</option>`;}); updateDepositAmount();}
function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=depositNumbers[method];}
function updateDepositAmount(){const id=parseInt(document.getElementById('depositPlan').value);const plan=plans.find(p=>p.id===id); if(plan) document.getElementById('depositAmount').value=plan.invest;}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value).then(()=>showToast('Number copied'));}
function submitDeposit(){const planId=parseInt(document.getElementById('depositPlan').value);const plan=plans.find(p=>p.id===planId); const proof=document.getElementById('depositProof').files[0]; const txID=document.getElementById('txID').value.trim(); if(!txID){showToast('Enter TX ID');return;} const tx={user:currentUser.user,type:'Deposit',amount:plan.invest,plan:plan.name,status:'Pending',time:new Date().toLocaleString(),proof:proof?proof.name:'',txID:txID}; transactions.push(tx); saveTx(); showToast('Deposit submitted'); renderTransactions();}

// ---------- WITHDRAW ----------
function submitWithdraw(){const method=document.getElementById('withdrawMethod').value; const number=document.getElementById('withdrawNumber').value.trim(); const amount=parseInt(document.getElementById('withdrawAmount').value); if(!number||!amount){showToast('Enter account & amount');return;} if(amount>currentUser.balance){showToast('Insufficient balance');return;} const tx={user:currentUser.user,type:'Withdraw',amount:amount,method:method,status:'Pending',time:new Date().toLocaleString()}; transactions.push(tx); saveTx(); currentUser.balance-=amount; saveUsers(); renderWallet(); renderTransactions(); showToast('Withdraw request submitted');}

// ---------- TRANSACTIONS ----------
function renderTransactions(){const container=document.getElementById('transactionsList');container.innerHTML=''; transactions.filter(t=>t.user===currentUser.user).forEach(t=>{const div=document.createElement('div');div.className='tx'; div.style.background='#111'; div.style.color='#fff'; div.innerHTML=`<strong>${t.type}</strong> — ${t.amount} PKR — ${t.plan||t.method||''} — Status: ${t.status} — ${t.time}`; container.appendChild(div);});}

// ---------- PROFILE ----------
function uploadDP(){const file=document.getElementById('profileDP').files[0]; if(!file) return; const reader=new FileReader(); reader.onload=function(e){document.getElementById('profileImg').src=e.target.result; currentUser.dp=e.target.result; saveUsers(); saveCurrent();}; reader.readAsDataURL(file);}
</script>

</body>
</html>
