<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>VERBOSE Dashboard</title>
<style>
:root{--neon:#00f7ff;--accent:#ff5cff;--dark:#070707;}
*{box-sizing:border-box;}
body{margin:0;font-family:Inter, Arial,sans-serif;background:var(--dark);color:#fff;overflow-x:hidden;}
header{text-align:center;padding:24px 12px;font-size:28px;font-weight:800;letter-spacing:2px;background:linear-gradient(90deg,var(--neon),var(--accent));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.login-box,.page{max-width:430px;margin:18px auto;background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0.01));padding:18px;border-radius:12px;border:1px solid rgba(0,255,240,0.06);box-shadow:0 8px 30px rgba(0,0,0,0.6);}
input,button,select{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid rgba(0,255,240,0.08);background:transparent;color:#e6f7fb;outline:none;font-size:14px;}
input::placeholder{color:rgba(230,247,251,0.5);}
button{background:linear-gradient(90deg,var(--neon),var(--accent));border:none;color:#001;font-weight:700;cursor:pointer;padding:11px;border-radius:10px;transition:transform .15s ease,box-shadow .15s;}
button:hover{transform:translateY(-2px);box-shadow:0 10px 30px rgba(0,0,0,0.5);}
.nav{position:fixed;bottom:0;left:0;right:0;background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0.01));display:flex;justify-content:space-around;padding:12px 6px;border-top:1px solid rgba(0,255,240,0.04);font-size:14px;gap:6px;}
.nav div{text-align:center;cursor:pointer;width:64px;}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px;}
.hidden{display:none;}
.small{font-size:13px;color:rgba(230,247,251,0.8);}
.user-box{background:linear-gradient(90deg,rgba(0,255,240,0.06),rgba(255,92,255,0.02));padding:14px;border-radius:12px;margin-bottom:12px;font-weight:700;display:flex;justify-content:space-between;align-items:center;gap:10px;border:1px solid rgba(0,255,240,0.06);}
.user-box .left{display:flex;flex-direction:column;gap:6px;}
.user-box .right{text-align:right;font-size:13px;}
.badge{background:rgba(0,255,240,0.06);padding:6px 10px;border-radius:999px;color:var(--neon);font-weight:800;}
.alert-box{background:linear-gradient(90deg, rgba(255,0,136,0.04), rgba(0,0,0,0.02));padding:12px;border-radius:10px;margin-bottom:12px;color:var(--accent);border:1px solid rgba(255,0,136,0.06);box-shadow:0 6px 20px rgba(255,0,136,0.03) inset;}
.plan-box{border:1px solid rgba(0,255,240,0.06);padding:12px;margin:10px 0;border-radius:10px;background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(0,0,0,0.04));transition:box-shadow .2s ease, transform .12s ease;display:flex;gap:12px;align-items:center;}
.plan-box .meta{flex:1;}
.plan-box .meta b{display:block;margin-bottom:6px;font-size:15px;}
.plan-box .meta .small{margin-top:6px;}
.plan-box .actions{width:140px;text-align:right;}
.plan-box:hover{box-shadow:0 12px 30px rgba(0,255,240,0.06);transform:translateY(-4px);}
.referral-box{background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(0,0,0,0.03));padding:12px;border-radius:10px;margin:10px 0;border:1px solid rgba(0,255,240,0.04);}
.referral-box input{background:transparent;border:1px dashed rgba(255,255,255,0.03);padding:8px;border-radius:8px;}
.countdown{font-weight:700;color:var(--neon);}
@media(max-width:480px){.login-box,.page{margin:12px;padding:14px}.nav div{width:48px}header{font-size:22px}.logout-btn{padding:10px 12px;font-size:14px}}
</style>
</head>
<body>
<header>VERBOSE</header>

