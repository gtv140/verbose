<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>VERBOSE</title>
<style>
:root{--neon:#00f7ff;--accent:#ff5cff;--dark:#070707}
*{box-sizing:border-box;margin:0;padding:0;font-family:Arial,sans-serif}
body{background:var(--dark);color:#fff;overflow-x:hidden}
header{text-align:center;padding:20px;font-size:28px;font-weight:800;background:linear-gradient(90deg,var(--neon),var(--accent));-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.login-box,.page{max-width:430px;margin:20px auto;padding:18px;border-radius:12px;background:rgba(255,255,255,0.02);border:1px solid rgba(0,255,240,0.06);box-shadow:0 8px 30px rgba(0,0,0,0.6)}
input,button,select{width:100%;padding:10px;margin-top:10px;border-radius:8px;border:1px solid rgba(0,255,240,0.08);background:transparent;color:#e6f7fb;outline:none;font-size:14px}
button{background:linear-gradient(90deg,var(--neon),var(--accent));border:none;color:#001;font-weight:700;cursor:pointer;transition:0.15s}
button:hover{transform:translateY(-2px)}
.nav{position:fixed;bottom:0;left:0;right:0;background:rgba(255,255,255,0.02);display:flex;justify-content:space-around;padding:10px;border-top:1px solid rgba(0,255,240,0.04)}
.nav div{text-align:center;cursor:pointer;width:60px}
.hidden{display:none}
.small{font-size:13px;color:rgba(230,247,251,0.7)}
.user-box{background:rgba(0,255,240,0.06);padding:12px;border-radius:12px;margin-bottom:12px;display:flex;justify-content:space-between;align-items:center}
.badge{background:rgba(0,255,240,0.06);padding:4px 10px;border-radius:999px;color:var(--neon);font-weight:700}
.plan-box{border:1px solid rgba(0,255,240,0.06);padding:12px;margin:10px 0;border-radius:10px;background:rgba(255,255,255,0.01);display:flex;justify-content:space-between;align-items:center;transition:0.2s}
.plan-box:hover{box-shadow:0 12px 30px rgba(0,255,240,0.06)}
.countdown{font-weight:700;color:var(--neon)}
</style>
</head>
<body>
<header>VERBOSE</header>

<div id="loginPage" class="login-box">
<h2>Login / Signup</h2>
<select id="userOption"><option value="login">Login</option><option value="signup">Signup</option></select>
<input id="user" placeholder="Username"/>
<input id="pass" placeholder="Password" type="password"/>
<button onclick="login()">Submit</button>
<p class="small">Use same device/browser to keep your account saved.</p>
</div>

<div id="dashboard" class="page hidden">
<div class="user-box">
<div>
<b id="dashUser">—</b><br>
<span class="small">Member since: <span id="dashSince">—</span></span>
</div>
<div>
Balance: Rs <span id="dashBalance">0</span><br>
<span class="badge">Daily: Rs <span id="dashDaily">0</span></span>
</div>
</div>
<h2>Dashboard Overview</h2>
<div class="referral-box">
<input id="refLink" readonly style="flex:1"/>
<button onclick="copyReferral()">Copy Link</button>
<p class="small">Share link to get bonuses when friends deposit.</p>
</div>
<button onclick="logout()">Logout</button>
<div class="small" style="margin-top:10px;text-align:center;">
VERBOSE Company — Secure, Transparent, Professional.<br>
Admin Support 24/7: WhatsApp 03705519562 | Call 03379827882
</div>
</div>

<div id="plans" class="page hidden">
<h2>Plans</h2>
<div id="plansList"></div>
</div>

<div id="deposit" class="page hidden">
<h2>Deposit</h2>
<label>Method</label>
<select id="depositMethod" onchange="updateDepositNumber()"><option value="jazzcash">JazzCash</option><option value="easypaisa">EasyPaisa</option></select>
<div style="display:flex;gap:8px;margin-top:10px">
<input id="depositNumber" readonly style="flex:1"/>
<button onclick="copyDepositNumber()">Copy</button>
</div>
<label>Amount</label>
<input id="depositAmount" readonly/>
<label>TX ID</label>
<input id="depositTxId" placeholder="Transaction ID"/>
<label>Upload Proof</label>
<input type="file" id="depositProof"/>
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="withdrawal" class="page hidden">
<h2>Withdrawal</h2>
<label>Method</label>
<select id="withdrawMethod"><option value="jazzcash">JazzCash</option><option value="easypaisa">EasyPaisa</option><option value="bank">Bank</option></select>
<input id="withdrawAccount" placeholder="Account Number"/>
<input id="withdrawAmount" placeholder="Amount"/>
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<div id="support" class="page hidden">
<h2>Support</h2>
<p>WhatsApp: 03705519562 | Call: 03379827882</p>
</div>

<div class="nav">
<div onclick="showPage('dashboard')">🏠 Home</div>
<div onclick="showPage('plans')">📦 Plans</div>
<div onclick="showPage('deposit')">💰 Deposit</div>
<div onclick="showPage('withdrawal')">💵 Withdraw</div>
<div onclick="showPage('support')">📞 Support</div>
</div>

<script>
let currentUser=localStorage.getItem('verbose_user')||null;
let balance=parseFloat(localStorage.getItem('verbose_balance'))||0;
let dailyProfit=parseFloat(localStorage.getItem('verbose_daily'))||0;
let referralCode=localStorage.getItem('verbose_referral')||'';

let plansData=[];

// 7 special plans 200-3000 with 3x
for(let i=1;i<=7;i++){
  let invest=200*i*2;
  let multiplier=3;
  let days=5+i;
  let daily=Math.round((invest*multiplier)/days);
  plansData.push({id:i,name:`Plan ${i}`,invest,total:invest*multiplier,daily,days,offer:true,countdown:Date.now()+24*3600*1000});
}
// Normal plans 8-30 upto 30000, 2.5x
for(let i=8;i<=30;i++){
  let invest=1000*i;
  let multiplier=2.5;
  let days=10+i;
  let daily=Math.round((invest*multiplier)/days);
  plansData.push({id:i,name:`Plan ${i}`,invest,total:Math.round(invest*multiplier),daily,days,offer:false});
}

function login(){
  const option=document.getElementById('userOption').value;
  const u=document.getElementById('user').value.trim();
  const p=document.getElementById('pass').value.trim();
  if(!u||!p){alert("Enter username & password");return;}
  currentUser=u;
  localStorage.setItem('verbose_user',currentUser);
  referralCode=referralCode||Math.random().toString(36).substring(2,10);
  localStorage.setItem('verbose_referral',referralCode);
  if(option==='signup'){localStorage.setItem('verbose_balance','0');localStorage.setItem('verbose_daily','0');}
  balance=parseFloat(localStorage.getItem('verbose_balance'))||0;
  dailyProfit=parseFloat(localStorage.getItem('verbose_daily'))||0;
  document.getElementById('dashUser').innerText=currentUser;
  document.getElementById('dashBalance').innerText=balance;
  document.getElementById('dashDaily').innerText=dailyProfit;
  document.getElementById('dashSince').innerText=new Date().toLocaleDateString();
  document.getElementById('refLink').value=`https://gtv140.github.io/verbose/?ref=${referralCode}`;
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('dashboard').classList.remove('hidden');
  renderPlans();
}

function logout(){
  currentUser=null;
  localStorage.removeItem('verbose_user');
  document.getElementById('loginPage').classList.remove('hidden');
  document.getElementById('dashboard').classList.add('hidden');
}

function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

function copyReferral(){const ref=document.getElementById('refLink');ref.select();document.execCommand('copy');alert("Referral copied!");}
function copyDepositNumber(){const num=document.getElementById('depositNumber');num.select();document.execCommand('copy');alert("Deposit number copied!");}

function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';}
function submitDeposit(){let tx=document.getElementById('depositTxId').value.trim();let proof=document.getElementById('depositProof').files[0];if(!tx||!proof){alert("TX & proof required!");return;} alert("Deposit submitted! Contact admin.");}
function submitWithdraw(){let amount=parseFloat(document.getElementById('withdrawAmount').value);let acc=document.getElementById('withdrawAccount').value.trim();if(!amount||!acc){alert("Enter account & amount");return;} alert(`Withdrawal requested Rs ${amount}. Admin will review.`);}

function renderPlans(){
  const container=document.getElementById('plansList'); container.innerHTML='';
  plansData.forEach(p=>{
    const div=document.createElement('div'); div.className='plan-box';
    div.innerHTML=`<div>
<b>${p.name}</b>
<div class="small">Invest: Rs ${p.invest} | Total: Rs ${p.total} | Days: ${p.days} ${p.offer?'<span style="color:#0ff">Special!</span>':''}</div>
<div class="countdown" id="timer${p.id}"></div>
<div class="small">Daily Profit: Rs ${p.daily}</div>
</div>`;
    container.appendChild(div);

    if(p.offer){
      const timerEl=document.getElementById(`timer${p.id}`);
      const interval=setInterval(()=>{
        let diff=p.countdown-Date.now();
        if(diff<=0){timerEl.innerText="Offer ended";clearInterval(interval);}
        else{
          let h=Math.floor(diff/3600000); let m=Math.floor((diff%3600000)/60000); let s=Math.floor((diff%60000)/1000);
          timerEl.innerText=`Countdown: ${h}h ${m}m ${s}s`;
        }
      },1000);
    }
  });
}

</script>
</body>
</html>
