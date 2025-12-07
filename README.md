<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#f4f4f4;color:#333;}
header{padding:20px;text-align:center;font-weight:900;font-size:24px;background:#222;color:#fff;}
.wrap{max-width:480px;margin:12px auto;padding:12px;background:#fff;border-radius:8px;box-shadow:0 0 8px rgba(0,0,0,0.1);}
.card{padding:12px;margin-bottom:12px;border-radius:8px;border:1px solid #ccc;background:#fafafa;}
input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:6px;border:1px solid #ccc;font-size:14px;}
button{cursor:pointer;background:#007bff;color:#fff;border:none;}
button:hover{background:#0056b3;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:#eee;border-top:1px solid #ccc;}
.nav div{text-align:center;font-size:13px;cursor:pointer;}
.plan{display:flex;justify-content:space-between;align-items:center;padding:10px;margin-bottom:6px;border:1px solid #ccc;border-radius:6px;background:#fefefe;}
.plan button{width:auto;padding:6px 12px;margin-left:6px;}
.alert{background:#ffe0e0;padding:10px;margin-bottom:12px;border-radius:6px;color:#a00;}
.success{background:#e0ffe0;padding:10px;margin-bottom:12px;border-radius:6px;color:#070;}
.referral-box{display:flex;gap:6px;margin-bottom:12px;}
.referral-box input{flex:1;}
</style>
</head>
<body>
<header>VERBOSE</header>
<div class="wrap">

<!-- LOGIN / SIGNUP -->
<div id="loginCard" class="card">
<h3>Login / Signup</h3>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">New User</option>
</select>
<input id="inputUser" placeholder="Username"/>
<input id="inputPass" placeholder="Password" type="password"/>
<input id="referralInput" placeholder="Referral Code (optional)"/>
<button onclick="doAuth()">Submit</button>
<p style="font-size:13px;color:#555;">Tip: Use same device & browser. Data stored locally.</p>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card" style="display:none;">
<div class="alert">⚠️ All transactions are manually approved. Deposit/Withdrawal issues? Contact Administration.</div>
<div>
<strong id="welcomeText">Welcome —</strong>
<p id="memberSince">Member since —</p>
<p>Balance: Rs <span id="balanceText">0</span></p>
<p>Daily: Rs <span id="dailyText">0</span></p>
<button onclick="doLogout()">Logout</button>
</div>

<div class="referral-box">
<input id="referralLink" readonly/>
<button onclick="copyReferral()">Copy Referral</button>
</div>

<div>
<h4>Support / Admin</h4>
<p>Email: <strong>Rock.earn92@gmail.com</strong></p>
<p>Whatsapp: <strong>03705519562</strong></p>
<p>VERBOSE is a branch of Rock Earn. Millions of users are working globally.</p>
</div>
</div>

<!-- PLANS -->
<div id="plansCard" class="card" style="display:none;">
<h3>Plans</h3>
<p>Special Offers: 7 plans (24h timer). Normal plans: 25 plans.</p>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="depositCard" class="card" style="display:none;">
<h3>Deposit</h3>
<label>Method</label>
<select id="depositMethod">
<option value="jazzcash">JazzCash — 03705519562</option>
<option value="easypaisa">EasyPaisa — 03379827882</option>
</select>
<label>Amount</label>
<input id="depositAmount" readonly/>
<label>TX ID</label>
<input id="depositTx" placeholder="Enter TX ID"/>
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawCard" class="card" style="display:none;">
<h3>Withdrawal</h3>
<label>Username</label>
<input id="withdrawUsername" readonly/>
<label>Account / Mobile Number</label>
<input id="withdrawAccount" placeholder="Enter account or mobile number"/>
<label>Amount</label>
<input id="withdrawAmount" placeholder="Enter amount"/>
<button onclick="submitWithdraw()">Request Withdrawal</button>
<p>Withdrawal is manually approved. Contact admin for urgent issues.</p>
</div>

</div>

<div class="nav">
<div onclick="nav('dashboardCard')">Home</div>
<div onclick="nav('plansCard')">Plans</div>
<div onclick="nav('depositCard')">Deposit</div>
<div onclick="nav('withdrawCard')">Withdraw</div>
</div>

<script>
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
const KEY_DAILY='verbose_daily_';
const KEY_USER_PLANS='verbose_plans_';
const KEY_DEPOSITS='verbose_deposits';
const KEY_WITHDRAWS='verbose_withdraws';
let currentUser=localStorage.getItem(KEY_USER)||null;

// 7 Special Offers
let plans=[];
for(let i=1;i<=7;i++){
let invest=200*i;
if(invest>3000) invest=3000;
let days=20+Math.floor(Math.random()*51);
plans.push({id:i,name:'Special Plan '+i,invest:invest,total:invest*3,days:days});
}

function fmt(n){return Number(n).toLocaleString();}

// AUTH
function doAuth(){
const mode=document.getElementById('authMode').value;
const u=document.getElementById('inputUser').value.trim();
const p=document.getElementById('inputPass').value.trim();
const ref=document.getElementById('referralInput').value.trim();
if(!u||!p){alert('Enter username & password');return;}
const credKey='verbose_cred_'+u;
if(mode==='signup'){
if(localStorage.getItem(credKey)){alert('Username exists');return;}
localStorage.setItem(credKey,p);
localStorage.setItem(KEY_BAL+u,'0');
localStorage.setItem(KEY_DAILY+u,'0');
localStorage.setItem(KEY_USER_PLANS+u,'[]');
if(ref && localStorage.getItem('verbose_cred_'+ref)){
let bal=Number(localStorage.getItem(KEY_BAL+ref)||0);bal+=30;
localStorage.setItem(KEY_BAL+ref,bal);
alert(`Referral bonus Rs 30 added to ${ref}`);
}
}else{
if(localStorage.getItem(credKey)!==p){alert('Wrong username/password');return;}
}
localStorage.setItem(KEY_USER,u);currentUser=u;
afterLoginUI();
}

function afterLoginUI(){nav('dashboardCard');renderPlans();updateReferralLink();}
function updateReferralLink(){if(!currentUser) return;document.getElementById('referralLink').value=window.location.href+'?ref='+currentUser;}
function copyReferral(){const link=document.getElementById('referralLink');link.select();document.execCommand('copy');alert('Referral link copied!');}
function doLogout(){localStorage.removeItem(KEY_USER);currentUser=null;nav('loginCard');}

// NAV
function nav(cardId){
['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>document.getElementById(id).style.display='none');
document.getElementById(cardId).style.display='block';
renderDashboard();
fillWithdrawUser();
}

// DASHBOARD
function renderDashboard(){
if(!currentUser) return;
document.getElementById('welcomeText').innerText='Welcome, '+currentUser;
document.getElementById('memberSince').innerText='Member since: '+new Date().toLocaleDateString();
document.getElementById('balanceText').innerText=fmt(Number(localStorage.getItem(KEY_BAL+currentUser)||0));
document.getElementById('dailyText').innerText=fmt(Number(localStorage.getItem(KEY_DAILY+currentUser)||0));
}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList');container.innerHTML='';
plans.forEach(plan=>{
const div=document.createElement('div');div.className='plan';
div.innerHTML=`<div><strong>${plan.name}</strong><br>Invest: Rs ${fmt(plan.invest)} · Total: Rs ${fmt(plan.total)} · Days: ${plan.days}</div>
<button onclick="buyPlan(${plan.id})">Buy Now</button>`;
container.appendChild(div);
});
}
function buyPlan(id){
if(!currentUser){alert('Login first');return;}
const plan=plans.find(p=>p.id===id);
if(!plan){alert('Plan not found');return;}
let userPlans=JSON.parse(localStorage.getItem(KEY_USER_PLANS+currentUser)||'[]');
userPlans.push({planId:plan.id,dailyProfit:Math.round(plan.total/plan.days),lastCredit:Date.now()});
localStorage.setItem(KEY_USER_PLANS+currentUser,JSON.stringify(userPlans));
document.getElementById('depositAmount').value=plan.invest;
nav('depositCard');
alert(`Plan ${plan.name} selected. Submit your deposit. Admin will manually approve.`);
}

// DEPOSIT
function submitDeposit(){
if(!currentUser){alert('Login first');return;}
const amount=Number(document.getElementById('depositAmount').value);
const tx=document.getElementById('depositTx').value.trim();
if(!tx||!amount){alert('All fields required');return;}
const deposits=JSON.parse(localStorage.getItem(KEY_DEPOSITS)||'[]');
deposits.push({user:currentUser,amount,tx,approved:false});
localStorage.setItem(KEY_DEPOSITS,JSON.stringify(deposits));
alert('Deposit submitted! Admin will approve manually.');
document.getElementById('depositTx').value='';
nav('dashboardCard');
}

// WITHDRAW
function fillWithdrawUser(){if(!currentUser) return;document.getElementById('withdrawUsername').value=currentUser;}
function submitWithdraw(){
if(!currentUser){alert('Login first');return;}
const account=document.getElementById('withdrawAccount').value.trim();
const amount=Number(document.getElementById('withdrawAmount').value);
if(!account||!amount){alert('Enter account & amount');return;}
const withdraws=JSON.parse(localStorage.getItem(KEY_WITHDRAWS)||'[]');
withdraws.push({user:currentUser,account,amount,approved:false});
localStorage.setItem(KEY_WITHDRAWS,JSON.stringify(withdraws));
alert('Withdrawal requested! Admin will approve manually.');
document.getElementById('withdrawAccount').value='';document.getElementById('withdrawAmount').value='';
nav('dashboardCard');
}
</script>
</body>
</html>
