<VERBOSE>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>VERBOSE — Premium Earning Platform</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
<style>
:root{--neon:#00ffff;--bg1:#0f0c29;--bg2:#302b63;--bg3:#24243e}
*{box-sizing:border-box}
html,body{height:100%;margin:0;font-family:'Orbitron',sans-serif;background:#000;color:#fff;overflow-x:hidden}
body{background:linear-gradient(270deg,var(--bg1),var(--bg2),var(--bg3));background-size:600% 600%;animation:gradientBG 18s ease infinite}
@keyframes gradientBG{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
#bgCanvas{position:fixed;left:0;top:0;width:100%;height:100%;z-index:-1;pointer-events:none}
#app{max-width:1200px;margin:18px auto;padding:18px}
header{display:flex;align-items:center;justify-content:space-between;gap:12px}
header h1{margin:0;color:var(--neon);text-shadow:0 0 8px var(--neon)}
.small{font-size:13px;color:#cfe}
.card{background:rgba(0,255,255,0.04);border:1px solid rgba(0,255,255,0.12);padding:14px;border-radius:12px;box-shadow:0 6px 20px rgba(0,0,0,0.6);backdrop-filter:blur(6px)}
.layout{display:grid;grid-template-columns:320px 1fr;gap:18px;margin-top:18px}
nav{position:sticky;top:18px;height:fit-content;display:flex;flex-direction:column;gap:8px}
nav button{display:flex;align-items:center;gap:10px;padding:10px;border-radius:10px;border:none;background:linear-gradient(90deg,rgba(0,255,255,0.06),rgba(0,255,255,0.03));color:var(--neon);cursor:pointer}
nav button.active{box-shadow:0 0 18px rgba(0,255,255,0.18);transform:scale(1.01)}
main section{display:none}
main section.active{display:block}
input,select,textarea{width:100%;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.08);background:rgba(0,0,0,0.35);color:#fff;outline:none}
button.primary{background:var(--neon);color:#000;font-weight:700;border-radius:10px;padding:10px 12px;border:none;cursor:pointer}
button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);color:#fff;padding:10px;border-radius:8px}
.plan-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:12px;margin-top:10px}
.plan-card{border-radius:12px;padding:12px;background:linear-gradient(180deg, rgba(0,255,255,0.03), rgba(0,255,255,0.02));border:1px solid rgba(0,255,255,0.06);position:relative;transition:transform .18s,box-shadow .18s}
.plan-card:hover{transform:translateY(-8px);box-shadow:0 12px 40px rgba(0,255,255,0.06)}
.badge{position:absolute;top:10px;right:10px;background:linear-gradient(90deg,#ffea00,#ff9900);color:#000;padding:6px 8px;border-radius:8px;font-weight:800;box-shadow:0 6px 20px rgba(255,150,0,0.15)}
.muted{color:#bfe}
.row{display:flex;gap:10px;align-items:center;flex-wrap:wrap}
.notif{position:fixed;right:18px;bottom:18px;background:var(--neon);color:#000;padding:12px 18px;border-radius:12px;font-weight:700;box-shadow:0 10px 30px rgba(0,255,255,0.12)}
@media(max-width:900px){.layout{grid-template-columns:1fr}}
table{width:100%;border-collapse:collapse}
th,td{padding:8px;border-bottom:1px solid rgba(255,255,255,0.03);text-align:left;font-size:13px}
.success{color:#0f0}
.center{display:flex;justify-content:center;align-items:center;gap:8px}
.smallbtn{padding:6px 10px;border-radius:8px;border:none;cursor:pointer}
</style>
</head>
<body>
<canvas id="bgCanvas"></canvas>
<div id="app">
<header>
<div>
<h1>VERBOSE</h1>
<div class="small">Owner: John Wilson • Launch Date: 17 Nov 2025</div>
</div>
<div id="topActions" class="row">
<div id="welcome" class="small muted">Not logged in</div>
<button id="openAuth" class="primary">Login / Signup</button>
</div>
</header>

<div class="layout">
<aside class="card">
<nav>
<button data-tab="dashboard" class="active">📊 Dashboard</button>
<button data-tab="plans">💼 Plans</button>
<button data-tab="deposit">💰 Deposit</button>
<button data-tab="withdrawal">💸 Withdrawal</button>
<button data-tab="transactions">🧾 Transactions</button>
<button data-tab="about">📑 Company</button>
<button id="logoutBtn" style="margin-top:10px;background:linear-gradient(90deg,#ff4d4d,#ff1a1a);color:#fff;display:none">Logout</button>
</nav>

<hr style="border:none;border-top:1px solid rgba(255,255,255,0.04);margin:12px 0"/>
<div class="small muted">Payment Numbers</div>
<div class="row" style="margin-top:8px">
<div class="card" style="padding:10px;width:100%">
<div style="display:flex;justify-content:space-between;align-items:center">
<div><strong>JazzCash</strong><div class="small muted">03705519562</div></div>
<button class="ghost" onclick="copyText('03705519562')">Copy</button>
</div>
<hr style="border:none;border-top:1px solid rgba(255,255,255,0.03);margin:8px 0"/>
<div style="display:flex;justify-content:space-between;align-items:center">
<div><strong>EasyPaisa</strong><div class="small muted">03379827882</div></div>
<button class="ghost" onclick="copyText('03379827882')">Copy</button>
</div>
</div>
</div>
</aside>

<main>
<section id="authSection" class="card active">
<h2 style="margin-top:0">Login / Signup</h2>
<div class="field"><input id="inpUser" placeholder="Username" /></div>
<div class="field"><input id="inpPass" type="password" placeholder="Password" /></div>
<div class="row">
<button class="primary" onclick="simpleLogin()">Login</button>
<button class="ghost" onclick="simpleSignup()">Signup</button>
</div>
<div class="small muted" style="margin-top:10px">Note: Signup saved locally. Refresh keeps you logged in.</div>
</section>

<section id="dashboard" class="card">
<div style="display:flex;justify-content:space-between;align-items:center;gap:12px">
<div>
<h2 style="margin:0">Dashboard</h2>
<div class="small muted" id="dashSub">Overview</div>
</div>
<div class="row">
<div class="card" style="padding:8px;">
<div class="small muted">Balance</div>
<div style="font-weight:800;color:var(--neon);font-size:18px" id="balDisplay">0 PKR</div>
</div>
<button onclick="showTab('deposit')" class="primary">Deposit Now</button>
</div>
</div>
<hr style="border:none;border-top:1px solid rgba(255,255,255,0.04);margin:12px 0"/>
<div style="display:flex;gap:12px;flex-wrap:wrap">
<div class="card" style="flex:1;min-width:220px">
<h4 style="margin:0">Active Plans</h4>
<div id="activePlans" class="small muted" style="margin-top:8px">No active plans</div>
</div>
<div class="card" style="flex:1;min-width:220px">
<h4 style="margin:0">Achievements</h4>
<div id="achv" class="small muted" style="margin-top:8px">No badges yet</div>
</div>
</div>
</section>

<section id="plans" class="card">
<h2 style="margin-top:0">Plans</h2>
<div class="plan-grid" id="planGrid"></div>
</section>

<section id="deposit" class="card">
<h2 style="margin-top:0">Deposit</h2>
<div class="small muted">Select plan, send payment and upload proof.</div>
<div style="margin-top:12px">
<label class="small muted">Choose Plan</label>
<select id="planSelect" onchange="onPlanChange()"></select>
</div>
<div style="display:flex;gap:10px;margin-top:8px;flex-wrap:wrap">
<div style="flex:1">
<label class="small muted">Amount (auto)</label>
<input id="amountInput" readonly />
</div>
<div style="width:160px">
<label class="small muted">Method</label>
<select id="payMethod" onchange="onMethodChange()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
</div>
</div>
<div style="display:flex;gap:10px;margin-top:8px;align-items:center">
<input id="payNumber" readonly style="flex:1" />
<button class="ghost" onclick="copyCurrentNumber()">Copy</button>
</div>
<div style="margin-top:8px">
<input id="txId" placeholder="Transaction ID (optional)" />
</div>
<div style="margin-top:8px">
<input id="proof" type="file" />
</div>
<div style="display:flex;gap:10px;margin-top:10px">
<button class="primary" onclick="submitDeposit()">Submit Deposit</button>
<button class="ghost" onclick="backToDashboard()">Back</button>
</div>
</section>

<section id="withdrawal" class="card">
<h2 style="margin-top:0">Withdrawal</h2>
<div class="small muted">Request withdrawal (simulated).</div>
<div style="margin-top:12px">
<label class="small muted">Method</label>
<select id="withdrawMethod"><option value="jazzcash">JazzCash</option><option value="easypaisa">EasyPaisa</option></select>
</div>
<div style="margin-top:8px"><input id="withdrawAccount" placeholder="Account Number / Username" /></div>
<div style="margin-top:8px"><input id="withdrawAmount" placeholder="Amount PKR" type="number" /></div>
<div style="display:flex;gap:10px;margin-top:10px">
<button class="primary" onclick="submitWithdrawal()">Request Withdrawal</button>
<button class="ghost" onclick="backToDashboard()">Back</button>
</div>
</section>

<section id="transactions" class="card">
<h2 style="margin-top:0">Transactions</h2>
<div style="overflow:auto;max-height:320px">
<table id="txTable">
<thead><tr><th>Type</th><th>Amount</th><th>Plan</th><th>Time</th><th>Status</th></tr></thead>
<tbody></tbody>
</table>
</div>
</section>

<section id="about" class="card">
<h2 style="margin-top:0">About VERBOSE</h2>
<p class="small muted">
VERBOSE ek premium earning platform hai jo modern digital finance ko neon style me present karta hai.
Owner: <strong>John Wilson</strong>. Launch Date: <strong>17 Nov 2025</strong>.
Mission: Financial freedom, transparent plans, aur easy UX.
</p>
<div style="display:flex;gap:10px;margin-top:8px">
<button class="primary" onclick="backToDashboard()">Back</button>
</div>
</section>
</main>
</div>
</div>

<div id="notifRoot"></div>

<script>
/* ------------------------------ Neon background ------------------------------ */
const canvas=document.getElementById('bgCanvas'),ctx=canvas.getContext('2d');
let W=canvas.width=innerWidth,H=canvas.height=innerHeight;
window.addEventListener('resize',()=>{W=canvas.width=innerWidth;H=canvas.height=innerHeight;});
const particles=[];
for(let i=0;i<140;i++){particles.push({x:Math.random()*W,y:Math.random()*H,r:Math.random()*1.6+0.6,vx:(Math.random()-0.5)*0.6,vy:(Math.random()-0.5)*0.6,hue:180+Math.random()*60});}
function drawBG(){ctx.clearRect(0,0,W,H);particles.forEach(p=>{ctx.beginPath();ctx.fillStyle=`hsla(${p.hue},100%,60%,0.12)`;ctx.shadowBlur=12;ctx.shadowColor='rgba(0,255,255,0.15)';ctx.fillRect(p.x,p.y,p.r*2,p.r*2);p.x+=p.vx;p.y+=p.vy;if(p.x<0)p.x=W;if(p.x>W)p.x=0;if(p.y<0)p.y=H;if(p.y>H)p.y=0;});requestAnimationFrame(drawBG);}
drawBG();

/* ------------------------------ App Data ------------------------------ */
let realUser=JSON.parse(localStorage.getItem('verbose_users_single')||'null');
let currentUser=JSON.parse(localStorage.getItem('verbose_current')||'null');
let transactions=JSON.parse(localStorage.getItem('verbose_tx')||'[]');

const navButtons=document.querySelectorAll('nav button[data-tab]');
navButtons.forEach(btn=>btn.addEventListener('click',()=>showTab(btn.dataset.tab)));
document.getElementById('openAuth').addEventListener('click',()=>showTab('authSection'));
const logoutBtn=document.getElementById('logoutBtn');
logoutBtn.addEventListener('click',simpleLogout);

/* ------------------------------ Plans ------------------------------ */
const plans=[{id:1,name:'Plan 1',invest:250,days:25,totalProfit:1200,offer:true},{id:2,name:'Plan 2',invest:400,days:28,totalProfit:1600,offer:true},{id:3,name:'Plan 3',invest:600,days:30,totalProfit:2400,offer:true},{id:4,name:'Plan 4',invest:800,days:32,totalProfit:3200,offer:true},{id:5,name:'Plan 5',invest:1000,days:35,totalProfit:4000,offer:true},{id:6,name:'Plan 6',invest:1500,days:40,totalProfit:3750},{id:7,name:'Plan 7',invest:2000,days:45,totalProfit:5000},{id:8,name:'Plan 8',invest:3000,days:50,totalProfit:7500},{id:9,name:'Plan 9',invest:5000,days:55,totalProfit:12500},{id:10,name:'Plan 10',invest:7000,days:60,totalProfit:17500},{id:11,name:'Plan 11',invest:10000,days:70,totalProfit:25000},{id:12,name:'Plan 12',invest:15000,days:80,totalProfit:37500},{id:13,name:'Plan 13',invest:20000,days:90,totalProfit:50000},{id:14,name:'Plan 14',invest:25000,days:100,totalProfit:62500},{id:15,name:'Plan 15',invest:30000,days:110,totalProfit:75000},{id:16,name:'Plan 16',invest:35000,days:120,totalProfit:87500},{id:17,name:'Plan 17',invest:38000,days:125,totalProfit:95000},{id:18,name:'Plan 18',invest:40000,days:130,totalProfit:100000},{id:19,name:'Plan 19',invest:42000,days:135,totalProfit:105000},{id:20,name:'Plan 20',invest:45000,days:140,totalProfit:112500},{id:21,name:'Plan 21',invest:48000,days:145,totalProfit:120000},{id:22,name:'Plan 22',invest:50000,days:150,totalProfit:125000},{id:23,name:'Plan 23',invest:52000,days:150,totalProfit:130000},{id:24,name:'Plan 24',invest:54000,days:150,totalProfit:135000},{id:25,name:'Plan 25',invest:55000,days:150,totalProfit:137500}];

/* ------------------------------ UI & Storage ------------------------------ */
function saveUser(){localStorage.setItem('verbose_users_single',JSON.stringify(realUser));localStorage.setItem('verbose_current',JSON.stringify(currentUser));}
// continuation from your code
function updateUI(){
    if(currentUser){
        document.getElementById('welcome').innerText=`Hi, ${currentUser.user}`;
        document.getElementById('logoutBtn').style.display='block';
        document.getElementById('authSection').classList.remove('active');
        showTab('dashboard');
        document.getElementById('balDisplay').innerText = currentUser.balance + ' PKR';
        populatePlans();
        populatePlanSelect();
        populateActivePlans();
        populateTransactions();
    } else {
        document.getElementById('welcome').innerText = 'Not logged in';
        document.getElementById('logoutBtn').style.display='none';
        showTab('authSection');
    }
}

function simpleSignup(){
    const user=document.getElementById('inpUser').value.trim();
    const pass=document.getElementById('inpPass').value.trim();
    if(!user || !pass){alert('Enter username and password'); return;}
    if(realUser && realUser.user===user){alert('User exists'); return;}
    realUser={user:user,pass:pass,balance:0,activePlans:[]};
    currentUser = realUser;
    saveUser();
    updateUI();
    notify('Signup successful');
}

function simpleLogin(){
    const user=document.getElementById('inpUser').value.trim();
    const pass=document.getElementById('inpPass').value.trim();
    if(!user || !pass){alert('Enter username and password'); return;}
    if(!realUser || realUser.user!==user || realUser.pass!==pass){alert('Invalid credentials'); return;}
    currentUser = realUser;
    saveUser();
    updateUI();
    notify('Login successful');
}

function simpleLogout(){
    currentUser=null;
    localStorage.setItem('verbose_current',null);
    updateUI();
    notify('Logged out');
}

/* ------------------------------ Tabs ------------------------------ */
function showTab(tab){
    document.querySelectorAll('main section').forEach(sec=>sec.classList.remove('active'));
    document.getElementById(tab).classList.add('active');
    navButtons.forEach(btn=>btn.classList.remove('active'));
    const activeBtn=[...navButtons].find(b=>b.dataset.tab===tab);
    if(activeBtn) activeBtn.classList.add('active');
}

/* ------------------------------ Notifications ------------------------------ */
function notify(msg){
    const n=document.createElement('div');
    n.className='notif';
    n.innerText=msg;
    document.getElementById('notifRoot').appendChild(n);
    setTimeout(()=>n.remove(),3000);
}

/* ------------------------------ Plans ------------------------------ */
function populatePlans(){
    const container=document.getElementById('planGrid');
    container.innerHTML='';
    plans.forEach(p=>{
        const div=document.createElement('div');
        div.className='plan-card';
        div.innerHTML=`<h3>${p.name}</h3>
                       <div class="small muted">Invest: ${p.invest} PKR</div>
                       <div class="small muted">Days: ${p.days}</div>
                       <div class="small muted">Total Profit: ${p.totalProfit} PKR</div>
                       ${p.offer?'<div class="badge">HOT</div>':''}
                       <button class="primary" style="margin-top:8px" onclick="startPlan(${p.id})">Buy Now</button>`;
        container.appendChild(div);
    });
}

function populatePlanSelect(){
    const sel=document.getElementById('planSelect');
    sel.innerHTML='';
    plans.forEach(p=>{
        const opt=document.createElement('option');
        opt.value=p.id;
        opt.innerText=`${p.name} - ${p.invest} PKR`;
        sel.appendChild(opt);
    });
    onPlanChange();
}

function onPlanChange(){
    const id=parseInt(document.getElementById('planSelect').value);
    const plan=plans.find(p=>p.id===id);
    if(plan){
        document.getElementById('amountInput').value=plan.invest;
        onMethodChange();
    }
}

function onMethodChange(){
    const method=document.getElementById('payMethod').value;
    if(method==='jazzcash') document.getElementById('payNumber').value='03705519562';
    else document.getElementById('payNumber').value='03379827882';
}

function copyText(txt){navigator.clipboard.writeText(txt); notify('Copied!');}
function copyCurrentNumber(){copyText(document.getElementById('payNumber').value);}

/* ------------------------------ Deposit ------------------------------ */
function submitDeposit(){
    const planId=parseInt(document.getElementById('planSelect').value);
    const plan=plans.find(p=>p.id===planId);
    if(!plan){alert('Select plan'); return;}
    const tx={type:'Deposit',amount:plan.invest,plan:plan.name,time:new Date().toLocaleString(),status:'Completed'};
    transactions.push(tx);
    currentUser.balance += plan.invest;
    currentUser.activePlans.push(plan);
    saveUser();
    populateActivePlans();
    populateTransactions();
    document.getElementById('balDisplay').innerText = currentUser.balance + ' PKR';
    notify('Deposit successful');
    showTab('dashboard');
}

/* ------------------------------ Withdrawal ------------------------------ */
function submitWithdrawal(){
    const amt=parseFloat(document.getElementById('withdrawAmount').value);
    const acc=document.getElementById('withdrawAccount').value.trim();
    const method=document.getElementById('withdrawMethod').value;
    if(!amt || amt<=0 || !acc){alert('Enter valid amount & account'); return;}
    if(amt>currentUser.balance){alert('Insufficient balance'); return;}
    const tx={type:'Withdrawal',amount:amt,plan:'-',time:new Date().toLocaleString(),status:'Requested'};
    transactions.push(tx);
    currentUser.balance -= amt;
    saveUser();
    populateTransactions();
    document.getElementById('balDisplay').innerText = currentUser.balance + ' PKR';
    notify('Withdrawal requested');
    showTab('dashboard');
}

/* ------------------------------ Active Plans ------------------------------ */
function populateActivePlans(){
    const div=document.getElementById('activePlans');
    if(currentUser.activePlans.length===0) div.innerText='No active plans';
    else div.innerHTML=currentUser.activePlans.map(p=>`${p.name} (${p.invest} PKR)`).join(', ');
}

/* ------------------------------ Transactions ------------------------------ */
function populateTransactions(){
    const tbody=document.querySelector('#txTable tbody');
    tbody.innerHTML='';
    transactions.forEach(tx=>{
        const tr=document.createElement('tr');
        tr.innerHTML=`<td>${tx.type}</td><td>${tx.amount}</td><td>${tx.plan}</td><td>${tx.time}</td><td>${tx.status}</td>`;
        tbody.appendChild(tr);
    });
}

/* ------------------------------ Back to Dashboard ------------------------------ */
function backToDashboard(){showTab('dashboard');}

/* ------------------------------ Init ------------------------------ */
updateUI();
</script>
</body>
</html>
        document.getElementById('welcome').innerText=`Hi, ${currentUser.user}`;
        document.getElementById('logoutBtn').style.display='block';
        document.getElementById('
