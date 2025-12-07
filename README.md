<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE - Rock Earn Branch</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
body{margin:0;font-family:Arial,sans-serif;background:#f5f5f5;color:#333;}
header{padding:20px;text-align:center;background:#0077cc;color:white;font-size:24px;font-weight:bold;}
.wrap{max-width:600px;margin:20px auto;padding:10px;}
.card{background:white;padding:15px;border-radius:8px;box-shadow:0 2px 6px rgba(0,0,0,0.1);margin-bottom:15px;}
input,select,button{width:100%;padding:10px;margin:5px 0;border-radius:5px;border:1px solid #ccc;font-size:14px;}
button{background:#0077cc;color:white;border:none;cursor:pointer;}
button:hover{background:#005fa3;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;background:#eee;border-top:1px solid #ccc;padding:10px;}
.nav div{cursor:pointer;text-align:center;font-size:14px;color:#333;}
.plan{border:1px solid #ccc;padding:10px;margin-bottom:8px;border-radius:5px;}
.plan h4{margin:0 0 5px 0;}
.referral-box{display:flex;gap:5px;margin-bottom:10px;}
.referral-box input{flex:1;}
.logout-btn{position:fixed;bottom:60px;left:50%;transform:translateX(-50%);background:#cc0000;color:white;padding:10px 20px;border-radius:20px;cursor:pointer;}
</style>
</head>
<body>

<header>VERBOSE - Rock Earn Branch</header>
<div class="wrap">

<!-- LOGIN -->
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
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card" style="display:none;">
<h3>Welcome, <span id="welcomeText"></span></h3>
<p>Member since: <span id="memberSince"></span></p>
<p>Balance: Rs <span id="balanceText">0</span></p>
<p>Daily Earnings: Rs <span id="dailyText">0</span></p>

<div class="referral-box">
<input id="referralLink" readonly/>
<button onclick="copyReferral()">Copy Link</button>
</div>

<h4>Support:</h4>
<p>Whatsapp: 03705519562<br>Email: Rock.earn92@gmail.com</p>

<h4>Activity:</h4>
<p>Coming soon...</p>
</div>

<!-- PLANS -->
<div id="plansCard" class="card" style="display:none;">
<h3>Plans</h3>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="depositCard" class="card" style="display:none;">
<h3>Deposit</h3>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<input id="depositNumber" readonly/>
<input id="depositAmount" readonly placeholder="Amount"/>
<input id="depositTx" placeholder="Transaction ID"/>
<input id="depositProof" type="file"/>
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawCard" class="card" style="display:none;">
<h3>Withdraw</h3>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<input id="withdrawUsername" readonly/>
<input id="withdrawAccount" placeholder="Account / Mobile Number"/>
<input id="withdrawAmount" placeholder="Amount"/>
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<div class="logout-btn" onclick="doLogout()">Logout</div>

<div class="nav">
<div onclick="nav('dashboardCard')"><i class="fas fa-home"></i><br>Home</div>
<div onclick="nav('plansCard')"><i class="fas fa-box"></i><br>Plans</div>
<div onclick="nav('depositCard')"><i class="fas fa-wallet"></i><br>Deposit</div>
<div onclick="nav('withdrawCard')"><i class="fas fa-hand-holding-usd"></i><br>Withdraw</div>
</div>

<script>
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
const KEY_DAILY='verbose_daily_';
const KEY_USER_PLANS='verbose_plans_';
let currentUser=localStorage.getItem(KEY_USER)||null;

// SAMPLE PLANS
let plans=[];
for(let i=1;i<=7;i++){
let invest=200*i;let days=20+Math.floor(Math.random()*51);
let total=invest*3;
plans.push({id:i,name:'Special Plan '+i,invest,total,days});
}

// FUNCTIONS
function fmt(n){return Number(n).toLocaleString();}
function doAuth(){
const mode=document.getElementById('authMode').value;
const u=document.getElementById('inputUser').value.trim();
const p=document.getElementById('inputPass').value.trim();
const ref=document.getElementById('referralInput').value.trim();
if(!u||!p){alert('Enter username & password');return;}
const credKey='verbose_cred_'+u;
if(mode==='signup'){
if(localStorage.getItem(credKey)){alert('Username exists');return;}
localStorage.setItem(credKey,p);localStorage.setItem(KEY_BAL+u,'0');localStorage.setItem(KEY_DAILY+u,'0');localStorage.setItem(KEY_USER_PLANS+u,'[]');
if(ref && localStorage.getItem('verbose_cred_'+ref)){let bal=Number(localStorage.getItem(KEY_BAL+ref)||0);bal+=30;localStorage.setItem(KEY_BAL+ref,bal);}
}else{
if(localStorage.getItem(credKey)!==p){alert('Wrong username/password');return;}
}
localStorage.setItem(KEY_USER,u);currentUser=u;afterLoginUI();
}

function afterLoginUI(){nav('dashboardCard');renderPlans();updateReferralLink();}
function updateReferralLink(){if(!currentUser) return;document.getElementById('referralLink').value=window.location.href+'?ref='+currentUser;}
function copyReferral(){const link=document.getElementById('referralLink');link.select();document.execCommand('copy');alert('Referral link copied!');}
function doLogout(){localStorage.removeItem(KEY_USER);currentUser=null;nav('loginCard');}
function nav(cardId){
['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>document.getElementById(id).style.display='none');
document.getElementById(cardId).style.display='block';
renderDashboard();
fillWithdrawUser();
updateReferralLink();
}

function renderDashboard(){if(!currentUser)return;
document.getElementById('welcomeText').innerText=currentUser;
document.getElementById('memberSince').innerText=new Date().toLocaleDateString();
document.getElementById('balanceText').innerText=fmt(Number(localStorage.getItem(KEY_BAL+currentUser)||0));
document.getElementById('dailyText').innerText=fmt(Number(localStorage.getItem(KEY_DAILY+currentUser)||0));
}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList');container.innerHTML='';
plans.forEach(plan=>{
const dailyProfit=Math.round(plan.total/plan.days);
const div=document.createElement('div');div.className='plan';
div.innerHTML=`<h4>${plan.name}</h4>
<p>Invest: Rs ${fmt(plan.invest)} | Total: Rs ${fmt(plan.total)} | Days: ${plan.days} | Daily: Rs ${fmt(dailyProfit)}</p>
<button onclick="buyPlan(${plan.id})">Buy Plan</button>`;
container.appendChild(div);
});
}

function buyPlan(id){if(!currentUser){alert('Login first');return;}
const plan=plans.find(p=>p.id===id);if(!plan){alert('Plan not available');return;}
let userPlans=JSON.parse(localStorage.getItem(KEY_USER_PLANS+currentUser)||'[]');
userPlans.push({planId:plan.id,dailyProfit:Math.round(plan.total/plan.days),lastCredit:Date.now()});
localStorage.setItem(KEY_USER_PLANS+currentUser,JSON.stringify(userPlans));
document.getElementById('depositAmount').value=plan.invest;updateDepositNumber();nav('depositCard');alert('Plan selected. Submit deposit. Admin will approve manually.');
}

// DEPOSIT
function updateDepositNumber(){const method=document.getElementById('depositMethod').value;document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';}
function submitDeposit(){if(!currentUser){alert('Login first');return;}
const tx=document.getElementById('depositTx').value.trim();const proof=document.getElementById('depositProof').files[0];const amount=Number(document.getElementById('depositAmount').value)||0;
if(!tx||!proof||!amount){alert('All fields required');return;}
alert('Deposit submitted! Admin will approve manually.');
document.getElementById('depositTx').value='';document.getElementById('depositProof').value='';nav('dashboardCard');}

// WITHDRAW
function fillWithdrawUser(){if(!currentUser) return;document.getElementById('withdrawUsername').value=currentUser;}
function submitWithdraw(){if(!currentUser){alert('Login first');return;}
const account=document.getElementById('withdrawAccount').value.trim();const amount=Number(document.getElementById('withdrawAmount').value);
if(!account||!amount){alert('Enter account and amount');return;}
alert('Withdrawal request submitted! Admin will approve manually.');
document.getElementById('withdrawAccount').value='';document.getElementById('withdrawAmount').value='';nav('dashboardCard');}
</script>

</body>
</html>
