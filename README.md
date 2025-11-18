<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>VERBOSE — Premium Platform</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
<style>
:root {
  --neon:#00ffff;
  --accent:#ffea00;
  --bg:#0f0c29;
}
*{box-sizing:border-box;}
html,body{height:100%;margin:0;font-family:'Orbitron',sans-serif;background:#000;color:#fff;overflow-x:hidden;}
body{background:linear-gradient(270deg,#0f0c29,#302b63,#24243e);background-size:600% 600%;animation:gradientBG 20s ease infinite;}
@keyframes gradientBG{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
header{display:flex;justify-content:space-between;align-items:center;padding:15px;}
header h1{color:var(--neon);text-shadow:0 0 15px var(--neon);}
header .small{font-size:13px;color:#cfe;}
nav{display:flex;gap:10px;flex-wrap:wrap;}
button{cursor:pointer;border:none;border-radius:10px;padding:10px 15px;font-weight:700;transition:0.2s;}
button.primary{background:var(--neon);color:#000;}
button.primary:hover{opacity:0.9;}
button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.2);color:#fff;}
button.ghost:hover{opacity:0.8;}
.layout{display:flex;gap:20px;padding:15px;}
aside{flex:0 0 200px;background:rgba(0,255,255,0.05);border-radius:12px;padding:15px;backdrop-filter:blur(5px);display:none;}
aside button{display:block;width:100%;margin-bottom:10px;text-align:left;}
main{flex:1;}
.card{background:rgba(0,255,255,0.05);padding:15px;border-radius:12px;margin-bottom:15px;backdrop-filter:blur(5px);}
input,select{width:100%;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.1);background:rgba(0,0,0,0.3);color:#fff;outline:none;}
table{width:100%;border-collapse:collapse;}
th,td{padding:8px;border-bottom:1px solid rgba(255,255,255,0.1);}
.muted{color:#ccc;}
#notif{position:fixed;bottom:20px;right:20px;background:var(--neon);color:#000;padding:12px 18px;border-radius:12px;display:none;}
</style>
</head>
<body>
<header>
  <div>
    <h1>VERBOSE</h1>
    <div class="small">Owner: John Wilson • Launch Date: 17 Nov 2025</div>
  </div>
  <div>
    <span id="welcomeText" class="muted"></span>
    <button id="authBtn" class="primary">Login / Signup</button>
    <button id="logoutBtn" class="primary" style="display:none;background:#ff4d4d;">Logout</button>
  </div>
</header>

<div class="layout">
  <aside id="sideNav">
    <button data-tab="dashboard" class="tabBtn">📊 Dashboard</button>
    <button data-tab="plans" class="tabBtn">💼 Plans</button>
    <button data-tab="deposit" class="tabBtn">💰 Deposit</button>
    <button data-tab="withdraw" class="tabBtn">💸 Withdrawal</button>
    <button data-tab="transactions" class="tabBtn">🧾 Transactions</button>
    <button data-tab="about" class="tabBtn">📑 Company</button>
  </aside>

  <main>
    <!-- Login / Signup -->
    <section id="authSection" class="card">
      <h2>Login / Signup</h2>
      <input id="username" placeholder="Username">
      <input id="password" type="password" placeholder="Password">
      <div style="margin-top:10px;">
        <button class="primary" onclick="loginUser()">Login</button>
        <button class="ghost" onclick="signupUser()">Signup</button>
      </div>
    </section>

    <!-- Dashboard -->
    <section id="dashboard" class="card" style="display:none;">
      <h2>Dashboard</h2>
      <p>Balance: <span id="balance">0</span> PKR</p>
      <p>Total Profit: <span id="totalProfit">0</span> PKR</p>
    </section>

    <!-- Plans -->
    <section id="plans" class="card" style="display:none;">
      <h2>Plans</h2>
      <div id="planContainer"></div>
    </section>

    <!-- Deposit -->
    <section id="deposit" class="card" style="display:none;">
      <h2>Deposit</h2>
      <label>Plan</label>
      <select id="depositPlan"></select>
      <label>Amount</label>
      <input id="depositAmount" readonly>
      <label>Method</label>
      <select id="depositMethod" onchange="updatePayNumber()">
        <option value="jazzcash">JazzCash</option>
        <option value="easypaisa">EasyPaisa</option>
      </select>
      <label>Pay Number</label>
      <div style="display:flex;gap:5px;">
        <input id="payNumber" readonly>
        <button class="ghost" onclick="copyNumber()">Copy</button>
      </div>
      <label>Transaction ID (optional)</label>
      <input id="txId">
      <label>Proof</label>
      <input type="file" id="proofFile">
      <button class="primary" onclick="submitDeposit()">Submit Deposit</button>
    </section>

    <!-- Withdrawal -->
    <section id="withdraw" class="card" style="display:none;">
      <h2>Withdrawal</h2>
      <label>Method</label>
      <select id="withdrawMethod">
        <option value="jazzcash">JazzCash</option>
        <option value="easypaisa">EasyPaisa</option>
      </select>
      <label>Account Number</label>
      <input id="withdrawAccount">
      <label>Amount</label>
      <input type="number" id="withdrawAmount">
      <button class="primary" onclick="submitWithdraw()">Request Withdrawal</button>
    </section>

    <!-- Transactions -->
    <section id="transactions" class="card" style="display:none;">
      <h2>Transactions</h2>
      <table>
        <thead>
          <tr><th>Type</th><th>Amount</th><th>Plan</th><th>Status</th></tr>
        </thead>
        <tbody id="txTable"></tbody>
      </table>
    </section>

    <!-- Company -->
    <section id="about" class="card" style="display:none;">
      <h2>About VERBOSE</h2>
      <p class="muted">
        VERBOSE ek premium earning platform hai jo modern digital finance ko neon style me present karta hai.<br>
        Owner: <strong>John Wilson</strong><br>
        Launch Date: <strong>17 Nov 2025</strong><br>
        Mission: Financial freedom, transparent plans aur easy UX.
      </p>
    </section>
  </main>
</div>

<div id="notif">Copied!</div>

<script>
let users = JSON.parse(localStorage.getItem('verbose_users')||'[]');
let currentUser = JSON.parse(localStorage.getItem('verbose_current')||'null');
let transactions = JSON.parse(localStorage.getItem('verbose_tx')||'[]');

const plans = [];
for(let i=1;i<=25;i++){
  plans.push({id:i,name:'Plan '+i,days:20+i,invest:250*i,dailyProfit:48*i,totalProfit:1200*i});
}

const depositNumbers = { jazzcash:'03705519562', easypaisa:'03379827882' };

const sideNav = document.getElementById('sideNav');
function showTab(tab){
  document.querySelectorAll('main section').forEach(s=>s.style.display='none');
  document.getElementById(tab).style.display='block';
}
document.querySelectorAll('.tabBtn').forEach(b=>b.addEventListener('click',()=>showTab(b.dataset.tab)));

function signupUser(){
  const u=document.getElementById('username').value;
  const p=document.getElementById('password').value;
  if(!u||!p){alert('Enter username and password');return;}
  if(users.find(x=>x.username===u)){alert('User exists');return;}
  users.push({username:u,password:p,balance:0});
  localStorage.setItem('verbose_users',JSON.stringify(users));
  alert('Signup successful');
}

function loginUser(){
  const u=document.getElementById('username').value;
  const p=document.getElementById('password').value;
  const user = users.find(x=>x.username===u && x.password===p);
  if(user){
    currentUser = user;
    localStorage.setItem('verbose_current',JSON.stringify(currentUser));
    sideNav.style.display='block';
    updateUI();
    showTab('dashboard');
  } else {alert('Invalid credentials');}
}

function logoutUser(){
  currentUser=null;
  localStorage.setItem('verbose_current',JSON.stringify(null));
  sideNav.style.display='none';
  updateUI();
  showTab('authSection');
}
document.getElementById('logoutBtn').addEventListener('click',logoutUser);
document.getElementById('authBtn').addEventListener('click',()=>showTab('authSection'));

function updateUI(){
  if(currentUser){
    document.getElementById('welcomeText').innerText='Welcome, '+currentUser.username;
    document.getElementById('authBtn').style.display='none';
    document.getElementById('logoutBtn').style.display='inline-block';
    document.getElementById('balance').innerText=currentUser.balance;
  }else{
    document.getElementById('welcomeText').innerText='';
    document.getElementById('authBtn').style.display='inline-block';
    document.getElementById('logoutBtn').style.display='none';
  }
  renderPlans();
  renderDepositPlans();
  renderTransactions();
}

function renderPlans(){
  const container=document.getElementById('planContainer');
  container.innerHTML='';
  plans.forEach(p=>{
    const div=document.createElement('div');
    div.className='card';
    div.innerHTML=`<strong>${p.name}</strong><br>Invest: ${p.invest} PKR<br>Days: ${p.days}<br>Total Profit: ${p.totalProfit}`;
    container.appendChild(div);
  });
}

function renderDepositPlans(){
  const sel=document.getElementById('depositPlan');
  sel.innerHTML='';
  plans.forEach(p=>{const o=document.createElement('option');o.value=p.id;o.text=p.name;sel.add(o);});
  updateDepositAmount();
}
document.getElementById('depositPlan').addEventListener('change',updateDepositAmount);
function updateDepositAmount(){
  const planId = +document.getElementById('depositPlan').value;
  const plan = plans.find(p=>p.id===planId);
  document.getElementById('depositAmount').value=plan ? plan.invest : '';
}
function updatePayNumber(){
  const method = document.getElementById('depositMethod').value;
  document.getElementById('payNumber').value = depositNumbers[method];
}
function copyNumber(){
  const num = document.getElementById('payNumber');
  num.select();num.setSelectionRange(0,99999);
  navigator.clipboard.writeText(num.value);
  const n=document.getElementById('notif'); n.style.display='block';
  setTimeout(()=>n.style.display='none',1000);
}
function submitDeposit(){
  const planId = +document.getElementById('depositPlan').value;
  const plan = plans.find(p=>p.id===planId);
  if(!plan){alert('Select plan');return;}
  const tx={type:'Deposit',amount:plan.invest,plan:plan.name,status:'Pending',user:currentUser.username};
  transactions.push(tx);
  localStorage.setItem('verbose_tx',JSON.stringify(transactions));
  currentUser.balance += plan.invest;
  users = users.map(u=>u.username===currentUser.username?currentUser:u);
  localStorage.setItem('verbose_users',JSON.stringify(users));
  localStorage.setItem('verbose_current',JSON.stringify(currentUser));
  updateUI();
  alert('Deposit submitted');
}
function submitWithdraw(){
  const amt=+document.getElementById('withdrawAmount').value;
  if(amt>currentUser.balance){alert('Insufficient balance');return;}
  const planName='N/A';
  const tx={type:'Withdrawal',amount:amt,plan:planName,status:'Pending',user:currentUser.username};
  transactions.push(tx);
  currentUser.balance -= amt;
  users = users.map(u=>u.username===currentUser.username?currentUser:u);
  localStorage.setItem('verbose_users',JSON.stringify(users));
  localStorage.setItem('verbose_current',JSON.stringify(currentUser));
  localStorage.setItem('verbose_tx',JSON.stringify(transactions));
  updateUI();
  alert('Withdrawal requested');
}
function renderTransactions(){
  const tbody=document.getElementById('txTable');
  tbody.innerHTML='';
  transactions.filter(t=>t.user===currentUser?.username).forEach(t=>{
    const tr=document.createElement('tr');
    tr.innerHTML=`<td>${t.type}</td><td>${t.amount}</td><td>${t.plan}</td><td>${t.status}</td>`;
    tbody.appendChild(tr);
  });
}

// Initialize
if(currentUser){sideNav.style.display='block';}
else{sideNav.style.display='none';}
updateUI();
updatePayNumber();
</script>
</body>
</html>
