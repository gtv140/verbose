<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>VERBOSE</title>
<style>
:root{
  --neon:#00f7ff;
  --accent:#ff5cff;
  --dark:#070707;
  --panel:#0f1114;
}
*{box-sizing:border-box}
body {
    margin:0;
    font-family:Inter, Arial, sans-serif;
    background:var(--dark);
    color:#fff;
    overflow-x:hidden;
}
header {
    text-align:center;
    padding:24px 12px;
    font-size:28px;
    font-weight:800;
    letter-spacing:2px;
    background:linear-gradient(90deg,var(--neon),var(--accent));
    -webkit-background-clip:text;
    -webkit-text-fill-color:transparent;
}
.login-box, .page {
    max-width:430px;
    margin:18px auto;
    background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
    padding:18px;
    border-radius:12px;
    border:1px solid rgba(0,255,240,0.06);
    box-shadow:0 8px 30px rgba(0,0,0,0.6);
}
input,button,select {
    width:100%;
    padding:10px;
    margin-top:10px;
    border-radius:8px;
    border:1px solid rgba(0,255,240,0.08);
    background:transparent;
    color:#e6f7fb;
    outline:none;
    font-size:14px;
}
input::placeholder{color:rgba(230,247,251,0.5)}
button {
    background:linear-gradient(90deg,var(--neon),var(--accent));
    border:none;
    color:#001;
    font-weight:700;
    cursor:pointer;
    padding:11px;
    border-radius:10px;
    transition:transform .15s ease, box-shadow .15s;
}
button:hover{ transform:translateY(-2px); box-shadow:0 10px 30px rgba(0,0,0,0.5); }
.nav {
    position:fixed;
    bottom:0; left:0; right:0;
    background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
    display:flex;
    justify-content:space-around;
    padding:12px 6px;
    border-top:1px solid rgba(0,255,240,0.04);
    font-size:14px;
    gap:6px;
}
.nav div{text-align:center;cursor:pointer; width:64px;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px}
.hidden{display:none;}
.small {font-size:13px;color:rgba(230,247,251,0.8);}
.user-box {
    background:linear-gradient(90deg, rgba(0,255,240,0.06), rgba(255,92,255,0.02));
    padding:14px;
    border-radius:12px;
    margin-bottom:12px;
    font-weight:700;
    display:flex;
    justify-content:space-between;
    align-items:center;
    gap:10px;
    border:1px solid rgba(0,255,240,0.06);
}
.user-box .left {display:flex;flex-direction:column;gap:6px}
.user-box .right {text-align:right;font-size:13px}
.badge {background:rgba(0,255,240,0.06);padding:6px 10px;border-radius:999px;color:var(--neon);font-weight:800}
.alert-box {
    background:linear-gradient(90deg, rgba(255,0,136,0.04), rgba(0,0,0,0.02));
    padding:12px;
    border-radius:10px;
    margin-bottom:12px;
    color:var(--accent);
    border:1px solid rgba(255,0,136,0.06);
    box-shadow:0 6px 20px rgba(255,0,136,0.03) inset;
}
.plan-box {
    border:1px solid rgba(0,255,240,0.06);
    padding:12px;
    margin:10px 0;
    border-radius:10px;
    background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(0,0,0,0.04));
    transition:box-shadow .2s ease, transform .12s ease;
    display:flex;
    gap:12px;
    align-items:center;
}
.plan-box .meta {flex:1}
.plan-box .meta b{display:block;margin-bottom:6px;font-size:15px}
.plan-box .meta .small {margin-top:6px}
.plan-box .actions {width:120px;text-align:right}
.plan-box:hover{box-shadow:0 12px 30px rgba(0,255,240,0.06); transform:translateY(-4px)}
.countdown {font-weight:700;color:var(--neon)}
@media (max-width:480px){
  .login-box,.page{margin:12px;padding:14px}
  .nav div{width:48px}
  header{font-size:22px}
  .logout-btn{padding:10px 12px;font-size:14px}
}
</style>
</head>
<body>

<header>VERBOSE</header>

<div id="loginPage" class="login-box">
  <h2 style="margin:0 0 8px 0">Login / Signup</h2>
  <select id="userOption" aria-label="option">
    <option value="login">Login</option>
    <option value="signup">New User Signup</option>
  </select>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password" />
  <button onclick="login()">Submit</button>
  <p class="small" style="margin-top:10px">Tip: Use same device/browser to keep your account saved (local storage).</p>
