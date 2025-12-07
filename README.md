<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
:root{
  --neon:#00f7ff;
  --accent:#ff5cff;
  --dark:#070707;
  --panel:#0f1114;
}
*{box-sizing:border-box}
body{
  margin:0;
  font-family:Inter, Arial, sans-serif;
  background:var(--dark);
  color:#fff;
  overflow-x:hidden;
  position:relative;
  -webkit-font-smoothing:antialiased;
  -moz-osx-font-smoothing:grayscale;
}
header{
  text-align:center;
  padding:24px 12px;
  font-size:28px;
  font-weight:800;
  letter-spacing:2px;
  background:linear-gradient(90deg,var(--neon),var(--accent));
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent;
}
.login-box, .page{
  max-width:430px;
  margin:18px auto;
  background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
  padding:18px;
  border-radius:12px;
  border:1px solid rgba(0,255,240,0.06);
  box-shadow:0 8px 30px rgba(0,0,0,0.6);
}
input,button,select{
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
button{
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
.nav{
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
.user-box{
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
.alert-box{
  background:linear-gradient(90deg, rgba(255,0,136,0.04), rgba(0,0,0,0.02));
  padding:12px;
  border-radius:10px;
  margin-bottom:12px;
  color:var(--accent);
  border:1px solid rgba(255,0,136,0.06);
  box-shadow:0 6px 20px rgba(255,0,136,0.03) inset;
}
.plan-box{
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
.offer{color:var(--neon);font-weight:800;}
.referral-box{background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(0,0,0,0.03));padding:12px;border-radius:10px;margin:10px 0;border:1px solid rgba(0,255,240,0.04)}
.referral-box input{background:transparent;border:1px dashed rgba(255,255,255,0.03);padding:8px;border-radius:8px}
.support-box{
  background:linear-gradient(90deg, rgba(0,0,0,0.35), rgba(0,0,0,0.45));
  padding:14px;
  border-radius:12px;
  border:1px solid rgba(255,0,136,0.06);
  box-shadow:0 10px 30px rgba(255,0,136,0.03) inset;
}
.support-box h2{margin:0 0 8px 0;font-size:18px;background:linear-gradient(90deg,var(--neon),var(--accent));-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.support-grid{display:flex;gap:12px;flex-direction:column}
.support-item{display:flex;gap:10px;align-items:center}
.support-item .icon{width:44px;height:44px;border-radius:10px;display:flex;align-items:center;justify-content:center;background:rgba(0,255,240,0.04);color:var(--neon);font-weight:800}
.support-item p{margin:0;font-size:14px}
.support-note{font-size:13px;color:rgba(230,247,251,0.75);margin-top:8px}
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
  <select id="userOption">
    <option value="login">Login</option>
    <option value="signup">New User Signup</option>
  </select>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password" />
  <button onclick="login()">Submit</button>
  <p class="small" style="margin-top:10px">Tip: Use same device/browser to keep your account saved (local storage).</p>
</div>

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
    </div>
  </div>

  <h2 style="text-align:center;color:var(--neon);margin:6px 0 10px 0">Dashboard Overview</h2>
  <p class="small" style="text-align:center;margin-top:-6px">Track your plans, deposits and withdrawals securely. Our support team is available 24/7.</p>

  <div class="referral-box">
    <div style="display:flex;gap:8px;align-items:center">
      <input id="refLink" readonly style="flex:1" />
      <button onclick="copyReferral()" style="width:110px">Copy Link</button>
    </div>
    <div class="small" style="margin-top:8px">Share this link to invite friends. When they deposit, you get bonus credits automatically.</div>
  </div>

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
  <p class="small" style="margin-top:8px">After submitting, share proof with admin on WhatsApp or Email for verification.</p>
</div>

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
    <p class="support-note">Important: Always use the official support channels above to verify deposits/withdrawals. Avoid sharing sensitive information publicly.</p>
    <p style="color:var(--neon);font-weight:800;text-align:center;margin-top:10px">VERBOSE — Secure. Transparent. Professional.</p>
  </div>
</div>

<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div onclick="showPage('support')"><span class="ico">📞</span>Support</div>
</div>

<script>
let currentUser = localStorage.getItem('verbose_user') || null;
let balance = parseFloat(localStorage.getItem('verbose_balance')) || 0;
let dailyProfit = parseFloat(localStorage.getItem('verbose_daily')) || 0;
let plansData = [];
let userPlans = JSON.parse(localStorage.getItem('verbose_userPlans')||'[]');
let referralCode = localStorage.getItem('verbose_referral') || '';

for(let i=1;i<=30;i++){
    const invest = Math.round(200 + (i-1)*(30000-200)/29);
    const days = 20 + Math.floor((i-1)*(70-20)/29);
    const multiplier = i<=7 ? 3 : 2.5;
    const comingSoon = i > 25;
    plansData.push({
      id:i,
      name:`Plan ${i}`,
      invest:invest,
      days:days,
      total:Math.round(invest*multiplier),
      multiplier:multiplier,
      offer:i<=7,
      coming:comingSoon
    });
}

function login(){
    const option = document.getElementById('userOption').value;
    const u = document.getElementById('user').value.trim();
    const p = document.getElementById('pass').value.trim();
    if(!u||!p){ alert("Enter username & password"); return; }
    currentUser = u;
    localStorage.setItem('verbose_user', currentUser);
    referralCode = referralCode || Math.random().toString(36).substring(2,10);
    localStorage.setItem('verbose_referral', referralCode);
    if(option === 'signup'){
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
    document.getElementById('refLink').value = `https://gtv140.github.io/verbose/?ref=${referralCode}`;
    document.getElementById('loginPage').classList.add('hidden');
    document.getElementById('dashboard').classList.remove('hidden');
    document.getElementById('bottomNav').classList.remove('hidden');
    renderPlans();
}

function logout(){
    currentUser = null;
    localStorage.removeItem('verbose_user');
    document.getElementById('loginPage').classList.remove('hidden');
    document.getElementById('dashboard').classList.add('hidden');
    document.getElementById('bottomNav').classList.add('hidden');
    document.getElementById('user').value='';
    document.getElementById('pass').value='';
}

function showPage(id){
    document.querySelectorAll('.page').forEach(p => p.classList.add('hidden'));
    document.getElementById(id).classList.remove('hidden');
}

function copyDepositNumber(){
    const num = document.getElementById('depositNumber');
    if(!num) return;
    num.select();
    num.setSelectionRange(0,99999);
    document.execCommand('copy');
    alert("Deposit number copied!");
}
function copyReferral(){
    const ref = document.getElementById('refLink');
    ref.select();
    ref.setSelectionRange(0,99999);
    document.execCommand('copy');
    alert("Referral link copied!");
}

function updateDepositNumber(){
    const method = document.getElementById('depositMethod').value;
    const number = method==='jazzcash' ? '03001234567' : '03101234567';
    document.getElementById('depositNumber').value = number;
    document.getElementById('depositAmount').value = 'Rs 5000';
}

function submitDeposit(){
    const tx = document.getElementById('depositTxId').value.trim();
    const proof = document.getElementById('depositProof').files[0];
    if(!tx || !proof){ alert("Enter TX ID and upload proof!"); return; }
    alert("Deposit submitted! Admin will verify shortly.");
    document.getElementById('depositTxId').value='';
    document.getElementById('depositProof').value='';
}

function submitWithdraw(){
    const amt = parseFloat(document.getElementById('withdrawAmount').value);
    const acc = document.getElementById('withdrawAccount').value.trim();
    if(!amt || !acc){ alert("Enter amount and account number!"); return; }
    if(amt>balance){ alert("Insufficient balance!"); return; }
    balance -= amt;
    localStorage.setItem('verbose_balance', balance);
    document.getElementById('dashBalance').innerText = balance;
    alert("Withdrawal request submitted! Admin will process it.");
    document.getElementById('withdrawAmount').value='';
    document.getElementById('withdrawAccount').value='';
}

function renderPlans(){
    const container = document.getElementById('plansList');
    container.innerHTML='';
    plansData.forEach(p=>{
        const div = document.createElement('div');
        div.className='plan-box';
        div.innerHTML=`
        <div class="meta">
          <b>${p.name} ${p.offer?'<span class="offer">HOT</span>':''}</b>
          <div>Invest: Rs ${p.invest}</div>
          <div>Days: ${p.days}</div>
          <div>Total Return: Rs ${p.total}</div>
          ${p.coming?'<div class="small">Coming Soon</div>':''}
        </div>
        <div class="actions">
          ${!p.coming?'<button onclick="buyPlan('+p.id+')">Buy</button>':'<span class="small">Wait</span>'}
        </div>`;
        container.appendChild(div);
    });
}

function buyPlan(id){
    const plan = plansData.find(p=>p.id===id);
    if(balance<plan.invest){ alert("Insufficient balance!"); return; }
    balance -= plan.invest;
    dailyProfit += Math.round((plan.total-plan.invest)/plan.days);
    userPlans.push(plan);
    localStorage.setItem('verbose_balance', balance);
    localStorage.setItem('verbose_daily', dailyProfit);
    localStorage.setItem('verbose_userPlans', JSON.stringify(userPlans));
    document.getElementById('dashBalance').innerText = balance;
    document.getElementById('dashDaily').innerText = dailyProfit;
    alert(`Plan ${plan.name} purchased!`);
}

if(currentUser){ login(); }
</script>
</body>
</html>
