<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
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
#popup{position:fixed;top:20%;left:50%;transform:translateX(-50%);background:linear-gradient(90deg,#00f7ff,#ff5cff);padding:20px;border-radius:12px;box-shadow:0 10px 30px rgba(0,0,0,0.5);color:#001;font-weight:800;z-index:9999;text-align:center;}
#popup button{margin-top:10px;padding:8px 12px;border:none;border-radius:8px;background:#001;color:#fff;font-weight:700;cursor:pointer;}
.help-box{background:linear-gradient(90deg,rgba(0,255,240,0.06),rgba(255,92,255,0.02));padding:10px;border-radius:10px;margin:10px 0;display:flex;justify-content:space-around;}
.help-box a{color:var(--neon);text-decoration:none;font-weight:700;}
.support-icon{display:flex;align-items:center;gap:6px;padding:10px 12px;margin-bottom:12px;border-radius:10px;background:linear-gradient(90deg, rgba(0,255,240,0.06), rgba(255,92,255,0.02));cursor:pointer;width:fit-content;font-weight:700;}
.support-icon .ico{font-size:18px;}
.support-icon:hover{box-shadow:0 6px 20px rgba(0,255,240,0.2);transform:translateY(-2px);transition:all 0.15s ease;}
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

  <div class="alert-box">Active Members: <span id="activeMembers">0</span></div>
  <div id="activePlansBox"></div>

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
  <input id="depositAmount" placeholder="Enter Amount" />
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
  <input id="withdrawAccount" placeholder="Account Number" />
  <input id="withdrawAmount" placeholder="Amount" />
  <button onclick="submitWithdraw()">Request Withdrawal</button>
  <p class="small" style="margin-top:8px">Requests are reviewed by admin.</p>
</div>

<!-- HISTORY -->
<div id="history" class="page hidden">
  <h2>History</h2>

  <!-- SUPPORT ICON -->
  <div class="support-icon" onclick="openSupport()">
    <span class="ico">🛠️</span>
    <div class="small">Support</div>
  </div>

  <div id="historyList"></div>
</div>

<!-- POPUP -->
<div id="popup" class="hidden">
  <span id="popupText"></span><br>
  <button onclick="closePopup()">Close</button>
</div>

<!-- NAVIGATION -->
<div id="bottomNav" class="nav hidden">
  <div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
  <div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
  <div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
  <div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
  <div onclick="showPage('history')"><span class="ico">📜</span>History</div>
</div>

<script>
// ===== STORAGE =====
let currentUser = localStorage.getItem('verbose_user')||null;
let balance = parseFloat(localStorage.getItem('verbose_balance')||'5000');
let dailyProfit = parseFloat(localStorage.getItem('verbose_daily')||'0');
let userPlans = JSON.parse(localStorage.getItem('verbose_userPlans')||'[]');
let referralCode = localStorage.getItem('verbose_referral')||'';
let savedEndTimes = JSON.parse(localStorage.getItem('verbose_offerEndTimes')||'{}');
let history = JSON.parse(localStorage.getItem('verbose_history')||'[]');
let totalUsers = parseInt(localStorage.getItem('verbose_totalUsers')||'1');

// ===== PLANS DATA =====
let plansData=[];
for(let i=1;i<=30;i++){
  let invest, days, multiplier, planName, special;
  if(i<=7){ invest = 200+(i-1)*400; multiplier=2.8; days=25+(i-1)*5; planName=`Special Plan ${i}`; special=true; }
  else{ invest=4000+(i-8)*1000; multiplier=2.3; days=60+Math.floor((i-8)/3)*2; planName=`Plan ${i}`; special=false;}
  let endTime = savedEndTimes[i] || (special ? new Date().getTime()+24*60*60*1000 : new Date().getTime()+days*24*60*60*1000);
  if(special) savedEndTimes[i]=endTime;
  plansData.push({id:i,name:planName,invest,days,total:Math.round(invest*multiplier),daily:Math.round((invest*multiplier)/days),special,endTime});
}
localStorage.setItem('verbose_offerEndTimes',JSON.stringify(savedEndTimes));

// ===== FUNCTIONS =====
function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}
function login(){
  const option=document.getElementById('userOption').value;
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u||!p){alert('Enter username & password');return;}
  currentUser=u; localStorage.setItem('verbose_user',currentUser);
  referralCode=referralCode||Math.random().toString(36).substring(2,10); localStorage.setItem('verbose_referral',referralCode);
  if(option==='signup'){ balance=5000; dailyProfit=0; userPlans=[]; totalUsers++; localStorage.setItem('verbose_totalUsers',totalUsers);
    localStorage.setItem('verbose_balance',balance); localStorage.setItem('verbose_daily',dailyProfit); localStorage.setItem('verbose_userPlans',JSON.stringify(userPlans)); }
  updateDashboard();
}
function logout(){ currentUser=null; localStorage.removeItem('verbose_user'); location.reload(); }
function copyReferral(){navigator.clipboard.writeText(document.getElementById('refLink').value); alert('Referral link copied!');}
function copyDepositNumber(){navigator.clipboard.writeText(document.getElementById('depositNumber').value); alert('Deposit number copied!');}
function updateDepositNumber(){ const method=document.getElementById('depositMethod').value; document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';}

// ===== DASHBOARD =====
function updateDashboard(){
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance.toFixed(2);
  document.getElementById('dashDaily').innerText=dailyProfit.toFixed(2);
  document.getElementById('dashSince').innerText=new Date().toLocaleDateString();
  document.getElementById('refLink').value=`https://gtv140.github.io/verbose/?ref=${referralCode}`;
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('dashboard').classList.remove('hidden');
  document.getElementById('bottomNav').classList.remove('hidden');
  renderPlans(); renderHistory(); updateActiveMembers(); updateActivePlans(); setTimeout(checkSpecialOffers,5000);
}

// ===== SUPPORT =====
function openSupport(){
  window.open("https://chat.whatsapp.com/GJEVKhdDeNKCNkA8r3gONu","_blank");
}

// ===== HISTORY =====
function renderHistory(){
  const list=document.getElementById('historyList'); list.innerHTML='';
  history.forEach(h=>{
    const div=document.createElement('div');
    div.className='plan-box';
    div.innerHTML=`<div class='meta'>${h}</div>`;
    list.appendChild(div);
  });
}

// ===== PLANS =====
function renderPlans(){
  const list=document.getElementById('plansList'); list.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div'); div.className='plan-box';
    let countdownHTML=''; 
    if(p.special){ countdownHTML=`<div class='small countdown' id='countdown${p.id}'></div>`; startCountdown(p.id); }
    div.innerHTML=`<div class='meta'><b>${p.name}</b>
      <div class='small'>Invest: Rs ${p.invest} | Total: Rs ${p.total} | Daily: Rs ${p.daily} | Days: ${p.days}</div>
      ${countdownHTML}
    </div>
    <div class='actions'><button onclick='buyNow(${p.id})'>Buy Now</button></div>`;
    list.appendChild(div);
  });
}

