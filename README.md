<VERBOSE><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>VERBOSE — Premium Earning Platform</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
<style>
:root{--neon:#00ffff;--bg1:#0f0c29;--bg2:#302b63;--bg3:#24243e;}
*{box-sizing:border-box;}
html,body{height:100%;margin:0;font-family:'Orbitron',sans-serif;background:#000;color:#fff;overflow-x:hidden;}
body{background:linear-gradient(270deg,var(--bg1),var(--bg2),var(--bg3));background-size:600% 600%;animation:gradientBG 18s ease infinite;}
@keyframes gradientBG{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
#bgCanvas{position:fixed;left:0;top:0;width:100%;height:100%;z-index:-1;pointer-events:none;}
#authSection,#mainApp{max-width:400px;margin:100px auto;padding:18px;}
#mainApp{display:none;max-width:1200px;margin:18px auto;padding:18px;}
header{display:flex;align-items:center;justify-content:space-between;gap:12px;}
header h1{margin:0;color:var(--neon);text-shadow:0 0 12px var(--neon);}
.small{font-size:13px;color:#cfe;}
.card{background:rgba(0,255,255,0.04);border:1px solid rgba(0,255,255,0.12);padding:14px;border-radius:12px;box-shadow:0 6px 20px rgba(0,0,0,0.6);backdrop-filter:blur(6px);}
.layout{display:grid;grid-template-columns:320px 1fr;gap:18px;margin-top:18px;}
nav{position:sticky;top:18px;height:fit-content;display:flex;flex-direction:column;gap:8px;}
nav button{display:flex;align-items:center;gap:10px;padding:10px;border-radius:10px;border:none;background:linear-gradient(90deg,rgba(0,255,255,0.06),rgba(0,255,255,0.03));color:var(--neon);cursor:pointer;}
nav button.active{box-shadow:0 0 18px rgba(0,255,255,0.18);transform:scale(1.01);}
main section{display:none;}
main section.active{display:block;}
input,select,textarea{width:100%;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.08);background:rgba(0,0,0,0.35);color:#fff;outline:none;}
button.primary{background:var(--neon);color:#000;font-weight:700;border-radius:10px;padding:10px 12px;border:none;cursor:pointer;}
button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);color:#fff;padding:10px;border-radius:8px;}
.plan-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:12px;margin-top:10px;}
.plan-card{border-radius:12px;padding:12px;background:linear-gradient(180deg, rgba(0,255,255,0.03), rgba(0,255,255,0.02));border:1px solid rgba(0,255,255,0.06);position:relative;transition:transform .18s,box-shadow .18s;}
.plan-card:hover{transform:translateY(-8px);box-shadow:0 12px 40px rgba(0,255,255,0.06);}
.badge{position:absolute;top:10px;right:10px;background:linear-gradient(90deg,#ffea00,#ff9900);color:#000;padding:6px 8px;border-radius:8px;font-weight:800;box-shadow:0 6px 20px rgba(255,150,0,0.15);}
.muted{color:#bfe;}
.row{display:flex;gap:10px;align-items:center;flex-wrap:wrap;}
.notif{position:fixed;right:18px;bottom:18px;background:var(--neon);color:#000;padding:12px 18px;border-radius:12px;font-weight:700;box-shadow:0 10px 30px rgba(0,255,255,0.12);}
@media(max-width:900px){.layout{grid-template-columns:1fr}}
table{width:100%;border-collapse:collapse;}
th,td{padding:8px;border-bottom:1px solid rgba(255,255,255,0.03);text-align:left;font-size:13px;}
.success{color:#0f0;}
.center{display:flex;justify-content:center;align-items:center;gap:8px;}
.smallbtn{padding:6px 10px;border-radius:8px;border:none;cursor:pointer;}
#userBalance{font-weight:800;color:var(--neon);font-size:18px;margin:0 12px;}
</style>
</head>
<body>
<canvas id="bgCanvas"></canvas>
<div id="authSection" class="card">
<h2>Login / Signup</h2>
<input id="inpUser" placeholder="Username" />
<input id="inpPass" type="password" placeholder="Password" />
<div class="row">
<button class="primary" onclick="simpleLogin()">Login</button>
<button class="ghost" onclick="simpleSignup()">Signup</button>
</div>
</div>
<div id="mainApp">
<header>
<div id="welcome"></div>
<div id="userBalance"></div>
<button id="logoutBtn" class="primary" onclick="simpleLogout()">Logout</button>
</header>
<div class="layout">
<aside class="card">
<nav>
<button data-tab="dashboard" class="active">📊 Dashboard</button>
<button data-tab="plans">💼 Plans</button>
<button data-tab="deposit">💰 Deposit</button>
<button data-tab="withdrawal">💸 Withdrawal</button>
<button data-tab="transactions">🧾 Transactions</button>
<button data-tab="adminPanel">🛠 Admin Panel</button>
<button data-tab="about">📑 Company</button>
</nav>
</aside>
<main>
<section id="dashboard" class="card">
<h2>Dashboard</h2>
<div class="row" style="margin-top:8px;flex-wrap:wrap;">
<div class="card" style="padding:12px;"><div>Balance</div><div id="balDisplay">0 PKR</div></div>
<div class="card" style="padding:12px;"><div>Total Profit</div><div id="profitDisplay">0 PKR</div></div>
</div>
<h4>Active Plans</h4>
<div id="activePlans" class="small muted">No active plans</div>
</section>
<section id="plans" class="card">
<h2>Plans</h2>
<div class="plan-grid" id="planGrid"></div>
</section>
<section id="deposit" class="card">
<h2>Deposit</h2>
<label class="small muted">Choose Plan</label>
<select id="planSelect" onchange="onPlanChange()"></select>
<label class="small muted">Amount</label>
<input id="amountInput" readonly />
<label class="small muted">Method</label>
<select id="payMethod" onchange="onMethodChange()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="payNumber" readonly />
<button class="ghost" onclick="copyCurrentNumber()">Copy</button>
<input id="txId" placeholder="Transaction ID (optional)" />
<button class="primary" onclick="submitDeposit()">Submit Deposit</button>
</section>
<section id="withdrawal" class="card">
<h2>Withdrawal</h2>
<select id="withdrawMethod"><option value="jazzcash">JazzCash</option><option value="easypaisa">EasyPaisa</option></select>
<input id="withdrawAccount" placeholder="Account Number / Username" />
<input id="withdrawAmount" placeholder="Amount PKR" type="number" />
<button class="primary" onclick="submitWithdrawal()">Request Withdrawal</button>
</section>
<section id="transactions" class="card">
<h2>Transactions</h2>
<table id="txTable"><thead><tr><th>Type</th><th>Amount</th><th>Plan</th><th>Time</th><th>Status</th></tr></thead><tbody></tbody></table>
</section>
<section id="adminPanel" class="card">
<h2>Admin Panel</h2>
<table id="userTable"><thead><tr><th>Username</th><th>Balance</th><th>Active Plans</th><th>Action</th></tr></thead><tbody></tbody></table>
<table id="adminTxTable"><thead><tr><th>User</th><th>Type</th><th>Amount</th><th>Plan</th><th>Time</th><th>Status</th><th>Action</th></tr></thead><tbody></tbody></table>
</section>
<section id="about" class="card">
<h2>About VERBOSE</h2>
<p class="small muted">Premium earning platform neon style. Owner: John Wilson. Launch: 17 Nov 2025.</p>
</section>
</main>
</div>
</div>
<div id="notifRoot"></div>
<script>
// Neon Background
const canvas=document.getElementById('bgCanvas'),ctx=canvas.getContext('2d');
let W=canvas.width=innerWidth,H=canvas.height=innerHeight;
window.addEventListener('resize',()=>{W=canvas.width=innerWidth;H=canvas.height=innerHeight;});
const particles=[];for(let i=0;i<200;i++){particles.push({x:Math.random()*W,y:Math.random()*H,r:Math.random()*2+0.5,vx:(Math.random()-0.5)*0.8,vy:(Math.random()-0.5)*0.8,hue:180+Math.random()*60});}
function drawBG(){ctx.clearRect(0,0,W,H);particles.forEach(p=>{ctx.beginPath();ctx.fillStyle=`hsla(${p.hue},100%,60%,0.12)`;ctx.shadowBlur=14;ctx.shadowColor=`hsla(${p.hue},100%,60%,0.3)`;ctx.fillRect(p.x,p.y,p.r*2,p.r*2);p.x+=p.vx;p.y+=p.vy;if(p.x<0)p.x=W;if(p.x>W)p.x=0;if(p.y<0)p.y=H;if(p.y>H)p.y=0;});requestAnimationFrame(drawBG);}
drawBG();// Users & Admin const adminCreds={user:'AdminKhan',pass:'SuperSecret123'}; let users=JSON.parse(localStorage.getItem('verbose_users')||'[]'); let currentUser=JSON.parse(localStorage.getItem('verbose_current')||'null'); let transactions=JSON.parse(localStorage.getItem('verbose_tx')||'[]');

// Plans const plans=[ {id:1,name:'Special 1',invest:250,days:24,dailyProfit:50,totalProfit:1200,offer:true}, {id:2,name:'Special 2',invest:500,days:24,dailyProfit:100,totalProfit:2400,offer:true}, {id:3,name:'Special 3',invest:1000,days:24,dailyProfit:200,totalProfit:4800,offer:true}, {id:4,name:'Plan A',invest:2000,days:90,dailyProfit:55,totalProfit:4950,offer:false}, {id:5,name:'Plan B',invest:5000,days:90,dailyProfit:140,totalProfit:12600,offer:false} ];

function saveAll(){localStorage.setItem('verbose_users',JSON.stringify(users));localStorage.setItem('verbose_current',JSON.stringify(currentUser));localStorage.setItem('verbose_tx',JSON.stringify(transactions));} function showNotif(msg){const n=document.createElement('div');n.className='notif';n.innerText=msg;document.getElementById('notifRoot').appendChild(n);setTimeout(()=>n.remove(),2500);}

// Render Plans function renderPlans(){const grid=document.getElementById('planGrid');grid.innerHTML='';plans.forEach(p=>{const div=document.createElement('div');div.className='plan-card';div.innerHTML=<h4>${p.name}</h4><div>Invest: ${p.invest} PKR</div><div>Days: ${p.days}</div><div>Daily Profit: ${p.dailyProfit} PKR</div><div>Total Profit: ${p.totalProfit} PKR</div>${p.offer?'<div class="badge">Offer</div>':''};grid.appendChild(div);});} function renderPlanDropdown(){const sel=document.getElementById('planSelect');sel.innerHTML='';plans.forEach(p=>{const opt=document.createElement('option');opt.value=p.id;opt.innerText=${p.name} — ${p.invest} PKR;sel.appendChild(opt);});onPlanChange();} function onPlanChange(){const planId=parseInt(document.getElementById('planSelect').value);const plan=plans.find(p=>p.id===planId);if(plan) document.getElementById('amountInput').value=plan.invest;onMethodChange();} function onMethodChange(){const method=document.getElementById('payMethod').value;document.getElementById('payNumber').value=(method==='jazzcash')?'03705519562':'03379827882';} function copyCurrentNumber(){navigator.clipboard.writeText(document.getElementById('payNumber').value);showNotif('Copied!');}

// Deposit & Withdraw function submitDeposit(){if(!currentUser){showNotif('Login first');return;}const planId=parseInt(document.getElementById('planSelect').value);const plan=plans.find(p=>p.id===planId);if(!plan){showNotif('Select plan');return;}currentUser.balance+=plan.invest;currentUser.profit+=plan.totalProfit;currentUser.activePlans.push(plan.name);transactions.push({user:currentUser.user,type:'Deposit',amount:plan.invest,plan:plan.name,time:new Date().toLocaleString(),status:'Success'});saveAll();showNotif('Deposit added!');renderApp();} function submitWithdrawal(){if(!currentUser){showNotif('Login first');return;}const amt=parseInt(document.getElementById('withdrawAmount').value);if(isNaN(amt)||amt>currentUser.balance){showNotif('Invalid amount');return;}currentUser.balance-=amt;transactions.push({user:currentUser.user,type:'Withdrawal',amount:amt,plan:'-',time:new Date().toLocaleString(),status:'Success'});saveAll();showNotif('Withdrawal successful!');renderApp();}

// Session & Render function renderApp(){renderPlans();renderPlanDropdown();if(!currentUser){document.getElementById('authSection').style.display='block';document.getElementById('mainApp').style.display='none';}else{document.getElementById('authSection').style.display='none';document.getElementById('mainApp').style.display='block';document.getElementById('welcome').innerText='Welcome, '+currentUser.user;document.getElementById('userBalance').innerText=currentUser.balance+' PKR';}}

// Login / Signup / Logout functions are already defined above renderApp(); </script>

</body>
</html>