<!-- LOGIN -->
<div id="loginPage" class="login-box">
  <h2>Login / Signup</h2>
  <select id="userOption"><option value="login">Login</option><option value="signup">New User Signup</option></select>
  <input id="user" placeholder="Username" />
  <input id="pass" placeholder="Password" type="password"/>
  <button onclick="login()">Submit</button>
  <p class="small">Tip: Use same device/browser to keep your account saved (local storage).</p>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
  <div class="alert-box">Warning: Only use official VERBOSE channels for deposits. Never share passwords.</div>
  <div class="user-box">
    <div class="left">
      <div style="display:flex;gap:12px;align-items:center">
        <div style="width:56px;height:56px;border-radius:12px;background:linear-gradient(90deg,var(--neon),var(--accent));display:flex;align-items:center;justify-content:center;color:#001;font-weight:900">V</div>
        <div>
          <div id="dashUser" style="font-size:16px;font-weight:800">—</div>
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
  <div class="referral-box">
    <div style="display:flex;gap:8px;align-items:center">
      <input id="refLink" readonly style="flex:1" />
      <button onclick="copyReferral()">Copy Link</button>
    </div>
    <div class="small">Share this link to invite friends. Bonuses apply automatically.</div>
  </div>
  <button class="logout-btn" onclick="logout()">Logout</button>
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
  <p class="small" style="margin-top:8px">After submitting, share proof with admin on WhatsApp or Email.</p>
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
  <input id="withdrawAccount" placeholder="Account Number" />
  <input id="withdrawAmount" placeholder="Amount" />
  <button onclick="submitWithdraw()">Request Withdrawal</button>
  <p class="small" style="margin-top:8px">Requests are reviewed by admin.</p>
</div>

<!-- HISTORY -->
<div id="history" class="page hidden">
  <h2>History</h2>
  <div id="historyList"></div>
</div>

<!-- SUPPORT -->
<div id="support" class="page hidden">
  <div class="support-box">
    <h2>Contact Admin</h2>
    <div class="support-grid">
      <div class="support-item">
        <div class="icon">💬</div>
        <div>
          <p><strong>WhatsApp Support</strong></p>
          <p class="small">Fastest support via group or admin message.</p>
          <p style="margin-top:6px"><a href="https://chat.whatsapp.com/Kmaiv3VdSo09rio4qcRTRM" target="_blank">Join WhatsApp Group</a></p>
        </div>
      </div>
      <div class="support-item">
        <div class="icon">📧</div>
        <div>
          <p><strong>Email Support</strong></p>
          <p class="small">Send proofs or queries via email.</p>
          <p style="margin-top:6px"><a href="mailto:rock.earn92@gmail.com">rock.earn92@gmail.com</a></p>
        </div>
      </div>
    </div>
    <p class="small" style="text-align:center;margin-top:10px;color:var(--neon);font-weight:800;">VERBOSE — Secure. Transparent. Professional.</p>
  </div>
</div>

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div onclick="showPage('history')"><span class="ico">📜</span>History</div>
  <div onclick="showPage('support')"><span class="ico">📞</span>Support</div>
</div>

<script>
// ===== STORAGE =====
let currentUser = localStorage.getItem('verbose_user')||null;
let balance = parseFloat(localStorage.getItem('verbose_balance')||'0');
let dailyProfit = parseFloat(localStorage.getItem('verbose_daily')||'0');
let userPlans = JSON.parse(localStorage.getItem('verbose_userPlans')||'[]');
let referralCode = localStorage.getItem('verbose_referral')||'';
let history = JSON.parse(localStorage.getItem('verbose_history')||'[]');

// ===== PLANS DATA =====
let plansData=[];
for(let i=1;i<=30;i++){
  let invest;
  let days = 25 + Math.floor((i-1)/5);
  let multiplier = i<=7 ? 2.8 : 2.3;
  if(i<=7){ invest = 200 + (i-1)*400 } // special offers 200-3000
  else{ invest = 4000 + (i-8)*1000 } // remaining plans up to 30k
  let planName = i<=7 ? `Special Plan ${i}` : `Plan ${i}`;
  plansData.push({id:i,name:planName,invest,days,total:Math.round(invest*multiplier),special:i<=7});
}

// ===== UI =====
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const option=document.getElementById('userOption').value;
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u||!p){alert("Enter username & password");return;}
  currentUser=u;
  localStorage.setItem('verbose_user',currentUser);
  referralCode=referralCode||Math.random().toString(36).substring(2,10);
  localStorage.setItem('verbose_referral',referralCode);
  if(option==='signup'){balance=0;dailyProfit=0;userPlans=[];localStorage.setItem('verbose_balance','0');localStorage.setItem('verbose_daily','0');localStorage.setItem('verbose_userPlans',JSON.stringify(userPlans));}
  document.getElementById('withdrawUsername').value=currentUser;
  updateDashboard();
}

// ===== DASHBOARD =====
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('dashSince').innerText=new Date().toLocaleDateString();
  document.getElementById('refLink').value=`https://gtv140.github.io/verbose/?ref=${referralCode}`;
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('dashboard').classList.remove('hidden');
  document.getElementById('bottomNav').classList.remove('hidden');
  renderPlans();
  renderHistory();
}
function logout(){currentUser=null;localStorage.removeItem('verbose_user');showPage('loginPage');document.getElementById('user').value='';document.getElementById('pass').value='';}
function copyReferral(){navigator.clipboard.writeText(document.getElementById('refLink').value); alert("Referral link copied!");}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert("Deposit number copied!");}