</div>

<div id="dashboard" class="page hidden">
  <div class="alert-box">Warning: Only use official VERBOSE channels for deposits and communications.</div>

  <div class="user-box">
    <div class="left">
      <div style="display:flex;gap:12px;align-items:center">
        <div style="width:56px;height:56px;border-radius:12px;background:linear-gradient(90deg,var(--neon),var(--accent));display:flex;align-items:center;justify-content:center;color:#001;font-weight:900">V</div>
        <div>
          <div style="font-size:16px;font-weight:800" id="dashUser">—</div>
          <div class="small">Member since: <span id="dashSince">—</span></div>
        </div>
      </div>
    </div>
    <div class="right">
      <div style="font-size:13px">Balance</div>
      <div style="font-size:18px;font-weight:900">Rs <span id="dashBalance">0</span></div>
      <div style="margin-top:8px" class="badge">Daily: Rs <span id="dashDaily">0</span></div>
    </div>
  </div>

  <h2 style="text-align:center;color:var(--neon);margin:6px 0 10px 0">Dashboard Overview</h2>

  <div style="margin-top:8px; text-align:center">
    <button class="logout-btn" onclick="logout()" title="Logout">Logout</button>
  </div>
</div>

<div id="plans" class="page hidden">
  <h2>Plans</h2>
  <div id="plansList"></div>
</div>

<div id="deposit" class="page hidden">
  <h2>Deposit</h2>
  <label>Method</label>
  <select id="depositMethod" onchange="updateDepositNumber()">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
  </select>
  <div style="display:flex;gap:8px;align-items:center;margin-top:10px">
    <input id="depositNumber" readonly style="flex:1" />
    <button onclick="copyDepositNumber()" style="width:110px">Copy</button>
  </div>
  <label style="margin-top:12px">Amount</label>
  <input id="depositAmount" readonly />
  <label>Transaction ID</label>
  <input id="depositTxId" placeholder="TX ID" />
  <label>Upload Proof</label>
  <input type="file" id="depositProof" />
  <button onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="withdrawal" class="page hidden">
  <h2>Withdrawal</h2>
  <label>Method</label>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
    <option value="bank">Bank</option>
  </select>
  <input id="withdrawAccount" placeholder="Account Number" />
  <input id="withdrawAmount" placeholder="Amount" />
  <button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
</div>

<script>
// USERS & STORAGE
let currentUser = localStorage.getItem('verbose_user') || null;
let balance = parseFloat(localStorage.getItem('verbose_balance')) || 0;
let dailyProfit = parseFloat(localStorage.getItem('verbose_daily')) || 0;
let plansData = [];
let userPlans = JSON.parse(localStorage.getItem('verbose_userPlans')||'[]');

// CREATE PLANS
for(let i=1;i<=25;i++){
    const invest = Math.round( i<=7 ? 200 + (i-1)*(3000-200)/6 : 4000 + (i-8)*(30000-4000)/17 );
    const days = 20 + Math.floor((i-1)*(70-20)/24);
    const multiplier = i<=7 ? 3 : 2.5;
    const comingSoon = i > 20;
    plansData.push({
      id:i,
      name:`Plan ${i}`,
      invest:invest,
      days:days,
      total:Math.round(invest*multiplier),
      multiplier:multiplier,
      coming:comingSoon
    });
}

// AUTH
function login(){
    const option = document.getElementById('userOption').value;
    const u = document.getElementById('user').value.trim();
    const p = document.getElementById('pass').value.trim();
    if(!u||!p){ alert("Enter username & password"); return; }
    currentUser = u;
    localStorage.setItem('verbose_user', currentUser);
    if(option==='signup'){
        localStorage.setItem('verbose_balance','0');
        localStorage.setItem('verbose_daily','0');
        userPlans = [];
        localStorage.setItem('verbose_userPlans', JSON.stringify(userPlans));
    }
    balance = parseFloat(localStorage.getItem('verbose_balance')) || 0;
    dailyProfit = parseFloat(localStorage.getItem('verbose_daily')) || 0;
    document.getElementById('dashUser').innerText = currentUser;
    document.getElementById('dashBalance').innerText = balance;
    document.getElementById('dashDaily').innerText = dailyProfit;
    document.getElementById('dashSince').innerText = new Date().toLocaleDateString();
    document.getElementById('loginPage').classList.add('hidden');
    document.getElementById('dashboard').classList.remove('hidden');
    document.getElementById('bottomNav').classList.remove('hidden');
    updateDepositNumber();
    renderPlans();
}

