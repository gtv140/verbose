<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#fff;color:#000;}
.container{max-width:700px;margin:20px auto;padding:15px;}
h1,h2,h3{margin:10px 0;}
.card{border:1px solid #ccc;border-radius:8px;padding:15px;margin-bottom:15px;background:#f9f9f9;}
input,select{width:100%;padding:8px;margin:5px 0;border:1px solid #ccc;border-radius:6px;}
button{padding:8px 12px;border:none;border-radius:6px;background:#007BFF;color:#fff;cursor:pointer;}
table{width:100%;border-collapse:collapse;margin-top:10px;}
table,th,td{border:1px solid #ccc;}
th,td{padding:8px;text-align:center;}
th{background:#e0e0e0;}
.muted{color:#555;font-size:13px;}
</style>
</head>
<body>

<div class="container">

<h1>VERBOSE</h1>

<!-- LOGIN -->
<div class="card" id="loginCard">
<h2>Login / Signup</h2>
<input id="inputUser" placeholder="Username"/>
<input id="inputPass" placeholder="Password" type="password"/>
<button onclick="doAuth()">Submit</button>
<p class="muted">Tip: Data stored locally in your browser.</p>
</div>

<!-- DASHBOARD -->
<div class="card" id="dashboardCard" style="display:none;">
<h2>Welcome, <span id="welcomeText"></span></h2>
<p>Balance: Rs <span id="balanceText">0</span></p>
<p>Daily Profit: Rs <span id="dailyText">0</span></p>

<h3>Plans</h3>
<div class="card">
<table id="plansTable">
<tr><th>Plan Name</th><th>Invest (Rs)</th><th>Total (Rs)</th><th>Days</th><th>Action</th></tr>
</table>
</div>

<h3>Deposit</h3>
<label>Method</label>
<select id="depositMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
</select>
<label>Number</label>
<input id="depositNumber" readonly value="03705519562"/>
<label>Amount</label>
<input id="depositAmount" placeholder="Enter amount"/>
<label>Transaction ID</label>
<input id="depositTx" placeholder="Enter TX ID"/>
<button onclick="submitDeposit()">Submit Deposit</button>

<h3>Withdraw</h3>
<label>Method</label>
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
</select>
<label>Account / Mobile Number</label>
<input id="withdrawAccount" placeholder="Enter account or mobile number"/>
<label>Amount</label>
<input id="withdrawAmount" placeholder="Enter amount"/>
<button onclick="submitWithdraw()">Request Withdrawal</button>
<p class="muted">⚠️ Withdrawal approved manually. Contact administration for urgent issues.</p>

<h3>Administration Contact</h3>
<p>Email: <strong>Rock.earn92@gmail.com</strong></p>
<p>WhatsApp: <strong>03705519562</strong></p>

<button onclick="doLogout()">Logout</button>
</div>

</div>

<script>
// STORAGE KEYS
const KEY_USER='verbose_user';
const KEY_BAL='verbose_balance_';
const KEY_DAILY='verbose_daily_';
const KEY_USER_PLANS='verbose_userplans_';
const KEY_DEPOSITS='verbose_deposits_';
const KEY_WITHDRAWS='verbose_withdraws_';
let currentUser=localStorage.getItem(KEY_USER)||null;

// SAMPLE PLANS
const plans=[
{id:1,name:'Special Plan 1',invest:200,total:600,days:20},
{id:2,name:'Special Plan 2',invest:400,total:1200,days:25},
{id:3,name:'Plan 3',invest:1000,total:2500,days:30},
{id:4,name:'Plan 4',invest:2000,total:5000,days:35},
{id:5,name:'Coming Soon',invest:0,total:0,days:0}
];

function fmt(n){return Number(n).toLocaleString();}

// AUTH
function doAuth(){
const u=document.getElementById('inputUser').value.trim();
const p=document.getElementById('inputPass').value.trim();
if(!u||!p){alert('Enter username & password');return;}
localStorage.setItem(KEY_USER,u);currentUser=u;
if(!localStorage.getItem(KEY_BAL+u)) localStorage.setItem(KEY_BAL+u,'0');
if(!localStorage.getItem(KEY_DAILY+u)) localStorage.setItem(KEY_DAILY+u,'0');
if(!localStorage.getItem(KEY_USER_PLANS+u)) localStorage.setItem(KEY_USER_PLANS+u,'[]');
if(!localStorage.getItem(KEY_DEPOSITS+u)) localStorage.setItem(KEY_DEPOSITS+u,'[]');
if(!localStorage.getItem(KEY_WITHDRAWS+u)) localStorage.setItem(KEY_WITHDRAWS+u,'[]');
afterLoginUI();
}

function afterLoginUI(){
document.getElementById('loginCard').style.display='none';
document.getElementById('dashboardCard').style.display='block';
document.getElementById('welcomeText').innerText=currentUser;
updateDashboard();
renderPlans();
}

// DASHBOARD UPDATE
function updateDashboard(){
document.getElementById('balanceText').innerText=fmt(Number(localStorage.getItem(KEY_BAL+currentUser)));
document.getElementById('dailyText').innerText=fmt(Number(localStorage.getItem(KEY_DAILY+currentUser)));
}

// PLANS
function renderPlans(){
const table=document.getElementById('plansTable');
table.innerHTML='<tr><th>Plan Name</th><th>Invest (Rs)</th><th>Total (Rs)</th><th>Days</th><th>Action</th></tr>';
plans.forEach(p=>{
const row=table.insertRow();
row.insertCell(0).innerText=p.name;
row.insertCell(1).innerText=p.invest>0?fmt(p.invest):'-';
row.insertCell(2).innerText=p.total>0?fmt(p.total):'-';
row.insertCell(3).innerText=p.days>0?p.days:'-';
const actionCell=row.insertCell(4);
if(p.invest>0) actionCell.innerHTML=`<button onclick="buyPlan(${p.id})">Buy</button>`; else actionCell.innerText='-';
});
}

// BUY PLAN
function buyPlan(id){
const plan=plans.find(p=>p.id===id);
if(!plan) return;
let userPlans=JSON.parse(localStorage.getItem(KEY_USER_PLANS+currentUser)||'[]');
const dailyProfit=Math.round(plan.total/plan.days);
userPlans.push({planId:plan.id,dailyProfit,lastCredit:Date.now(),invest:plan.invest,total:plan.total,days:plan.days});
localStorage.setItem(KEY_USER_PLANS+currentUser,JSON.stringify(userPlans));
document.getElementById('depositAmount').value=plan.invest;
alert(`Plan ${plan.name} selected. Submit your deposit. Admin will manually approve.`);
}

// DEPOSIT
function submitDeposit(){
const amount=Number(document.getElementById('depositAmount').value);
const tx=document.getElementById('depositTx').value.trim();
if(!amount||!tx){alert('Enter amount & TX ID');return;}
let deposits=JSON.parse(localStorage.getItem(KEY_DEPOSITS+currentUser)||'[]');
deposits.push({amount,tx,time:Date.now(),approved:false});
localStorage.setItem(KEY_DEPOSITS+currentUser,JSON.stringify(deposits));
alert('Deposit submitted! Admin will approve manually.');
document.getElementById('depositAmount').value='';
document.getElementById('depositTx').value='';
updateDashboard();
}

// WITHDRAW
function submitWithdraw(){
const account=document.getElementById('withdrawAccount').value.trim();
const amount=Number(document.getElementById('withdrawAmount').value);
if(!account||!amount){alert('Enter account & amount');return;}
let withdraws=JSON.parse(localStorage.getItem(KEY_WITHDRAWS+currentUser)||'[]');
withdraws.push({account,amount,time:Date.now(),approved:false});
localStorage.setItem(KEY_WITHDRAWS+currentUser,JSON.stringify(withdraws));
alert('Withdrawal request submitted! Admin will approve manually.');
document.getElementById('withdrawAccount').value='';
document.getElementById('withdrawAmount').value='';
updateDashboard();
}

// LOGOUT
function doLogout(){
localStorage.removeItem(KEY_USER);currentUser=null;
document.getElementById('loginCard').style.display='block';
document.getElementById('dashboardCard').style.display='none';
}
</script>

</body>
</html>