// ===== PLANS =====
function renderPlans(){
  const list=document.getElementById('plansList');list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div');div.className='plan-box';
    let countdownHTML = '';
    if(p.special){
      countdownHTML = `<div class="small countdown" id="countdown${p.id}"></div>`;
      startCountdown(p.id);
    }
    div.innerHTML=`<div class="meta"><b>${p.name}</b><div class="small">${p.special?'Special Offer':'Standard Plan'}</div>${countdownHTML}<div class="small" style="margin-top:8px">Invest: Rs ${p.invest} · Days: ${p.days}</div><div class="small">Total Profit: Rs ${p.total}</div></div><div class="actions"><button onclick="buyPlan(${p.id})">Buy Now</button></div>`;
    list.appendChild(div);
  });
}

// ===== COUNTDOWN =====
function startCountdown(id){
  const endTime = new Date().getTime() + 24*60*60*1000;
  const interval = setInterval(()=>{
    const now = new Date().getTime();
    const distance = endTime - now;
    if(distance<0){ document.getElementById(`countdown${id}`).innerText='Offer Expired'; clearInterval(interval); return;}
    const h = Math.floor(distance/(1000*60*60));
    const m = Math.floor((distance%(1000*60*60))/(1000*60));
    const s = Math.floor((distance%(1000*60))/1000);
    document.getElementById(`countdown${id}`).innerText=`${h}h ${m}m ${s}s left`;
  },1000);
}

// ===== BUY PLAN =====
function buyPlan(id){
  const plan = plansData.find(p=>p.id===id);
  if(!plan) return alert("Plan not found!");
  showPage('deposit');
  document.getElementById('depositAmount').value = plan.invest;
  document.getElementById('depositMethod').value='jazzcash';
  updateDepositNumber();
  alert(`You selected ${plan.name}. Deposit Rs ${plan.invest} to buy this plan.`);
}

// ===== DEPOSIT =====
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value = method==='jazzcash' ? '03705519562' : '03379827882';
}

// ===== SUBMIT DEPOSIT =====
function submitDeposit(){
  const method = document.getElementById('depositMethod').value;
  const amount = parseFloat(document.getElementById('depositAmount').value);
  const txId = document.getElementById('depositTxId').value.trim();
  const proof = document.getElementById('depositProof').files[0];

  if(!amount || !txId || !proof){ alert("Enter all deposit details and upload proof."); return; }

  // Update balance & daily profit
  balance += amount;
  dailyProfit += Math.round(amount * 0.05); // Example daily profit 5%
  localStorage.setItem('verbose_balance', balance);
  localStorage.setItem('verbose_daily', dailyProfit);

  // Add to history
  history.push({
    type:'Deposit',
    method:method,
    amount:amount,
    txId:txId,
    date:new Date().toLocaleString()
  });
  localStorage.setItem('verbose_history', JSON.stringify(history));

  alert(`Deposit of Rs ${amount} successful!`);
  updateDashboard();
  showPage('dashboard');
}

// ===== WITHDRAW =====
function submitWithdraw(){
  const method = document.getElementById('withdrawMethod').value;
  const account = document.getElementById('withdrawAccount').value.trim();
  const amount = parseFloat(document.getElementById('withdrawAmount').value);

  if(!amount || !account){ alert("Enter account number and amount"); return; }
  if(amount > balance){ alert("Insufficient balance"); return; }

  balance -= amount;
  localStorage.setItem('verbose_balance', balance);

  history.push({
    type:'Withdrawal',
    method:method,
    account:account,
    amount:amount,
    date:new Date().toLocaleString()
  });
  localStorage.setItem('verbose_history', JSON.stringify(history));

  alert(`Withdrawal request of Rs ${amount} submitted!`);
  updateDashboard();
}

// ===== HISTORY =====
function renderHistory(){
  const list = document.getElementById('historyList');
  list.innerHTML='';
  if(history.length===0){ list.innerHTML='<p class="small">No history yet.</p>'; return; }
  history.slice().reverse().forEach(h=>{
    const div = document.createElement('div');
    div.className='plan-box';
    div.innerHTML=`<div class="meta"><b>${h.type}</b><div class="small">${h.method||h.account||''}</div><div class="small">Rs ${h.amount}</div><div class="small">${h.date}</div></div>`;
    list.appendChild(div);
  });
}

// ===== INITIAL LOAD =====
if(currentUser){ updateDashboard(); }
</script>
</body>
</html>