// LOGOUT
function logout(){
    currentUser = null;
    localStorage.removeItem('verbose_user');
    document.getElementById('loginPage').classList.remove('hidden');
    document.getElementById('dashboard').classList.add('hidden');
    document.getElementById('bottomNav').classList.add('hidden');
}

// NAVIGATION
function showPage(pageId){
    const pages = document.querySelectorAll('.page');
    pages.forEach(p=>p.classList.add('hidden'));
    document.getElementById(pageId).classList.remove('hidden');
}

// DEPOSIT
function updateDepositNumber(){
    const method = document.getElementById('depositMethod').value;
    const number = method==='jazzcash' ? '03705519562' : '03379827882';
    document.getElementById('depositNumber').value = number;
    const selectedPlan = plansData[0]; // first plan as default
    document.getElementById('depositAmount').value = selectedPlan.invest;
}
function copyDepositNumber(){
    const dep = document.getElementById('depositNumber');
    dep.select();
    dep.setSelectionRange(0,99999);
    navigator.clipboard.writeText(dep.value);
    alert("Deposit number copied!");
}
function submitDeposit(){
    const txid = document.getElementById('depositTxId').value.trim();
    if(!txid){ alert("Enter TX ID"); return; }
    // Auto update balance
    const amount = parseFloat(document.getElementById('depositAmount').value);
    balance += amount;
    localStorage.setItem('verbose_balance', balance);
    document.getElementById('dashBalance').innerText = balance;
    alert("Deposit recorded. Admin will verify actual payment manually.");
}

// WITHDRAWAL
function submitWithdraw(){
    const amt = parseFloat(document.getElementById('withdrawAmount').value.trim());
    if(!amt){ alert("Enter amount"); return; }
    if(amt>balance){ alert("Insufficient balance"); return; }
    balance -= amt;
    localStorage.setItem('verbose_balance', balance);
    document.getElementById('dashBalance').innerText = balance;
    alert("Withdrawal recorded. Admin will process actual payment manually.");
}

// PLANS
function renderPlans(){
    const container = document.getElementById('plansList');
    container.innerHTML='';
    plansData.forEach(plan=>{
        const div = document.createElement('div');
        div.className='plan-box';
        div.innerHTML=`
            <div class="meta">
                <b>${plan.name}</b>
                <div>Invest: Rs ${plan.invest}</div>
                <div>Duration: ${plan.days} days</div>
                <div>Total Return: Rs ${plan.total}</div>
            </div>
            <div class="actions">
                <button onclick="buyPlan(${plan.id})" ${plan.coming ? 'disabled':''}>Buy</button>
            </div>
        `;
        container.appendChild(div);
    });
}
function buyPlan(id){
    const plan = plansData.find(p=>p.id===id);
    if(!plan) return;
    const start = Date.now();
    const end = start + plan.days*24*60*60*1000;
    userPlans.push({id:id,start:start,end:end});
    localStorage.setItem('verbose_userPlans', JSON.stringify(userPlans));
    balance += plan.invest; // auto add for display
    localStorage.setItem('verbose_balance', balance);
    document.getElementById('dashBalance').innerText = balance;
    alert(`Plan ${plan.name} bought! Balance updated for display only.`);
}

// DAILY PROFIT AUTO UPDATE
function addDailyProfit(){
    const profit = Math.round(balance*0.01); // 1% daily
    dailyProfit += profit;
    balance += profit;
    localStorage.setItem('verbose_daily', dailyProfit);
    localStorage.setItem('verbose_balance', balance);
    document.getElementById('dashDaily').innerText = dailyProfit;
    document.getElementById('dashBalance').innerText = balance;
}
setInterval(addDailyProfit, 24*60*60*1000); // daily update

if(currentUser){
    login();
}
</script>

</body>
</html>
