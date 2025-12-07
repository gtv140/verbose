<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
<style>
body{margin:0;font-family:Arial,sans-serif;background:#f9f9f9;color:#333;}
header{padding:20px;text-align:center;font-weight:900;font-size:24px;background:#007bff;color:#fff;}
.wrap{max-width:480px;margin:12px auto;padding:12px;}
.card{background:#fff;padding:12px;border-radius:12px;margin-bottom:12px;box-shadow:0 2px 6px rgba(0,0,0,0.1);}
input,select,button{width:100%;padding:10px;margin-top:6px;border-radius:6px;border:1px solid #ccc;font-size:14px;}
button{cursor:pointer;background:#007bff;color:#fff;border:none;}
.nav{position:fixed;bottom:0;left:0;right:0;display:flex;justify-content:space-around;padding:10px;background:#eee;border-top:1px solid #ccc;}
.nav div{text-align:center;font-size:12px;cursor:pointer;}
.plan{display:flex;justify-content:space-between;align-items:center;padding:10px;border:1px solid #ddd;border-radius:8px;margin-bottom:8px;background:#fafafa;}
.plan .actions button{padding:6px 12px;}
.alert-box{background:#ffefc2;padding:10px;border-radius:6px;margin-bottom:12px;font-weight:600;text-align:center;}
.copy-btn{width:auto;padding:6px 12px;margin-left:4px;background:#28a745;color:#fff;border:none;border-radius:6px;cursor:pointer;}
.admin-box a{display:block;color:#007bff;text-decoration:none;margin-bottom:4px;}
.hidden{display:none;}
</style>
</head>
<body>
<header>VERBOSE</header>
<div class="wrap">

<!-- ALERT -->
<div class="alert-box">⚠️ If any issue occurs with deposit/withdrawal, contact administration immediately.</div>

<!-- LOGIN -->
<div id="loginCard" class="card">
<h3>Login / Signup</h3>
<select id="authMode">
<option value="login">Login</option>
<option value="signup">Signup</option>
</select>
<input id="inputUser" placeholder="Username"/>
<input id="inputPass" placeholder="Password" type="password"/>
<button onclick="doAuth()"><i class="fas fa-sign-in-alt"></i> Submit</button>
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<div style="font-weight:800;font-size:18px;">Welcome, <span id="welcomeText"></span></div>
<div class="admin-box">
<strong>Administration:</strong>
<a href="tel:03705519562"><i class="fas fa-phone"></i> WhatsApp: 03705519562</a>
<a href="mailto:rock.earn92@gmail.com"><i class="fas fa-envelope"></i> Email: rock.earn92@gmail.com</a>
<p>Company: VERBOSE | Branch of Rock Earn | Trusted by millions of users</p>
</div>
<div style="margin-top:12px;">
<strong>Balance:</strong> Rs <span id="balanceText">0</span>
</div>
<button id="logoutBtn" onclick="doLogout()">Logout <i class="fas fa-sign-out-alt"></i></button>
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
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<label>Number</label>
<div style="display:flex;">
<input id="depositNumber" readonly/>
<button class="copy-btn" onclick="copyNumber()">Copy</button>
</div>
<label>Amount (PKR)</label>
<input id="depositAmount" readonly/>
<label>Transaction ID</label>
<input id="depositTx" placeholder="Enter TX ID"/>
<label>Upload Proof</label>
<input id="depositProof" type="file"/>
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawCard" class="card hidden">
<h3>Withdraw</h3>
<label>Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<label>Account / Mobile</label>
<input id="withdrawAccount" placeholder="Enter account or mobile number"/>
<label>Amount (PKR)</label>
<input id="withdrawAmount" placeholder="Enter amount"/>
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

</div>

<div class="nav">
<div onclick="nav('dashboardCard')"><i class="fas fa-home"></i><br>Home</div>
<div onclick="nav('plansCard')"><i class="fas fa-box"></i><br>Plans</div>
<div onclick="nav('depositCard')"><i class="fas fa-wallet"></i><br>Deposit</div>
<div onclick="nav('withdrawCard')"><i class="fas fa-money-bill-wave"></i><br>Withdraw</div>
</div>

<script>
// Storage
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
let currentUser=localStorage.getItem(KEY_USER)||null;

// Plans
const plans=[
{ id:1, name:'Special Plan 1', invest:200, total:600, days:20 },
{ id:2, name:'Special Plan 2', invest:400, total:1200, days:25 },
{ id:3, name:'Special Plan 3', invest:600, total:1800, days:30 },
{ id:4, name:'Special Plan 4', invest:800, total:2400, days:35 },
{ id:5, name:'Special Plan 5', invest:1000, total:3000, days:40 },
{ id:6, name:'Special Plan 6', invest:1200, total:3600, days:45 },
{ id:7, name:'Special Plan 7', invest:1500, total:4500, days:50 },
{ id:8, name:'Plan 1', invest:5000, total:12500, days:50 },
{ id:9, name:'Plan 2', invest:8000, total:20000, days:55 },
{ id:10, name:'Plan 3', invest:10000, total:25000, days:60 },
{ id:11, name:'Plan 4', invest:12000, total:30000, days:65 },
{ id:12, name:'Plan 5', invest:15000, total:37500, days:70 },
{ id:13, name:'Plan 6', invest:18000, total:45000, days:75 },
{ id:14, name:'Plan 7', invest:20000, total:50000, days:80 },
{ id:15, name:'Plan 8', invest:22000, total:55000, days:85 },
{ id:16, name:'Plan 9', invest:24000, total:60000, days:90 },
{ id:17, name:'Plan 10', invest:26000, total:65000, days:95 },
{ id:18, name:'Plan 11', invest:28000, total:70000, days:100 },
{ id:19, name:'Plan 12', invest:30000, total:75000, days:105 },
{ id:20, name:'Coming Soon', invest:0, total:0, days:0 },
{ id:21, name:'Coming Soon', invest:0, total:0, days:0 },
{ id:22, name:'Coming Soon', invest:0, total:0, days:0 },
{ id:23, name:'Coming Soon', invest:0, total:0, days:0 },
{ id:24, name:'Coming Soon', invest:0, total:0, days:0 },
{ id:25, name:'Coming Soon', invest:0, total:0, days:0 },
];

// AUTH
function doAuth(){
const mode=document.getElementById('authMode').value;
const u=document.getElementById('inputUser').value.trim();
const p=document.getElementById('inputPass').value.trim();
if(!u||!p){alert('Enter username & password');return;}
const credKey='verbose_cred_'+u;
if(mode==='signup'){
if(localStorage.getItem(credKey)){alert('Username exists');return;}
localStorage.setItem(credKey,p);
localStorage.setItem(KEY_BAL+u,'0');
}else{
if(localStorage.getItem(credKey)!==p){alert('Wrong username/password');return;}
}
localStorage.setItem(KEY_USER,u);
currentUser=u;
afterLoginUI();
}

function afterLoginUI(){
nav('dashboardCard');
renderPlans();
updateDashboard();
}

function doLogout(){localStorage.removeItem(KEY_USER);currentUser=null;nav('loginCard');}

function nav(cardId){
['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>document.getElementById(id).classList.add('hidden'));
document.getElementById(cardId).classList.remove('hidden');
updateDashboard();
}

// DASHBOARD
function updateDashboard(){
if(!currentUser) return;
document.getElementById('welcomeText').innerText=currentUser;
document.getElementById('balanceText').innerText=localStorage.getItem(KEY_BAL+currentUser)||0;
}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList');
container.innerHTML='';
plans.forEach(plan=>{
const div=document.createElement('div');div.className='plan';
let daily=Math.round(plan.total/plan.days)||0;
div.innerHTML=`<div>
<strong>${plan.name}</strong><br>
Invest: Rs ${plan.invest} | Total: Rs ${plan.total} | Days: ${plan.days} | Daily: Rs ${daily}
</div>
<div class="actions">
<button onclick="selectPlan(${plan.id})">Buy Now <i class="fas fa-shopping-cart"></i></button>
</div>`;
container.appendChild(div);
});
}

function selectPlan(id){
const plan=plans.find(p=>p.id===id);
if(!plan || plan.name.includes('Coming Soon')){alert('Plan not available');return;}
document.getElementById('depositAmount').value=plan.invest;
nav('depositCard');
}

// DEPOSIT
function updateDepositNumber(){
const method=document.getElementById('depositMethod').value;
document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}
function copyNumber(){
const num=document.getElementById('depositNumber');
num.select();document.execCommand('copy');alert('Number copied!');
}
function submitDeposit(){
if(!currentUser){alert('Login first');return;}
const amount=Number(document.getElementById('depositAmount').value);
const tx=document.getElementById('depositTx').value.trim();
const proof=document.getElementById('depositProof').files[0];
if(!tx||!proof||!amount){alert('All fields required');return;}
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
bal+=amount;
localStorage.setItem(KEY_BAL+currentUser,bal);
alert('Deposit successful!');
nav('dashboardCard');
}

// WITHDRAW
function submitWithdraw(){
if(!currentUser){alert('Login first');return;}
const amount=Number(document.getElementById('withdrawAmount').value);
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
if(amount>bal){alert('Insufficient balance');return;}
bal-=amount;
localStorage.setItem(KEY_BAL+currentUser,bal);
alert('Withdrawal requested!');
nav('dashboardCard');
}
</script>
</body>
</html>