// ===== BUY NOW =====
function buyNow(id){
  let plan = plansData.find(p=>p.id===id);
  if(!plan) return;
  if(balance < plan.invest){ alert('Insufficient balance!'); return; }
  balance -= plan.invest;
  dailyProfit += plan.daily;
  userPlans.push({id:plan.id,name:plan.name,daily:plan.daily,total:plan.total,start:new Date().getTime(),days:plan.days});
  history.push(`Bought ${plan.name} for Rs ${plan.invest} on ${new Date().toLocaleDateString()}`);
  localStorage.setItem('verbose_balance',balance);
  localStorage.setItem('verbose_daily',dailyProfit);
  localStorage.setItem('verbose_userPlans',JSON.stringify(userPlans));
  localStorage.setItem('verbose_history',JSON.stringify(history));
  updateDashboard();
  alert(`Successfully purchased ${plan.name}!`);
}

// ===== COUNTDOWN FOR SPECIAL OFFERS =====
function startCountdown(id){
  const el=document.getElementById(`countdown${id}`);
  const plan = plansData.find(p=>p.id===id);
  if(!el || !plan) return;
  const interval = setInterval(()=>{
    const now = new Date().getTime();
    let distance = plan.endTime - now;
    if(distance < 0){ el.innerText="Offer expired"; clearInterval(interval); return; }
    let hrs=Math.floor((distance%(1000*60*60*24))/(1000*60*60));
    let mins=Math.floor((distance%(1000*60*60))/(1000*60));
    let secs=Math.floor((distance%(1000*60))/1000);
    el.innerText=`Offer ends in ${hrs}h ${mins}m ${secs}s`;
  },1000);
}

// ===== ACTIVE MEMBERS & PLANS =====
function updateActiveMembers(){ document.getElementById('activeMembers').innerText=totalUsers; }
function updateActivePlans(){
  const box=document.getElementById('activePlansBox'); box.innerHTML='';
  userPlans.forEach(p=>{
    const div=document.createElement('div'); div.className='plan-box';
    div.innerHTML=`<div class='meta'><b>${p.name}</b><div class='small'>Daily: Rs ${p.daily} | Total: Rs ${p.total} | Days: ${p.days}</div></div>`;
    box.appendChild(div);
  });
}

// ===== DEPOSIT & WITHDRAW =====
function submitDeposit(){
  const amt = parseFloat(document.getElementById('depositAmount').value);
  if(isNaN(amt) || amt <=0){ alert('Enter valid amount'); return; }
  const tx = document.getElementById('depositTxId').value.trim();
  if(!tx){ alert('Enter transaction ID'); return; }
  alert(`Deposit of Rs ${amt} submitted. Admin will verify.`);
  history.push(`Deposited Rs ${amt} via ${document.getElementById('depositMethod').value} on ${new Date().toLocaleDateString()}`);
  localStorage.setItem('verbose_history',JSON.stringify(history));
  renderHistory();
}

function submitWithdraw(){
  const amt = parseFloat(document.getElementById('withdrawAmount').value);
  if(isNaN(amt) || amt <=0){ alert('Enter valid amount'); return; }
  const acc = document.getElementById('withdrawAccount').value.trim();
  if(!acc){ alert('Enter account number'); return; }
  alert(`Withdrawal of Rs ${amt} requested. Admin will process.`);
  history.push(`Requested withdrawal Rs ${amt} via ${document.getElementById('withdrawMethod').value} on ${new Date().toLocaleDateString()}`);
  localStorage.setItem('verbose_history',JSON.stringify(history));
  renderHistory();
}

// ===== POPUP =====
function closePopup(){ document.getElementById('popup').classList.add('hidden'); }
function showPopup(msg){ document.getElementById('popupText').innerText=msg; document.getElementById('popup').classList.remove('hidden'); }

// ===== SPECIAL OFFERS CHECK =====
function checkSpecialOffers(){
  plansData.forEach(p=>{
    if(p.special){
      const now = new Date().getTime();
      if(now > p.endTime){
        // Offer expired, remove if needed
        p.special=false;
        localStorage.setItem('verbose_offerEndTimes',JSON.stringify(savedEndTimes));
      }
    }
  });
}

// ===== INIT =====
if(currentUser) updateDashboard();
</script>
</body>
</html>
