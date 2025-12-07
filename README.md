<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
body { margin:0; font-family: Arial, sans-serif; background:#f5f5f5; color:#333; }
header { padding:20px; text-align:center; font-weight:900; font-size:24px; background:#0077cc; color:#fff; }
.wrap { max-width:600px; margin:20px auto; padding:20px; background:#fff; border-radius:10px; box-shadow:0 2px 8px rgba(0,0,0,0.1); }
.card { margin-bottom:20px; padding:15px; border:1px solid #ddd; border-radius:10px; background:#fff; }
input, select, button { width:100%; padding:10px; margin-top:8px; border-radius:6px; border:1px solid #ccc; font-size:14px; }
button { background:#0077cc; color:#fff; border:none; cursor:pointer; }
button:hover { background:#005fa3; }
.nav { position:fixed; bottom:0; left:0; right:0; display:flex; justify-content:space-around; padding:10px; background:#eee; border-top:1px solid #ccc; }
.nav div { cursor:pointer; font-size:14px; text-align:center; }
.hidden { display:none; }
.plan { padding:10px; border:1px solid #ddd; border-radius:6px; margin-bottom:10px; }
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
</div>

<!-- DASHBOARD -->
<div id="dashboardCard" class="card hidden">
<h3>Welcome, <span id="welcomeText"></span></h3>
<p>Balance: Rs <span id="balanceText">0</span></p>
<div id="userPlans"></div>
<button onclick="doLogout()">Logout</button>
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
<select id="depositMethod">
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
<button onclick="submitDeposit()">Submit Deposit</button>
</div>

<!-- WITHDRAW -->
<div id="withdrawCard" class="card hidden">
<h3>Withdraw</h3>
<label>Username</label>
<input id="withdrawUsername" readonly/>
<label>Account / Mobile Number</label>
<input id="withdrawAccount" placeholder="Enter account or mobile number"/>
<label>Amount (PKR)</label>
<input id="withdrawAmount" placeholder="Enter amount"/>
<button onclick="submitWithdraw()">Request Withdrawal</button>
</div>

</div>

<div class="nav">
<div onclick="nav('dashboardCard')">Home</div>
<div onclick="nav('plansCard')">Plans</div>
<div onclick="nav('depositCard')">Deposit</div>
<div onclick="nav('withdrawCard')">Withdraw</div>
</div>

<script>
// STORAGE KEYS
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
const KEY_USER_PLANS='verbose_plans_';
let currentUser=localStorage.getItem(KEY_USER)||null;
const plans=[
{ id:1,name:'Special Plan 1',invest:200,days:20,multiplier:3 },
{ id:2,name:'Special Plan 2',invest:400,days:25,multiplier:3 },
{ id:3,name:'Special Plan 3',invest:600,days:30,multiplier:3 },
{ id:4,name:'Special Plan 4',invest:800,days:35,multiplier:3 },
{ id:5,name:'Special Plan 5',invest:1000,days:40,multiplier:3 },
{ id:6,name:'Special Plan 6',invest:1500,days:50,multiplier:3 },
{ id:7,name:'Special Plan 7',invest:3000,days:60,multiplier:3 },
{ id:8,name:'Plan 8',invest:3500,days:20,multiplier:2.5 },
{ id:9,name:'Plan 9',invest:5000,days:30,multiplier:2.5 },
// ... aur baki normal plans
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
localStorage.setItem(KEY_BAL+u,0);
localStorage.setItem(KEY_USER_PLANS+u,'[]');
}else{if(localStorage.getItem(credKey)!==p){alert('Wrong username/password');return;}}
localStorage.setItem(KEY_USER,u);currentUser=u;afterLoginUI();
}

function afterLoginUI(){
nav('dashboardCard');
renderDashboard();
renderPlans();
updateDepositNumber();
}

function nav(cardId){
['loginCard','dashboardCard','plansCard','depositCard','withdrawCard'].forEach(id=>document.getElementById(id).classList.add('hidden'));
document.getElementById(cardId).classList.remove('hidden');
fillWithdrawUser();
renderDashboard();
}

function renderDashboard(){
if(!currentUser)return;
document.getElementById('welcomeText').innerText=currentUser;
document.getElementById('balanceText').innerText=localStorage.getItem(KEY_BAL+currentUser)||0;
}

function renderPlans(){
const container=document.getElementById('plansList');container.innerHTML='';
plans.forEach(plan=>{
const div=document.createElement('div');div.className='plan';
const daily=Math.round(plan.invest*plan.multiplier/plan.days);
div.innerHTML=`<strong>${plan.name}</strong><br>Invest: Rs ${plan.invest} · Total: Rs ${plan.invest*plan.multiplier} · Days: ${plan.days} · Daily: Rs ${daily}<br>
<button onclick="buyPlan(${plan.id})">Buy Now</button>`;
container.appendChild(div);
});
}

function buyPlan(id){
const plan=plans.find(p=>p.id===id);
if(!plan){alert('Plan not found');return;}
document.getElementById('depositAmount').value=plan.invest;
nav('depositCard');
alert(`Selected ${plan.name}. Submit deposit to complete.`);
}

function updateDepositNumber(){
const method=document.getElementById('depositMethod').value;
document.getElementById('depositNumber').value=method==='jazzcash'?'03705519562':'03379827882';
}

function submitDeposit(){
if(!currentUser){alert('Login first');return;}
const tx=document.getElementById('depositTx').value.trim();
const proof=document.getElementById('depositProof').files[0];
const amount=Number(document.getElementById('depositAmount').value);
if(!tx||!proof||!amount){alert('All fields required');return;}
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
bal+=amount;
localStorage.setItem(KEY_BAL+currentUser,bal);
alert('Deposit completed! Balance updated.');
document.getElementById('depositTx').value='';document.getElementById('depositProof').value='';
nav('dashboardCard');
}

function fillWithdrawUser(){if(currentUser)document.getElementById('withdrawUsername').value=currentUser;}

function submitWithdraw(){
if(!currentUser){alert('Login first');return;}
const amount=Number(document.getElementById('withdrawAmount').value);
let bal=Number(localStorage.getItem(KEY_BAL+currentUser)||0);
if(!amount||amount>bal){alert('Invalid amount');return;}
bal-=amount;
localStorage.setItem(KEY_BAL+currentUser,bal);
alert('Withdrawal requested. Balance updated.');
renderDashboard();
}

function doLogout(){currentUser=null;localStorage.removeItem(KEY_USER);nav('loginCard');}
</script>
</body>
</html>
