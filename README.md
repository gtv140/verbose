<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>VERBOSE</title>
<style>
:root{--neon:#00f7ff;--accent:#ff5cff;--dark:#070707;}
*{box-sizing:border-box}
body{margin:0;font-family:Inter,Arial,sans-serif;background:var(--dark);color:#fff;overflow-x:hidden}
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
.countdown{font-weight:700;color:var(--neon)}
.logout-btn{padding:8px 10px;font-size:13px;border-radius:8px}
@media (max-width:480px){.login-box,.page{margin:12px;padding:14px}.nav div{width:48px}header{font-size:22px}}
</style>
</head>
<body>
<header>VERBOSE</header>

<div id="loginPage" class="login-box">
<h2>Login / Signup</h2>
<select id="userOption">
<option value="login">Login</option>
<option value="signup">New User Signup</option>
</select>
<input id="user" placeholder="Username" />
<input id="pass" placeholder="Password" type="password" />
<button onclick="login()">Submit</button>
<p class="small">Tip: Use same device/browser to keep your account saved.</p>
</div>

<div id="dashboard" class="page hidden">
<div class="user-box">
<div class="left">
<div style="display:flex;gap:12px;align-items:center">
<div style="width:56px;height:56px;border-radius:12px;background:linear-gradient(90deg,var(--neon),var(--accent));display:flex;align-items:center;justify-content:center;color:#001;font-weight:900">V</div>
<div><div id="dashUser">—</div>
<div class="small">Member since: <span id="dashSince">—</span></div></div>
</div>
</div>
<div class="right">
<div>Balance</div>
<div>Rs <span id="dashBalance">0</span></div>
<div style="margin-top:8px" class="badge">Daily: Rs <span id="dashDaily">0</span></div>
</div>
</div>
<div class="referral-box">
<input id="refLink" readonly style="flex:1" />
<button onclick="copyReferral()">Copy Link</button>
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
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="depositNumber" readonly />
<input id="depositAmount" readonly />
<label>TX ID</label>
<input id="depositTxId" placeholder="TX ID" />
<label>Upload Proof</label>
<input type="file" id="depositProof" />
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<div id="withdrawal" class="page hidden">
<h2>Withdrawal</h2>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<input id="withdrawUsername" readonly />
<input id="withdrawAccount" placeholder="Account Number" />
<input id="withdrawAmount" placeholder="Amount" />
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<div id="support" class="page hidden">
<h2>Support</h2>
<p>Contact admin via official channels only.</p>
</div>

<div id="bottomNav" class="nav hidden">
<div onclick="showPage('dashboard')"><span class="ico">🏠</span>Home</div>
<div onclick="showPage('plans')"><span class="ico">📦</span>Plans</div>
<div onclick="showPage('deposit')"><span class="ico">💰</span>Deposit</div>
<div onclick="showPage('withdrawal')"><span class="ico">💵</span>Withdraw</div>
<div onclick="showPage('support')"><span class="ico">📞</span>Support</div>
</div>

<script>
let currentUser=localStorage.getItem('verbose_user')||null;
let balance=parseFloat(localStorage.getItem('verbose_balance'))||0;
let dailyProfit=parseFloat(localStorage.getItem('verbose_daily'))||0;
let userPlans=JSON.parse(localStorage.getItem('verbose_userPlans')||'[]');
let referralCode=localStorage.getItem('verbose_referral')||'';
let plansData=[];
for(let i=1;i<=30;i++){
let invest,days,multiplier,coming=false,countdown;
if(i<=7){invest=Math.round(200+(i-1)*(3000-200)/6);multiplier=3;days=20+Math.floor((i-1)*(50-20)/6);countdown=localStorage.getItem(`planCountdown_${i}`)||Date.now()+24*3600*1000;}
else if(i<=25){invest=Math.round(200+(i-1)*(30000-200)/24);multiplier=2.5;days=20+Math.floor((i-1)*(70-20)/24);countdown=null;}
else{invest=Math.round(200+(i-1)*(30000-200)/24);multiplier=2.5;days=20+Math.floor((i-1)*(70-20)/24);coming=true;countdown=null;}
plansData.push({id:i,name:`Plan ${i}`,invest,days,total:Math.round(invest*multiplier),multiplier,offer:i<=7,coming,countdown});
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

function logout(){currentUser=null;localStorage.removeItem('verbose_user');document.getElementById('loginPage').classList.remove('hidden');document.getElementById('dashboard').classList.add('hidden');document.getElementById('bottomNav').classList.add('hidden');document.getElementById('user').value='';document.getElementById('pass').value='';}

function showPage(id){document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}

function copyDepositNumber(){const num=document.getElementById('depositNumber');num.select();document.execCommand('copy');alert("Deposit number copied!");}
function copyReferral(){const ref=document.getElementById('refLink');ref.select();document.execCommand('copy');alert("Referral link copied!");}

function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';document.getElementById('depositAmount').value='1000';}

function renderPlans(){
const list=document.getElementById('plansList');
list.innerHTML='';
plansData.forEach(plan=>{
const div=document.createElement('div');
div.className='plan-box';
div.innerHTML=`<div class="meta"><b>${plan.name}</b><br>Invest: Rs ${plan.invest}<br>Total: Rs ${plan.total}<br>Days: ${plan.days}${plan.offer?`<br><span class="offer">Special Offer! <span class="countdown" id="countdown${plan.id}">—</span></span>`:''}</div><div class="actions">${plan.coming?'Coming Soon':''}</div>`;
list.appendChild(div);
});
startCountdowns();
}

function startCountdowns(){
plansData.forEach(plan=>{
if(plan.offer){
const el=document.getElementById('countdown'+plan.id);
if(!el) return;
let interval=setInterval(()=>{
let now=Date.now();
let diff=plan.countdown-now;
if(diff<=0){el.innerText='Expired';clearInterval(interval);}
else{let h=Math.floor(diff/3600000);let m=Math.floor((diff%3600000)/60000);let s=Math.floor((diff%60000)/1000);el.innerText=`${h}h ${m}m ${s}s`;}
},1000);
}
});
}
</script>
</body>
</html>
