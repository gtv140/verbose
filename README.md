<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>VERBOSE</title>
<style>
:root{--neon:#00f7ff;--accent:#ff5cff;--dark:#070707;--panel:#0f1114}
*{box-sizing:border-box}
body{margin:0;font-family:Inter,Arial,sans-serif;background:var(--dark);color:#fff;overflow-x:hidden;position:relative;-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale}
header{text-align:center;padding:24px 12px;font-size:28px;font-weight:800;letter-spacing:2px;background:linear-gradient(90deg,var(--neon),var(--accent));-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.login-box,.page{max-width:430px;margin:18px auto;background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0.01));padding:18px;border-radius:12px;border:1px solid rgba(0,255,240,0.06);box-shadow:0 8px 30px rgba(0,0,0,0.6)}
input,button,select{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid rgba(0,255,240,0.08);background:transparent;color:#e6f7fb;outline:none;font-size:14px}
input::placeholder{color:rgba(230,247,251,0.5)}
button{background:linear-gradient(90deg,var(--neon),var(--accent));border:none;color:#001;font-weight:700;cursor:pointer;padding:11px;border-radius:10px;transition:transform .15s ease,box-shadow .15s}
button:hover{transform:translateY(-2px);box-shadow:0 10px 30px rgba(0,0,0,0.5)}
.nav{position:fixed;bottom:0;left:0;right:0;background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0.01));display:flex;justify-content:space-around;padding:12px 6px;border-top:1px solid rgba(0,255,240,0.04);font-size:14px;gap:6px}
.nav div{text-align:center;cursor:pointer;width:64px}
.nav div .ico{font-size:20px;display:block;margin-bottom:4px}
.hidden{display:none}
.small{font-size:13px;color:rgba(230,247,251,0.8)}
.user-box{background:linear-gradient(90deg,rgba(0,255,240,0.06),rgba(255,92,255,0.02));padding:14px;border-radius:12px;margin-bottom:12px;font-weight:700;display:flex;justify-content:space-between;align-items:center;gap:10px;border:1px solid rgba(0,255,240,0.06)}
.user-box .left{display:flex;flex-direction:column;gap:6px}
.user-box .right{text-align:right;font-size:13px}
.badge{background:rgba(0,255,240,0.06);padding:6px 10px;border-radius:999px;color:var(--neon);font-weight:800}
.alert-box{background:linear-gradient(90deg,rgba(255,0,136,0.04),rgba(0,0,0,0.02));padding:12px;border-radius:10px;margin-bottom:12px;color:var(--accent);border:1px solid rgba(255,0,136,0.06);box-shadow:0 6px 20px rgba(255,0,136,0.03) inset}
.plan-box{border:1px solid rgba(0,255,240,0.06);padding:12px;margin:10px 0;border-radius:10px;background:linear-gradient(180deg,rgba(255,255,255,0.01),rgba(0,0,0,0.04));transition:box-shadow .2s ease,transform .12s ease;display:flex;gap:12px;align-items:center;justify-content:space-between}
.plan-box .meta{flex:1}
.plan-box .meta b{display:block;margin-bottom:6px;font-size:15px}
.plan-box .meta .small{margin-top:6px}
.plan-box .actions button{width:110px}
.plan-box:hover{box-shadow:0 12px 30px rgba(0,255,240,0.06);transform:translateY(-4px)}
.offer{color:var(--neon);font-weight:800}
.referral-box{background:linear-gradient(180deg,rgba(255,255,255,0.01),rgba(0,0,0,0.03));padding:12px;border-radius:10px;margin:10px 0;border:1px solid rgba(0,255,240,0.04)}
.referral-box input{background:transparent;border:1px dashed rgba(255,255,255,0.03);padding:8px;border-radius:8px}
.support-box{background:linear-gradient(90deg,rgba(0,0,0,0.35),rgba(0,0,0,0.45));padding:14px;border-radius:12px;border:1px solid rgba(255,0,136,0.06);box-shadow:0 10px 30px rgba(255,0,136,0.03) inset}
.support-box h2{margin:0 0 8px 0;font-size:18px;background:linear-gradient(90deg,var(--neon),var(--accent));-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.support-grid{display:flex;gap:12px;flex-direction:column}
.support-item{display:flex;gap:10px;align-items:center}
.support-item .icon{width:44px;height:44px;border-radius:10px;display:flex;align-items:center;justify-content:center;background:rgba(0,255,240,0.04);color:var(--neon);font-weight:800}
.support-item p{margin:0;font-size:14px}
.support-note{font-size:13px;color:rgba(230,247,251,0.75);margin-top:8px}
.countdown{font-weight:700;color:var(--neon)}
.logout-btn{padding:8px 10px;font-size:13px;border-radius:8px}
@media (max-width:480px){.login-box,.page{margin:12px;padding:14px}.nav div{width:48px}header{font-size:22px}}
</style>
</head>
<body>
<header>VERBOSE</header>

<!-- LOGIN / SIGNUP -->
<div id="loginPage" class="login-box">
<h2 style="margin:0 0 8px 0">Login / Signup</h2>
<select id="userOption">
<option value="login">Login</option>
<option value="signup">New User Signup</option>
</select>
<input id="user" placeholder="Username" />
<input id="pass" placeholder="Password" type="password" />
<button onclick="login()">Submit</button>
<p class="small">Tip: Use same device/browser to keep your account saved (local storage).</p>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="page hidden">
<div class="alert-box">Withdrawal or Deposit issues? Contact Administration immediately. Company info at bottom.</div>
<div class="user-box">
<div class="left">
<div style="display:flex;gap:12px;align-items:center">
<div style="width:56px;height:56px;border-radius:12px;background:linear-gradient(90deg,var(--neon),var(--accent));display:flex;align-items:center;justify-content:center;color:#001;font-weight:900">V</div>
<div><div style="font-size:16px;font-weight:800" id="dashUser">—</div>
<div class="small">Member since: <span id="dashSince">—</span></div></div>
</div>
</div>
<div class="right">
<div style="font-size:13px">Balance</div>
<div style="font-size:18px;font-weight:900">Rs <span id="dashBalance">0</span></div>
<div style="margin-top:8px" class="badge">Daily: Rs <span id="dashDaily">0</span></div>
</div>
</div>
<h2 style="text-align:center;color:var(--neon);margin:6px 0 10px 0">Dashboard Overview</h2>
<p class="small" style="text-align:center;margin-top:-6px">Track your plans, deposits and withdrawals securely. Admin support is 24/7.</p>
<div class="referral-box">
<div style="display:flex;gap:8px;align-items:center">
<input id="refLink" readonly style="flex:1" />
<button onclick="copyReferral()" style="width:110px">Copy Link</button>
</div>
<div class="small">Share this link to invite friends. When they deposit, you get bonus credits automatically.</div>
</div>
<div style="margin-top:8px; text-align:center">
<button class="logout-btn" onclick="logout()">Logout</button>
</div>
<div style="text-align:center;margin-top:12px;font-size:13px;color:rgba(230,247,251,0.8)">
VERBOSE Company — Secure, Transparent, Professional.<br>
Your trusted investment platform with verified plans, automated profit calculation, and 24/7 admin support.<br>
Contact Admin: WhatsApp 03705519562 | Call: 03379827882
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
<p class="small">After submitting, share proof with admin via WhatsApp or Call for verification.</p>
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
<p class="small">Requests are reviewed by admin. Keep your proof ready. Contact admin if any issue.</p>
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
<p class="small">Fastest support — message admin directly.</p>
<p>+92 3705519562</p>
</div>
</div>
<div class="support-item">
<div class="icon">📞</div>
<div>
<p><strong>Call Support</strong></p>
<p class="small">24/7 assistance for deposits, withdrawals, and account issues.</p>
<p>+92 3379827882</p>
</div>
</div>
</div>
<p class="support-note">Always use official support channels. Avoid sharing sensitive info publicly.</p>
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
// ===== USERS & PLANS =====
let currentUser = localStorage.getItem('verbose_user') || null;
let balance = parseFloat(localStorage.getItem('verbose_balance')) || 0;
let dailyProfit = parseFloat(localStorage.getItem('verbose_daily')) || 0;
let userPlans = JSON.parse(localStorage.getItem('verbose_userPlans')||'[]');
let referralCode = localStorage.getItem('verbose_referral') || '';

let plansData = [];
// 7 special plans 3x
for(let i=1;i<=7;i++){
  let invest=200*i; let multiplier=3; let days=5+i; let daily=Math.round((invest*multiplier)/days);
  plansData.push({id:i,name:`Plan ${i}`,invest,total:invest*multiplier,daily,days,offer:true,countdown:Date.now()+24*3600*1000});
}
// Normal plans up to 30, 2.5x
for(let i=8;i<=30;i++){
  let invest=500*i; let multiplier=2.5; let days=10+i; let daily=Math.round((invest*multiplier)/days);
  plansData.push({id:i,name:`Plan ${i}`,invest,total:Math.round(invest*multiplier),daily,days,offer:false});
}

// ===== LOGIN / DASHBOARD =====
function login(){
  const option=document.getElementById('userOption').value;
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u||!p){alert("Enter username & password");return;}
  currentUser=u;
  localStorage.setItem('verbose_user',currentUser);
  referralCode=referralCode||Math.random().toString(36).substring(2,10);
  localStorage.setItem('verbose_referral',referralCode);
  if(option==='signup'){localStorage.setItem('verbose_balance','0');localStorage.setItem('verbose_daily','0');userPlans=[];localStorage.setItem('verbose_userPlans',JSON.stringify(userPlans));}
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
}

// ===== LOGOUT =====
function logout(){
  currentUser=null;
  localStorage.removeItem('verbose_user');
  document.getElementById('loginPage').classList.remove('hidden');
  document.getElementById('dashboard').classList.add('hidden');
  document.getElementById('bottomNav').classList.add('hidden');
  document.getElementById('user').value='';
  document.getElementById('pass').value='';
}

// ===== SHOW PAGES =====
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

// ===== COPY =====
function copyDepositNumber(){const num=document.getElementById('depositNumber');num.select();document.execCommand('copy');alert("Deposit number copied!");}
function copyReferral(){const ref=document.getElementById('refLink');ref.select();document.execCommand('copy');alert("Referral link copied!");}

// ===== DEPOSIT =====
function updateDepositNumber(){
  const method=document.getElementById('depositMethod').value;
  document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}

// ===== SUBMIT DEPOSIT =====
function submitDeposit(){
  let amount=parseFloat(document.getElementById('depositAmount').value);
  let tx=document.getElementById('depositTxId').value.trim();
  let proof=document.getElementById('depositProof').files[0];
  if(!tx || !proof){alert("Transaction ID and proof are required!");return;}
  alert("Deposit submitted! Contact admin for verification.");
}

// ===== WITHDRAW =====
function submitWithdraw(){
  let method=document.getElementById('withdrawMethod').value;
  let account=document.getElementById('withdrawAccount').value.trim();
  let amount=parseFloat(document.getElementById('withdrawAmount').value);
  if(!amount || !account){alert("Enter account number & amount!");return;}
  alert(`Withdrawal request submitted!\nMethod: ${method}\nAmount: Rs ${amount}\nAdmin will review soon.`);
}

// ===== RENDER PLANS =====
function renderPlans(){
  const container=document.getElementById('plansList'); container.innerHTML='';
  plansData.forEach(p=>{
    let div=document.createElement('div'); div.className='plan-box';
    div.innerHTML=`<div class="meta">
  <b>${p.name} ${p.offer?'<span class="offer">[Special Offer]</span>':''}</b>
  <div>Invest: Rs ${p.invest}</div>
  <div>Total Profit: Rs ${p.total}</div>
  <div>Daily Profit: Rs ${p.daily}</div>
  <div>Days: ${p.days}</div>
  ${p.offer?`<div class="countdown" id="countdown${p.id}">Countdown: --:--:--</div>`:''}
</div>
<div class="actions">
  <button onclick="buyPlan(${p.id})">Buy Now</button>
</div>
`;
    container.appendChild(div);

    // Countdown timer
    if(p.offer){
      let cdElem=document.getElementById(`countdown${p.id}`);
      function updateCountdown(){
        let now=Date.now();
        let diff=p.countdown-now;
        if(diff<=0){cdElem.innerText="Offer expired"; return;}
        let h=Math.floor(diff/3600000);
        let m=Math.floor((diff%3600000)/60000);
        let s=Math.floor((diff%60000)/1000);
        cdElem.innerText=`Countdown: ${h}h ${m}m ${s}s`;
      }
      updateCountdown();
      setInterval(updateCountdown,1000);
    }
  });
}

// ===== BUY PLAN =====
function buyPlan(id){
  let plan=plansData.find(p=>p.id===id);
  if(!plan){alert("Plan not found!");return;}
  alert(`You selected ${plan.name}.\nInvest Rs ${plan.invest}.\nAdmin will verify deposit.`);
  document.getElementById('depositAmount').value=plan.invest;
  showPage('deposit');
}

</script>
</body>
</html>
