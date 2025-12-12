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
}
*{box-sizing:border-box}
body{
    margin:0;
    font-family:Inter, Arial, sans-serif;
    background:var(--dark);
    color:#fff;
    overflow-x:hidden;
}
header{text-align:center;padding:24px 12px;font-size:28px;font-weight:800;letter-spacing:2px;background:linear-gradient(90deg,var(--neon),var(--accent));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.login-box,.page{
max-width:430px;margin:18px auto;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
padding:18px;border-radius:12px;border:1px solid rgba(0,255,240,0.06);box-shadow:0 8px 30px rgba(0,0,0,0.6);}
input,button,select{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid rgba(0,255,240,0.08);background:transparent;color:#e6f7fb;outline:none;font-size:14px;}
input::placeholder{color:rgba(230,247,251,0.5)}
button{background:linear-gradient(90deg,var(--neon),var(--accent));border:none;color:#001;font-weight:700;cursor:pointer;padding:11px;border-radius:10px;transition:transform .15s ease, box-shadow .15s;}
button:hover{ transform:translateY(-2px); box-shadow:0 10px 30px rgba(0,0,0,0.5); }
.nav{position:fixed;bottom:0;left:0;right:0;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));display:flex;justify-content:space-around;padding:12px 6px;border-top:1px solid rgba(0,255,240,0.04);font-size:14px;gap:6px;}
.nav div{text-align:center;cursor:pointer; width:64px;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px}
.hidden{display:none;}
.small{font-size:13px;color:rgba(230,247,251,0.8);}
.user-box{background:linear-gradient(90deg, rgba(0,255,240,0.06), rgba(255,92,255,0.02));padding:14px;border-radius:12px;margin-bottom:12px;font-weight:700;display:flex;justify-content:space-between;align-items:center;gap:10px;border:1px solid rgba(0,255,240,0.06);position:relative;}
.user-box .left{display:flex;flex-direction:column;gap:6px}
.user-box .right{text-align:right;font-size:13px}
.badge{background:rgba(0,255,240,0.06);padding:6px 10px;border-radius:999px;color:var(--neon);font-weight:800}
.user-box .icons{display:flex;gap:10px;position:absolute;top:14px;right:14px}
.user-box .icons div{width:34px;height:34px;background:rgba(0,255,240,0.06);border-radius:8px;display:flex;align-items:center;justify-content:center;color:var(--neon);font-weight:800;cursor:pointer;font-size:16px}
.alert-box{background:linear-gradient(90deg, rgba(255,0,136,0.04), rgba(0,0,0,0.02));padding:12px;border-radius:10px;margin-bottom:12px;color:var(--accent);border:1px solid rgba(255,0,136,0.06);box-shadow:0 6px 20px rgba(255,0,136,0.03) inset;}
.plan-box{border:1px solid rgba(0,255,240,0.06);padding:12px;margin:10px 0;border-radius:10px;background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(0,0,0,0.04));transition:box-shadow .2s ease, transform .12s ease;display:flex;gap:12px;align-items:center;}
.plan-box .meta{flex:1}
.plan-box .meta b{display:block;margin-bottom:6px;font-size:15px}
.plan-box .meta .small{margin-top:6px}
.plan-box .actions{width:120px;text-align:right}
.plan-box:hover{box-shadow:0 12px 30px rgba(0,255,240,0.06); transform:translateY(-4px)}
.offer{color:var(--neon);font-weight:800;}
.referral-box{background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(0,0,0,0.03));padding:12px;border-radius:10px;margin:10px 0;border:1px solid rgba(0,255,240,0.04)}
.referral-box input{background:transparent;border:1px dashed rgba(255,255,255,0.03);padding:8px;border-radius:8px}
.support-box{background:linear-gradient(90deg, rgba(0,0,0,0.35), rgba(0,0,0,0.45));padding:14px;border-radius:12px;border:1px solid rgba(255,0,136,0.06);box-shadow:0 10px 30px rgba(255,0,136,0.03) inset;}
.support-box h2{margin:0 0 8px 0;font-size:18px;background:linear-gradient(90deg,var(--neon),var(--accent));-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.support-grid{display:flex;gap:12px;flex-direction:column}
.support-item{display:flex;gap:10px;align-items:center}
.support-item .icon{width:44px;height:44px;border-radius:10px;display:flex;align-items:center;justify-content:center;background:rgba(0,255,240,0.04);color:var(--neon);font-weight:800}
.support-item p{margin:0;font-size:14px}
.support-note{font-size:13px;color:rgba(230,247,251,0.75);margin-top:8px}
.countdown{font-weight:700;color:var(--neon)}
.history-box{background:linear-gradient(180deg, rgba(0,255,240,0.04), rgba(0,0,0,0.05));padding:12px;border-radius:10px;margin:10px 0;border:1px solid rgba(0,255,240,0.06);}
.history-box p{margin:4px 0;font-size:13px;color:#00f7ff;}
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

<!-- LOGIN / SIGNUP -->
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

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="alert-box">Warning: Only use official VERBOSE channels for deposits and communications. Never share sensitive passwords.</div>
  
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
      <div style="margin-top:4px" class="badge">Active Users: <span id="activeUsers">1</span></div>
    </div>
    <div class="icons">
      <div title="Active Users">🟢</div>
      <div title="History">📜</div>
    </div>
  </div>

  <h2 style="text-align:center;color:var(--neon);margin:6px 0 10px 0">Dashboard Overview</h2>
  <p class="small" style="text-align:center;margin-top:-6px">Monitor plans, deposits & withdrawals securely. Neon interface for professional look.</p>

  <div class="referral-box">
    <div style="display:flex;gap:8px;align-items:center">
      <input id="refLink" readonly style="flex:1" />
      <button onclick="copyReferral()" style="width:110px">Copy Link</button>
    </div>
    <div class="small" style="margin-top:8px; color:var(--neon)">Share this link to invite friends. Bonus credits will reflect automatically.</div>
  </div>

  <div id="historyContainer" class="hidden history-box"></div>

  <div style="margin-top:8px; text-align:center">
    <button class="logout-btn" onclick="logout()" title="Logout">Logout</button>
  </div>
</div>

<!-- PLANS -->
<div id="plans" class="page hidden">
  <h2>Plans</h2>
  <div id="plansList"></div>
</div>

<!-- DEPOSIT -->
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
  <p class="small" style="margin-top:8px">After submitting, share proof with admin on WhatsApp or Email for verification.</p>
</div>

<!-- WITHDRAWAL -->
<div id="withdrawal" class="page hidden">
  <h2>Withdrawal</h2>
  <label>Method</label>
  <select id="withdrawMethod">
    <option value="jazzcash">JazzCash</option>
    <option value="easypaisa">EasyPaisa</option>
    <option value="bank">Bank</option>
  </select>
  <input id="withdrawUsername" readonly placeholder="Username" />
  <input id="withdrawAccount" placeholder="Account Number (manual)" />
  <input id="withdrawAmount" placeholder="Amount" />
  <button onclick="submitWithdraw()">Request Withdrawal</button>
  <p class="small" style="margin-top:8px">Requests are reviewed by admin. Keep your proof ready.</p>
</div>

<!-- SUPPORT -->
<div id="support" class="page hidden">
  <div class="support-box">
    <h2>Contact Administration</h2>
    <div class="support-grid">
      <div class="support-item">
        <div class="icon">💬</div>
        <div>
          <p><strong>WhatsApp Support</strong></p>
          <p class="small">Fastest support — join our group or message the admin directly.</p>
          <p style="margin-top:6px"><a href="https://chat.whatsapp.com/Kmaiv3VdSo09rio4qcRTRM" target="_blank">Join WhatsApp Group</a></p>
        </div>
      </div>
      <div class="support-item">
        <div class="icon">📧</div>
        <div>
          <p><strong>Email Support</strong></p>
          <p class="small">For formal queries and proofs, email our support team.</p>
          <p style="margin-top:6px"><a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></p>
        </div>
      </div>
    </div>
    <p class="support-note">Always use official channels for verification. Never share sensitive info publicly.</p>
    <p style="color:var(--neon);font-weight:800;text-align:center;margin-top:10px">VERBOSE — Secure. Transparent. Professional.</p>
  </div>
</div>

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div onclick="showPage('support')"><span class="ico">📞</span>Support</div>
</div>

<script>
// === USERS & LOCAL STORAGE ===
let currentUser = localStorage.getItem('verbose_user') || null;
let balance = parseFloat(localStorage.getItem('verbose_balance')) || 0;
let dailyProfit = parseFloat(localStorage.getItem('verbose_daily')) || 0;
let plansData = [];
let userPlans = JSON.parse(localStorage.getItem('verbose_userPlans')||'[]');
let referralCode = localStorage.getItem('verbose_referral') || '';
let activeUsers = parseInt(localStorage.getItem('verbose_activeUsers')||'1');

// === ACTIVE USERS SIMULATION ===
function updateActiveUsers(){ 
  activeUsers = 1 + Math.floor(Math.random()*10);
  document.getElementById('activeUsers').innerText=activeUsers;
  localStorage.setItem('verbose_activeUsers',activeUsers);
}
setInterval(updateActiveUsers, 10000);

// === CREATE PLANS ===
for(let i=1;i<=25;i++){
    let invest,multiplier,coming=false;
    if(i<=5){ invest=Math.round(200 + (i-1)*(1800-200)/4); multiplier=2.6; }
    else{ invest=Math.round(200 + (i-1)*(30000-200)/24); multiplier=2.2; }
    const days=25+Math.floor((i-1)*(78-25)/30);
    plansData.push({id:i,name:`Plan ${i}`,invest,days,total:Math.round(invest*multiplier),multiplier,offer:i<=5,coming});
}

// === AUTH FUNCTIONS ===
function login(){
    const option=document.getElementById('userOption').value;
    const u=document.getElementById('user').value.trim();
    const p=document.getElementById('pass').value.trim();
    if(!u||!p){ alert("Enter username & password"); return; }
    currentUser=u;
    localStorage.setItem('verbose_user',currentUser);
    referralCode=referralCode||Math.random().toString(36).substring(2,10);
    localStorage.setItem('verbose_referral',referralCode);
    if(option==='signup'){
        localStorage.setItem('verbose_balance','0');
        localStorage.setItem('verbose_daily','0');
        userPlans=[];
        localStorage.setItem('verbose_userPlans',JSON.stringify(userPlans));
    }
    balance=parseFloat(localStorage.getItem('verbose_balance'))||0;
    dailyProfit=parseFloat(localStorage.getItem('verbose_daily'))||0;
    document.getElementById('dashUser').innerText=currentUser;
    document.getElementById('dashBalance').innerText=balance;
    document.getElementById('dashDaily').innerText=dailyProfit;
    document.getElementById('dashSince').innerText=new Date().toLocaleDateString();
    document.getElementById('refLink').value=`https://gtv140.github.io/verbose/?ref=${referralCode}`;
    document.getElementById('loginPage').classList.add('hidden');
    document.getElementById('dashboard').classList.remove('hidden');
    document.getElementById('bottomNav').classList.remove('hidden');
    updateDepositNumber();
    renderPlans();
    renderHistory();
    document.getElementById('withdrawUsername').value=currentUser;
}

// === LOGOUT & PAGE SWITCH ===
function logout(){
    localStorage.removeItem('verbose_user');
    localStorage.removeItem('verbose_balance');
    localStorage.removeItem('verbose_daily');
    localStorage.removeItem('verbose_userPlans');
    currentUser=null;
    balance=0;
    dailyProfit=0;
    userPlans=[];
    document.getElementById('dashboard').classList.add('hidden');
    document.getElementById('loginPage').classList.remove('hidden');
    document.getElementById('bottomNav').classList.add('hidden');
}

function showPage(pageId){
    const pages=document.querySelectorAll('.page');
    pages.forEach(p=>p.classList.add('hidden'));
    document.getElementById(pageId).classList.remove('hidden');
}

// === REFERRAL COPY ===
function copyReferral(){
    const ref=document.getElementById('refLink');
    ref.select();
    navigator.clipboard.writeText(ref.value).then(()=>alert('Referral copied!'));
}

// === DEPOSIT LOGIC ===
function updateDepositNumber(){
    const method=document.getElementById('depositMethod').value;
    const number=method==='jazzcash'?'03705519562':'03379827882';
    document.getElementById('depositNumber').value=number;
    document.getElementById('depositAmount').value='1000';
}
function copyDepositNumber(){
    const dep=document.getElementById('depositNumber');
    dep.select();
    navigator.clipboard.writeText(dep.value).then(()=>alert('Deposit number copied!'));
}
function submitDeposit(){
    const tx=document.getElementById('depositTxId').value.trim();
    if(!tx){alert('Enter transaction ID'); return;}
    alert('Deposit submitted for verification!');
}

// === WITHDRAW LOGIC ===
function submitWithdraw(){
    const amt=document.getElementById('withdrawAmount').value.trim();
    const acc=document.getElementById('withdrawAccount').value.trim();
    if(!amt||!acc){ alert('Fill account & amount'); return;}
    alert(`Withdrawal of Rs ${amt} requested!`);
}

// === PLANS ===
function renderPlans(){
    const list=document.getElementById('plansList');
    list.innerHTML='';
    plansData.forEach(p=>{
        const div=document.createElement('div');
        div.className='plan-box';
        div.innerHTML=`<div class="meta">
            <b>${p.name}</b>
            <div class="small">Invest: Rs ${p.invest} | Total: Rs ${p.total} | Days: ${p.days}</div>
            ${p.offer?'<div class="offer">Special Offer!</div>':''}
        </div>
        <div class="actions">
            <button onclick="investPlan(${p.id})">Invest</button>
        </div>`;
        list.appendChild(div);
    });
}

function investPlan(id){
    const plan=plansData.find(p=>p.id===id);
    if(!plan) return;
    if(balance<plan.invest){alert('Insufficient balance'); return;}
    balance-=plan.invest;
    dailyProfit+=plan.total/plan.days;
    localStorage.setItem('verbose_balance',balance);
    localStorage.setItem('verbose_daily',dailyProfit);
    document.getElementById('dashBalance').innerText=balance;
    document.getElementById('dashDaily').innerText=dailyProfit;
    userPlans.push(plan);
    localStorage.setItem('verbose_userPlans',JSON.stringify(userPlans));
    renderHistory();
}

// === HISTORY ===
function renderHistory(){
    const container=document.getElementById('historyContainer');
    if(userPlans.length===0){ container.classList.add('hidden'); return;}
    container.classList.remove('hidden');
    container.innerHTML='';
    userPlans.forEach(p=>{
        const div=document.createElement('div');
        div.className='history-box';
        div.innerHTML=`<p><b>${p.name}</b></p>
        <p>Invested: Rs ${p.invest} | Total Return: Rs ${p.total} | Days: ${p.days}</p>`;
        container.appendChild(div);
    });
}

// === AUTO LOGIN IF LOCALSTORAGE ===
if(currentUser){
    login();
    document.getElementById('user').value=currentUser;
}
</script>
