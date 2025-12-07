<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
body{margin:0;font-family:Arial,sans-serif;background:#f5f5f5;color:#333;}
header{padding:20px;text-align:center;font-size:24px;font-weight:bold;background:#007BFF;color:white;}
.wrap{max-width:480px;margin:12px auto;padding:12px;}
.card{background:white;padding:12px;border-radius:12px;margin-bottom:12px;box-shadow:0 2px 6px rgba(0,0,0,0.1);}
input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:8px;border:1px solid #ccc;font-size:14px;}
.btn{background:#007BFF;border:none;color:white;font-weight:600;cursor:pointer;padding:10px;border-radius:8px;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:#f0f0f0;border-top:1px solid #ccc;}
.nav div{text-align:center;font-size:13px;cursor:pointer;}
.plan{display:flex;justify-content:space-between;align-items:center;padding:10px;border:1px solid #ccc;border-radius:8px;margin-bottom:8px;}
.plan button{padding:6px 10px;font-size:12px;}
.hidden{display:none;}
.alert-note{background:#ffe0e0;color:#900;padding:10px;margin-bottom:12px;border-radius:8px;text-align:center;}
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
<button class="btn" onclick="doAuth()">Submit</button>
<p style="font-size:12px;color:#666;">Tip: Data stored locally, same device & browser.</p>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<div class="alert-note">⚠️ Deposit/Withdrawal issues? Contact Support immediately (WhatsApp or Email)</div>
<div style="display:flex;justify-content:space-between;margin-bottom:12px;">
<div>
<div id="welcomeText" style="font-weight:bold;">Welcome —</div>
<div id="memberSince" style="font-size:12px;color:#666;">Member since —</div>
</div>
<div style="text-align:right;">
<div style="font-size:12px;color:#666;">Balance</div>
<div style="font-weight:bold;font-size:18px">Rs <span id="balanceText">0</span></div>
</div>
</div>
</div>

<!-- PLANS -->
<div id="plansCard" class="card hidden">
<h3>Plans</h3>
<div id="plansList"></div>
</div>

<!-- DEPOSIT -->
<div id="depositCard" class="card hidden">
<h3>Deposit</h3>
<label>Method</label>
<select id="depositMethod" onchange="updateDepositNumber()">
<option value="jazzcash">JazzCash — 03705519562</option>
<option value="easypaisa">EasyPaisa — 03379827882</option>
</select>
<label>Number</label>
<input id="depositNumber" readonly/>
<label>Amount (PKR)</label>
<input id="depositAmount" readonly/>
<label>Transaction ID</label>
<input id="depositTx" placeholder="Enter TX ID"/>
<label>Upload Proof</label>
<input id="depositProof" type="file"/>
<button class="btn" onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawCard" class="card hidden">
<h3>Withdrawal</h3>
<label>Username</label>
<input id="withdrawUsername" readonly/>
<label>Account / Mobile Number</label>
<input id="withdrawAccount" placeholder="Enter account or mobile number"/>
<label>Amount (PKR)</label>
<input id="withdrawAmount" placeholder="Enter amount"/>
<button class="btn" onclick="submitWithdraw()">Request Withdrawal</button>
</div>

<!-- NAV -->
<div class="nav">
<div onclick="nav('dashboardCard')"><i class="fas fa-home"></i><br>Home</div>
<div onclick="nav('plansCard')"><i class="fas fa-gift"></i><br>Plans</div>
<div onclick="nav('depositCard')"><i class="fas fa-wallet"></i><br>Deposit</div>
<div onclick="nav('withdrawCard')"><i class="fas fa-money-bill-wave"></i><br>Withdraw</div>
<div id="logoutBtn" onclick="doLogout()" class="hidden"><i class="fas fa-sign-out-alt"></i><br>Logout</div>
</div>

<script>
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
const KEY_USER_PLANS='verbose_plans_';
let currentUser=localStorage.getItem(KEY_USER)||null;

// Plans setup
let plans=[];
for(let i=1;i<=7;i++){plans.push({id:i,name:'Special Plan '+i,invest:200*i,total:(200*i)*3,days:20+Math.floor(Math.random()*51),offer:true});}
for(let i=8;i<=32;i++){let invest=3000+(i-8)*(30000-3000)/24;plans.push({id:i,name:'Plan '+(i-7),invest:Math.round(invest),total:Math.round(invest*2.5),days:20+Math.floor(Math.random()*51),offer:false});}
for(let i=33;i<=37;i++){plans.push({id:i,name:'Coming Soon',invest:0,total:0,days:0,offer:false});}

// AUTH
function doAuth(){
const mode=document.getElementById('authMode').value;
const u=document.getElementById('inputUser').value.trim();
const p=document.getElementById('inputPass').value.trim();
const credKey='verbose_cred_'+u;
if(!u||!p){alert('Enter username & password');return;}
if(mode==='signup'){
if(localStorage.getItem(credKey)){alert('Username exists');return;}
localStorage.setItem(credKey,p);localStorage.setItem(KEY_BAL+u,'0');localStorage.setItem(KEY_USER_PLANS+u,'[]');
}else{
if(localStorage.getItem(credKey)!==p){alert('Wrong username/password');return;}
}
localStorage.setItem(KEY_USER,u);currentUser=u;afterLoginUI();
}
function afterLoginUI(){nav('dashboardCard');renderPlans();renderDashboard();document.getElementById('logoutBtn').classList.remove('hidden');}
function doLogout(){localStorage.removeItem(KEY_USER);currentUser=null;nav('loginCard');document.getElementById('logoutBtn').classList.add('hidden');}

// NAV
function nav(cardId){['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>document.getElementById(id).classList.add('hidden'));document.getElementById(cardId).classList.remove('hidden');}

// DASHBOARD
function renderDashboard(){
if(!currentUser) return;
document.getElementById('welcomeText').innerText='Welcome, '+currentUser;
document.getElementById('memberSince').innerText='Member since: '+new Date().toLocaleDateString();
document.getElementById('balanceText').innerText=localStorage.getItem(KEY_BAL+currentUser)||0;
document.getElementById('withdrawUsername').value=currentUser;
}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList');
container.innerHTML='';
plans.forEach(plan=>{
const div=document.createElement('div');div.className='plan';
let dailyProfit=plan.days>0?Math.round(plan.total/plan.days):0;
div.innerHTML=`<div><strong>${plan.name}</strong><br>Invest: Rs ${plan.invest} · Total: Rs ${plan.total} · Days: ${plan.days} · Daily: Rs ${dailyProfit}</div>
<div>${plan.offer||plan.name!=='Coming Soon'?'<button onclick="buyPlan('+plan.id+')">Buy Now</button>':''}</div>`;
container.appendChild(div);
});
}

// BUY PLAN
function buyPlan(id){
if(!currentUser){alert('Login first');return;}
const plan=plans.find(p=>p.id===id);
if(!plan||plan.name==='Coming Soon'){alert('Plan not available');return;}
document.getElementById('depositAmount').value=plan.invest;
nav('depositCard');
alert('Deposit page opened. Enter TX ID & upload proof.');
}

// DEPOSIT
function updateDepositNumber(){
const method=document.getElementById('depositMethod').value;
document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function submitDeposit(){
if(!currentUser){alert('Login first');return;}
const amount=Number(document.getElementById('depositAmount').value)||0;
const tx=document.getElementById('depositTx').value.trim();
const proof=document.getElementById('depositProof').files[0];
if(!amount||!tx||!proof){alert('All fields required');return;}
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
bal+=amount;
localStorage.setItem(KEY_BAL+currentUser,bal);
alert('Deposit successful! Balance updated.');
document.getElementById('depositTx').value='';document.getElementById('depositProof').value='';
renderDashboard();nav('dashboardCard');
}

// WITHDRAW
function submitWithdraw(){
if(!currentUser){alert('Login first');return;}
const amount=Number(document.getElementById('withdrawAmount').value);
if(!amount){alert('Enter amount');return;}
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
if(amount>bal){alert('Insufficient balance');return;}
bal-=amount;localStorage.setItem(KEY_BAL+currentUser,bal);
alert('Withdrawal request successful! Balance updated.');
document.getElementById('withdrawAmount').value='';renderDashboard();nav('dashboardCard');
}
</script>

</body>
</html>
