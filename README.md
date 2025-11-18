<VERBOSE>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>VERBOSE — Premium Earning Platform (Neon)</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
<style>
  :root{
    --neon:#00fff0;
    --accent:#ffea00;
    --bg1:#050012;
    --bg2:#0d0626;
    --glass: rgba(255,255,255,0.04);
    --glass-border: rgba(0,255,255,0.08);
  }
  *{box-sizing:border-box}
  html,body{height:100%;margin:0;font-family:'Orbitron',sans-serif;background:#000;color:#fff}
  body{background:linear-gradient(120deg,var(--bg1),var(--bg2));overflow:auto}
  .bg-gradient { position:fixed;inset:0;z-index:-2; background:linear-gradient(270deg,#0f0530,#1b0a4a,#061022); background-size:800% 800%; animation:grad 18s ease infinite; filter:blur(28px) saturate(120%); opacity:0.9; }
  @keyframes grad{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
  #particles{position:fixed;inset:0;z-index:-1;pointer-events:none}
  .wrap{max-width:1200px;margin:20px auto;padding:18px}
  header{display:flex;align-items:center;justify-content:space-between;gap:12px}
  h1{color:var(--neon);margin:0;text-shadow:0 0 12px rgba(0,255,240,0.12)}
  .top-row{display:flex;gap:12px;align-items:center}
  .small{font-size:13px;color:#cfe;opacity:.9}
  .card{background:var(--glass);border:1px solid var(--glass-border);padding:14px;border-radius:12px;box-shadow:0 8px 30px rgba(0,0,0,0.6);backdrop-filter: blur(6px)}
  nav{display:flex;gap:8px;flex-direction:column}
  nav button{display:flex;align-items:center;gap:8px;padding:10px;border-radius:10px;border:none;background:transparent;color:var(--neon);cursor:pointer}
  nav button.active{box-shadow:0 0 18px rgba(0,255,240,0.12);background:linear-gradient(90deg,rgba(0,255,240,0.02),transparent)}
  main{display:grid;grid-template-columns:320px 1fr;gap:18px;margin-top:18px}
  @media(max-width:900px){main{grid-template-columns:1fr}}
  .auth-wrap{display:flex;gap:12px;align-items:center;justify-content:center;flex-direction:column;padding:30px}
  input,select,textarea{width:100%;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:rgba(0,0,0,0.35);color:#fff;outline:none}
  .row{display:flex;gap:10px;align-items:center;flex-wrap:wrap}
  .primary{background:var(--neon);color:#000;border:none;padding:10px 12px;border-radius:10px;cursor:pointer;font-weight:700}
  .ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:10px;border-radius:8px;color:#fff;cursor:pointer}
  .plan-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:12px;margin-top:12px}
  .plan{border-radius:12px;padding:12px;border:1px solid rgba(0,255,240,0.06);background:linear-gradient(180deg,rgba(0,255,240,0.02),transparent);position:relative;transition:transform .18s}
  .plan:hover{transform:translateY(-8px);box-shadow:0 16px 40px rgba(0,255,240,0.04)}
  .badge{position:absolute;top:10px;right:10px;background:var(--accent);color:#000;padding:6px 8px;border-radius:8px;font-weight:800}
  .muted{color:#bfe;opacity:.9}
  table{width:100%;border-collapse:collapse}
  th,td{padding:8px;border-bottom:1px solid rgba(255,255,255,0.03);font-size:13px}
  .notif{position:fixed;right:18px;bottom:18px;background:var(--neon);color:#000;padding:12px 16px;border-radius:12px;font-weight:700;box-shadow:0 10px 30px rgba(0,255,240,0.12)}
  footer{margin-top:30px;color:#9ef;opacity:.8;text-align:center}
  .icon{width:20px;height:20px;display:inline-block;vertical-align:middle;color:#fff}
  .hidden{display:none}
  .glow{box-shadow:0 8px 40px rgba(0,255,240,0.06)}
  .admin-badge{background:#ff4d4d;color:#000;padding:6px 8px;border-radius:8px;font-weight:800}
  a.link{color:var(--neon);text-decoration:none}
</style>
</head>
<body>
  <div class="bg-gradient"></div>
  <canvas id="particles"></canvas>

  <div class="wrap">
    <header>
      <div>
        <h1>VERBOSE</h1>
        <div class="small">Owner: John Wilson • Launch Date: 17 Nov 2025</div>
      </div>

      <div class="top-row">
        <div id="welcomeText" class="small muted">Not logged in</div>
        <button id="openAuthBtn" class="primary">Login / Signup</button>
        <div id="balanceTop" style="font-weight:800;color:var(--neon);margin-left:8px"></div>
        <button id="logoutBtn" class="primary hidden" style="background:linear-gradient(90deg,#ff4d4d,#ff1a1a)">Logout</button>
      </div>
    </header>

    <main>
      <aside class="card">
        <nav id="leftNav">
          <button data-tab="dashboard" class="active">📊 Dashboard</button>
          <button data-tab="plans">💼 Plans</button>
          <button data-tab="deposit">💰 Deposit</button>
          <button data-tab="withdrawal">💸 Withdrawal</button>
          <button data-tab="transactions">🧾 Transactions</button>
          <button data-tab="admin" id="adminNavBtn" class="hidden">🛠 Admin Panel</button>
          <button data-tab="about">📑 Company</button>
        </nav>
        <hr style="border:none;border-top:1px solid rgba(255,255,255,0.04);margin:12px 0"/>
        <div class="muted small">Payment Numbers</div>
        <div style="margin-top:10px">
          <div style="display:flex;justify-content:space-between;align-items:center;">
            <div><strong>JazzCash</strong><div class="muted small">03705519562</div></div>
            <button class="ghost" onclick="copyText('03705519562')">Copy</button>
          </div>
          <hr style="border:none;border-top:1px solid rgba(255,255,255,0.03);margin:8px 0"/>
          <div style="display:flex;justify-content:space-between;align-items:center;">
            <div><strong>EasyPaisa</strong><div class="muted small">03379827882</div></div>
            <button class="ghost" onclick="copyText('03379827882')">Copy</button>
          </div>
        </div>
      </aside>

      <section style="min-height:520px">
        <div id="authView" class="card auth-wrap">
          <h2 style="margin:0">Login / Signup</h2>
          <div style="width:360px;max-width:100%">
            <input id="authUser" placeholder="Username" />
            <input id="authPass" placeholder="Password" type="password" style="margin-top:8px"/>
            <div class="row" style="margin-top:10px">
              <button class="primary" onclick="doLogin()">Login</button>
              <button class="ghost" onclick="doSignup()">Signup</button>
            </div>
            <div class="muted small" style="margin-top:10px">Signup saves locally. Admin: <strong>AdminKhan</strong> / <strong>SuperSecret123</strong></div>
          </div>
        </div>

        <div id="appView" class="hidden">
          <div id="dashboard" class="card glow">
            <div style="display:flex;justify-content:space-between;align-items:center">
              <div>
                <h2 style="margin:0">Dashboard</h2>
                <div class="muted small" id="dashSub">Overview</div>
              </div>
              <div class="row">
                <div class="card" style="padding:8px">
                  <div class="muted small">Balance</div>
                  <div id="balDisplay" style="font-weight:800;color:var(--neon);font-size:18px">0 PKR</div>
                </div>
                <div class="card" style="padding:8px;margin-left:8px">
                  <div class="muted small">Profit</div>
                  <div id="profitDisplay" style="font-weight:800;color:var(--neon);font-size:18px">0 PKR</div>
                </div>
              </div>
            </div>
            <hr style="border:none;border-top:1px solid rgba(255,255,255,0.04);margin:12px 0"/>
            <div style="display:flex;gap:12px;flex-wrap:wrap">
              <div class="card" style="flex:1;min-width:220px">
                <h4 style="margin:0">Active Plans</h4>
                <div id="activePlans" class="muted small" style="margin-top:8px">No active plans</div>
              </div>
              <div class="card" style="flex:1;min-width:220px">
                <h4 style="margin:0">Achievements</h4>
                <div id="achv" class="muted small" style="margin-top:8px">No badges yet</div>
              </div>
            </div>
          </div>

          <div id="plansView" class="card hidden">
            <h2 style="margin-top:0">Plans</h2>
            <div class="plan-grid" id="planGrid"></div>
          </div>

          <div id="depositView" class="card hidden">
            <h2 style="margin-top:0">Deposit</h2>
            <div class="muted small">Choose plan, send payment, upload proof (simulation).</div>
            <div style="margin-top:12px">
              <label class="muted small">Choose Plan</label>
              <select id="depositPlan" onchange="onDepositPlanChange()"></select>
            </div>
            <div style="display:flex;gap:10px;margin-top:8px;align-items:center;">
              <div style="flex:1">
                <label class="muted small">Amount (auto)</label>
                <input id="depositAmount" readonly />
              </div>
              <div style="width:160px">
                <label class="muted small">Method</label>
                <select id="depositMethod" onchange="updateDepositNumber()">
                  <option value="jazzcash">JazzCash</option>
                  <option value="easypaisa">EasyPaisa</option>
                </select>
              </div>
            </div>
            <div style="display:flex;gap:8px;margin-top:10px;align-items:center">
              <input id="depositNumber" readonly style="flex:1" />
              <button class="ghost" onclick="copyText(document.getElementById('depositNumber').value)">Copy</button>
            </div>
            <div style="margin-top:8px"><input id="depositTx" placeholder="Transaction ID (optional)" /></div>
            <div style="margin-top:8px"><input id="depositProof" type="file" /></div>
            <div style="margin-top:10px" class="row">
              <button class="primary" onclick="submitDeposit()">Submit Deposit</button>
              <button class="ghost" onclick="navigate('dashboard')">Back</button>
            </div>
          </div>

          <div id="withdrawView" class="card hidden">
            <h2>Withdrawal</h2>
            <div class="muted small">Request withdrawal (simulated).</div>
            <div style="margin-top:8px">
              <label class="muted small">Method</label>
              <select id="withdrawMethod"><option value="jazzcash">JazzCash</option><option value="easypaisa">EasyPaisa</option></select>
            </div>
            <div style="margin-top:8px"><input id="withdrawAccount" placeholder="Account Number / Username" /></div>
            <div style="margin-top:8px"><input id="withdrawAmount" type="number" placeholder="Amount PKR" /></div>
            <div style="margin-top:10px" class="row">
              <button class="primary" onclick="submitWithdraw()">Request Withdrawal</button>
              <button class="ghost" onclick="navigate('dashboard')">Back</button>
            </div>
          </div>

          <div id="txView" class="card hidden">
            <h2>Transactions</h2>
            <div style="overflow:auto;max-height:320px">
              <table id="txTable"><thead><tr><th>Type</th><th>Amount</th><th>Plan</th><th>Days</th><th>Total Profit</th><th>Time</th><th>Status</th></tr></thead><tbody></tbody></table>
            </div>
          </div>

          <div id="adminView" class="card hidden">
            <h2>Admin Panel</h2>
            <div class="muted small">Manage users & transactions.</div>
            <div style="margin-top:12px">
              <h4>Users</h4>
              <table id="userTable"><thead><tr><th>Username</th><th>Balance</th><th>Active Plans</th><th>Action</th></tr></thead><tbody></tbody></table>
            </div>
            <div style="margin-top:12px">
              <h4>Pending Transactions</h4>
              <table id="adminTx"><thead><tr><th>User</th><th>Type</th><th>Amount</th><th>Plan</th><th>Time</th><th>Status</th><th>Action</th></tr></thead><tbody></tbody></table>
            </div>
          </div>

          <div id="aboutView" class="card hidden">
            <h2>About VERBOSE</h2>
            <p class="muted small">VERBOSE ek premium earning platform hai jo modern digital finance ko neon style me present karta hai. Owner: <strong>John Wilson</strong>. Launch Date: <strong>17 Nov 2025</strong>.</p>
          </div>
        </div>

        <footer class="muted small" style="margin-top:16px">Made with neon ✨</footer>
      </section>
    </main>
  </div>

  <div id="toastRoot"></div>

<script>
/* ---------- Particles ---------- */
const canvas = document.getElementById('particles');
const ctx = canvas.getContext('2d');
function resizeCanvas(){canvas.width=innerWidth;canvas.height=innerHeight;}
resizeCanvas();addEventListener('resize',resizeCanvas);
const particles=[];
for(let i=0;i<140;i++){particles.push({x:Math.random()*innerWidth,y:Math.random()*innerHeight,r:Math.random()*1.6+0.6,vx:(Math.random()-0.5)*0.6,vy:(Math.random()-0.5)*0.6,h:180+Math.random()*80});}
function drawParticles(){ctx.clearRect(0,0,canvas.width,canvas.height);for(const p of particles){ctx.beginPath();ctx.fillStyle=`hsla(${p.h},100%,60%,0.12)`;ctx.shadowBlur=12;ctx.shadowColor=`hsla(${p.h},100%,60%,0.14)`;ctx.fillRect(p.x,p.y,p.r*2,p.r*2);p.x+=p.vx;p.y+=p.vy;if(p.x<0)p.x=canvas.width;if(p.x>canvas.width)p.x=0;if(p.y<0)p.y=canvas.height;if(p.y>canvas.height)p.y=0;}requestAnimationFrame(drawParticles);}
drawParticles();

/* ---------- App data / storage ---------- */
const ADMIN = {user:'AdminKhan',pass:'SuperSecret123'};
const depositNumbers = {jazzcash:'03705519562',easypaisa:'03379827882'};

/* Plans (25). First 5 are special 24h offers. */
const now = Date.now();
const specialExpiryKey = 'verbose_special_expiry';
let specialExpiry = parseInt(localStorage.getItem(specialExpiryKey) || '0', 10);
if(!specialExpiry || specialExpiry < Date.now()){
  specialExpiry = Date.now() + 24*60*60*1000; // set fresh 24h from first load
  localStorage.setItem(specialExpiryKey, String(specialExpiry));
}

const plans = [
{ id:1, name:'Plan 1', days:25, invest:250, totalProfit:1200, offer:true },
{ id:2, name:'Plan 2', days:28, invest:500, totalProfit:2500, offer:true },
{ id:3, name:'Plan 3', days:30, invest:750, totalProfit:3750, offer:true },
{ id:4, name:'Plan 4', days:33, invest:1000, totalProfit:4000, offer:true },
{ id:5, name:'Plan 5', days:35, invest:1500, totalProfit:7700, offer:true },
{ id:6, name:'Plan 6', days:38, invest:2000, totalProfit:5700, offer:false },
{ id:7, name:'Plan 7', days:40, invest:2500, totalProfit:7200, offer:false },
{ id:8, name:'Plan 8', days:43, invest:3000, totalProfit:9000, offer:false },
{ id:9, name:'Plan 9', days:45, invest:3500, totalProfit:10800, offer:false },
{ id:10,name:'Plan 10',days:48, invest:4000, totalProfit:13200, offer:false },
{ id:11,name:'Plan 11',days:50, invest:5000, totalProfit:15000, offer:false },
{ id:12,name:'Plan 12',days:53, invest:6000, totalProfit:18000, offer:false },
{ id:13,name:'Plan 13',days:55, invest:7000, totalProfit:21000, offer:false },
{ id:14,name:'Plan 14',days:58, invest:8000, totalProfit:24000, offer:false },
{ id:15,name:'Plan 15',days:60, invest:9000, totalProfit:27000, offer:false },
{ id:16,name:'Plan 16',days:63, invest:10000,totalProfit:30000, offer:false },
{ id:17,name:'Plan 17',days:65, invest:12000,totalProfit:33000, offer:false },
{ id:18,name:'Plan 18',days:68, invest:14000,totalProfit:36000, offer:false },
{ id:19,name:'Plan 19',days:70, invest:16000,totalProfit:39000, offer:false },
{ id:20,name:'Plan 20',days:72, invest:18000,totalProfit:42000, offer:false },
{ id:21,name:'Plan 21',days:75, invest:20000,totalProfit:45000, offer:false },
{ id:22,name:'Plan 22',days:77, invest:25000,totalProfit:60000, offer:false },
{ id:23,name:'Plan 23',days:79, invest:30000,totalProfit:80000, offer:false },
{ id:24,name:'Plan 24',days:80, invest:40000,totalProfit:100000,offer:false },
{ id:25,name:'Plan 25',days:85, invest:60000,totalProfit:150000,offer:false }
];
for(const p of plans){ p.dailyProfit = Math.round(p.totalProfit / p.days); }

/* Storage helpers */
function getUsers(){ return JSON.parse(localStorage.getItem('verbose_users')||'[]'); }
function setUsers(u){ localStorage.setItem('verbose_users', JSON.stringify(u)); }
function getTx(){ return JSON.parse(localStorage.getItem('verbose_tx')||'[]'); }
function setTx(t){ localStorage.setItem('verbose_tx', JSON.stringify(t)); }
function getCurrent(){ return JSON.parse(localStorage.getItem('verbose_current')||'null'); }
function setCurrent(u){ localStorage.setItem('verbose_current', JSON.stringify(u)); }

/* ensure admin exists */
(function initUsers(){ let u = getUsers(); if(!u.find(x=>x.user===ADMIN.user)){ u.push({user:ADMIN.user,pass:ADMIN.pass,balance:0,active:[],profit:0,admin:true}); setUsers(u);} })();

/* ---------- UI helpers ---------- */
function $(id){return document.getElementById(id)}
function showToast(text){ const n=document.createElement('div'); n.className='notif'; n.innerText=text; document.body.appendChild(n); setTimeout(()=>n.remove(),2200); }
function copyText(t){ navigator.clipboard?.writeText(t).then(()=>showToast('Copied')) }

/* ---------- Navigation ---------- */
const leftButtons = document.querySelectorAll('nav button[data-tab]');
leftButtons.forEach(btn=>btn.addEventListener('click',()=>navigate(btn.dataset.tab)));
function navigate(tab){
  const views = { dashboard:$('dashboard'), plans:$('plansView'), deposit:$('depositView'), withdrawal:$('withdrawView'), transactions:$('txView'), admin:$('adminView'), about:$('aboutView') };
  $('authView').classList.add('hidden');
  $('appView').classList.remove('hidden');
  for(const b of leftButtons) b.classList.remove('active');
  document.querySelector(`nav button[data-tab="${tab}"]`)?.classList.add('active');
  for(const v of Object.values(views)) if(v) v.classList.add('hidden');
  if(views[tab]) views[tab].classList.remove('hidden');
  if(!views[tab]) $('dashboard').classList.remove('hidden');
  renderCommon();
}

/* ---------- Auth ---------- */
function doSignup(){
  const u = $('authUser').value.trim(); const p = $('authPass').value;
  if(!u||!p){ showToast('Enter username & password'); return; }
  let users = getUsers();
  if(users.find(x=>x.user===u)){ showToast('Username exists'); return; }
  users.push({user:u,pass:p,balance:0,active:[],profit:0,admin:false});
  setUsers(users);
  setCurrent({user:u,admin:false});
  showToast('Signup successful — logged in');
  afterLogin();
}
function doLogin(){
  const u = $('authUser').value.trim(); const p = $('authPass').value;
  if(!u||!p){ showToast('Enter username & password'); return; }
  if(u===ADMIN.user && p===ADMIN.pass){ setCurrent({user:ADMIN.user,admin:true}); showToast('Admin logged in'); afterLogin(); return; }
  let users = getUsers();
  const found = users.find(x=>x.user===u && x.pass===p);
  if(!found){ showToast('Invalid credentials'); return; }
  setCurrent({user:found.user,admin:false});
  showToast('Login successful');
  afterLogin();
}
function doLogout(){
  localStorage.removeItem('verbose_current');
  $('authView').classList.remove('hidden');
  $('appView').classList.add('hidden');
  $('welcomeText').innerText = 'Not logged in';
  $('logoutBtn').classList.add('hidden');
  $('balanceTop').innerText = '';
  // hide admin nav
  $('adminNavBtn').classList.add('hidden');
  // reset auth inputs
  $('authUser').value=''; $('authPass').value='';
  // navigate to auth
  for(const b of leftButtons) b.classList.remove('active');
  document.querySelector('nav button[data-tab="dashboard"]').classList.add('active');
}

/* after login */
function afterLogin(){
  const cur = getCurrent();
  if(!cur){ return doLogout(); }
  $('authView').classList.add('hidden');
  $('appView').classList.remove('hidden');
  $('logoutBtn').classList.remove('hidden');
  $('welcomeText').innerText = 'Welcome, ' + cur.user;
  // show admin nav if admin
  if(cur.admin) $('adminNavBtn').classList.remove('hidden'); else $('adminNavBtn').classList.add('hidden');
  // populate plans, dropdowns, tables
  renderPlans();
  populateDepositDropdown();
  renderTxTable();
  renderAdminTables();
  renderCommon();
}

/* ---------- Render common user data ---------- */
function renderCommon(){
  const cur = getCurrent();
  const users = getUsers();
  let userObj = null;
  if(cur) userObj = users.find(u=>u.user===cur.user);
  const balance = userObj ? userObj.balance : 0;
  const profit = userObj ? userObj.profit : 0;
  $('balDisplay').innerText = (balance||0) + ' PKR';
  $('profitDisplay').innerText = (profit||0) + ' PKR';
  $('balanceTop').innerText = (userObj ? (userObj.balance + ' PKR') : '');
  // active plans text
  if(userObj && userObj.active && userObj.active.length>0){
    $('activePlans').innerHTML = userObj.active.map(a=>`<div>${a.name} — ${a.invest} PKR — ${a.days} days — status: ${a.status || 'running'}</div>`).join('');
  } else { $('activePlans').innerText = 'No active plans'; }
  // show whether special offers still active
  const timeLeft = specialExpiry - Date.now();
  // update plan badges if needed
  // (renderPlans handles this)
}

/* ---------- Plans rendering ---------- */
function renderPlans(){
  const grid = $('planGrid'); grid.innerHTML = '';
  const specialActive = Date.now() < specialExpiry;
  for(const p of plans){
    const offerNow = p.offer && specialActive;
    const div = document.createElement('div'); div.className='plan';
    let html = `<h4 style="margin:0">${p.name}${offerNow? ' <span style="font-size:12px;color:#ffea00"> (24h OFFER)</span>':''}</h4>`;
    html += `<div style="margin-top:8px">Invest: <strong>${p.invest} PKR</strong></div>`;
    html += `<div>Days: ${p.days}</div>`;
    html += `<div>Daily Profit: ${p.dailyProfit} PKR</div>`;
    html += `<div>Total Profit: ${p.totalProfit} PKR</div>`;
    html += `<div style="margin-top:8px"><button class="primary" onclick="selectPlan(${p.id})">Buy / Deposit</button></div>`;
    if(offerNow) html += `<div class="badge">Special</div>`;
    div.innerHTML = html;
    grid.appendChild(div);
  }
}

/* select plan to deposit */
function selectPlan(planId){
  navigate('deposit');
  $('depositPlan').value = planId;
  onDepositPlanChange();
}

/* populate deposit dropdown */
function populateDepositDropdown(){
  const sel = $('depositPlan'); sel.innerHTML = '';
  for(const p of plans){ const opt = document.createElement('option'); opt.value=p.id; opt.innerText = `${p.name} — ${p.invest} PKR`; sel.appendChild(opt); }
  onDepositPlanChange();
}
function onDepositPlanChange(){
  const planId = parseInt($('depositPlan').value || plans[0].id,10);
  const p = plans.find(x=>x.id===planId);
  if(!p) return;
  $('depositAmount').value = p.invest;
  updateDepositNumber();
}
function updateDepositNumber(){
  const m = $('depositMethod').value;
  $('depositNumber').value = (m==='jazzcash' ? depositNumbers.jazzcash : depositNumbers.easypaisa);
}

/* ---------- Transactions / Deposit / Withdraw ---------- */
function submitDeposit(){
  const cur = getCurrent();
  if(!cur){ showToast('Login first'); return; }
  const planId = parseInt($('depositPlan').value,10);
  const p = plans.find(x=>x.id===planId);
  if(!p){ showToast('Select plan'); return; }
  const amount = p.invest;
  const txs = getTx();
  const tx = { id: 'tx_' + Date.now(), user:cur.user, type:'deposit', amount, planId, planName:p.name, days:p.days, totalProfit:p.totalProfit, time: new Date().toISOString(), status:'pending' };
  txs.push(tx); setTx(txs);
  showToast('Deposit submitted — pending admin approval');
  renderTxTable(); renderAdminTables();
  navigate('transactions');
}
function submitWithdraw(){
  const cur = getCurrent();
  if(!cur){ showToast('Login first'); return; }
  const acc = $('withdrawAccount').value.trim(); const amt = Number($('withdrawAmount').value);
  if(!acc || !amt || amt<=0){ showToast('Enter valid details'); return; }
  const txs = getTx();
  const tx = { id:'tx_'+Date.now(), user:cur.user, type:'withdraw', amount:amt, planId:null, planName:null, time: new Date().toISOString(), status:'pending', account:acc };
  txs.push(tx); setTx(txs);
  showToast('Withdrawal requested — pending admin');
  renderTxTable(); renderAdminTables();
  navigate('transactions');
}

/* render transaction table for user */
function renderTxTable(){
  const cur = getCurrent();
  const tbody = $('txTable').querySelector('tbody');
  tbody.innerHTML = '';
  const txs = getTx().slice().reverse();
  for(const t of txs){
    if(!cur || t.user !== cur.user){ continue; }
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${t.type}</td><td>${t.amount}</td><td>${t.planName || '-'}</td><td>${t.days || '-'}</td><td>${t.totalProfit || '-'}</td><td>${new Date(t.time).toLocaleString()}</td><td>${t.status}</td>`;
    tbody.appendChild(tr);
  }
}

/* ---------- Admin functions ---------- */
function renderAdminTables(){
  const users = getUsers();
  const txs = getTx();
  // users table
  const ubody = $('userTable').querySelector('tbody'); ubody.innerHTML = '';
  for(const u of users){
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${u.user}${u.admin? ' <span class="admin-badge">ADMIN</span>':''}</td><td>${u.balance||0}</td><td>${(u.active||[]).length}</td><td><button class="ghost" onclick="viewUser('${u.user}')">View</button></td>`;
    ubody.appendChild(tr);
  }
  // adminTx
  const tbody = $('adminTx').querySelector('tbody'); tbody.innerHTML = '';
  for(const t of txs.filter(x=>x.status==='pending').reverse()){
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${t.user}</td><td>${t.type}</td><td>${t.amount}</td><td>${t.planName||'-'}</td><td>${new Date(t.time).toLocaleString()}</td><td id="status_${t.id}">${t.status}</td><td>
      <button class="primary" onclick="adminApprove('${t.id}')">Approve</button>
      <button class="ghost" onclick="adminReject('${t.id}')">Reject</button>
    </td>`;
    tbody.appendChild(tr);
  }
}

/* admin view user (simple) */
function viewUser(username){
  const users = getUsers(); const u = users.find(x=>x.user===username);
  if(!u){ showToast('User not found'); return; }
  alert(`User: ${u.user}\nBalance: ${u.balance||0}\nActive plans: ${(u.active||[]).length}`);
}

/* approve tx */
function adminApprove(txId){
  let txs = getTx(); const idx = txs.findIndex(x=>x.id===txId); if(idx<0){ showToast('Tx not found'); return; }
  const tx = txs[idx];
  tx.status = 'approved';
  txs[idx] = tx; setTx(txs);
  // process
  let users = getUsers();
  const uidx = users.findIndex(x=>x.user===tx.user);
  if(uidx>=0){
    if(tx.type==='deposit'){
      // credit balance
      users[uidx].balance = (users[uidx].balance||0) + tx.amount;
      // add active plan entry
      const plan = plans.find(p=>p.id===tx.planId) || {name:tx.planName, invest:tx.amount, days:tx.days, totalProfit:tx.totalProfit};
      const active = { id: tx.planId || ('p'+Date.now()), name: plan.name, invest: plan.invest||tx.amount, days: plan.days||tx.days, totalProfit: plan.totalProfit||tx.totalProfit, dailyProfit: Math.round((plan.totalProfit||tx.totalProfit)/(plan.days||tx.days)), start: new Date().toISOString(), status:'active' };
      users[uidx].active = users[uidx].active || [];
      users[uidx].active.push(active);
      users[uidx].profit = (users[uidx].profit||0) + (plan.totalProfit||tx.totalProfit);
      showToast(`Deposit approved and ${tx.amount} PKR credited to ${tx.user}`);
    } else if(tx.type==='withdraw'){
      // deduct if sufficient balance
      const bal = users[uidx].balance || 0;
      if(bal >= tx.amount){
        users[uidx].balance = bal - tx.amount;
        showToast(`Withdrawal of ${tx.amount} approved for ${tx.user}`);
      } else {
        showToast('User balance insufficient — cannot pay; mark as rejected instead');
        tx.status = 'rejected';
        txs[idx] = tx; setTx(txs);
      }
    }
    setUsers(users);
  }
  setTx(txs);
  renderAdminTables(); renderTxTable(); renderCommon();
}

/* reject tx */
function adminReject(txId){
  let txs = getTx(); const idx = txs.findIndex(x=>x.id===txId); if(idx<0){ showToast('Tx not found'); return; }
  txs[idx].status = 'rejected'; setTx(txs); showToast('Transaction rejected');
  renderAdminTables(); renderTxTable();
}

/* ---------- load handlers ---------- */
$('openAuthBtn').addEventListener('click', ()=>{ $('authView').classList.remove('hidden'); $('appView').classList.add('hidden'); });
$('logoutBtn').addEventListener('click', ()=>{ doLogout(); showToast('Logged out'); });

/* initial render */
function init(){
  // wire left nav buttons to navigate
  leftButtons.forEach(b=>b.addEventListener('click',()=>{ navigate(b.dataset.tab); }));

  // pre-populate deposit dropdown
  populateDepositDropdown();
  updateDepositNumber();

  // if user logged in (persisted), restore session
  const cur = getCurrent();
  if(cur){ afterLogin(); renderCommon(); renderPlans(); }
  else { // show auth
    $('authView').classList.remove('hidden'); $('appView').classList.add('hidden');
  }
  // click default nav to setup handlers
  document.querySelector('nav button[data-tab="dashboard"]').addEventListener('click', ()=> navigate('dashboard'));
  // expose functions globally for HTML onclick usage
  window.selectPlan = selectPlan;
  window.copyText = copyText;
  window.onDepositPlanChange = onDepositPlanChange;
  window.updateDepositNumber = updateDepositNumber;
  window.submitDeposit = submitDeposit;
  window.submitWithdraw = submitWithdraw;
  window.adminApprove = adminApprove;
  window.adminReject = adminReject;
  window.viewUser = viewUser;
}
init();

/* When data changes elsewhere, update UI */
window.addEventListener('storage', (e)=>{
  // if other tab changed users/tx/current, re-render
  renderCommon(); renderTxTable(); renderAdminTables(); renderPlans(); populateDepositDropdown();
});
</script>
</body>
</html>
