<VERBOSE>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>VERBOSE</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#fff;color:#000;}
.container{max-width:600px;margin:20px auto;padding:15px;}
h1,h2,h3{margin:5px 0;}
.card{border:1px solid #ccc;border-radius:8px;padding:12px;margin-bottom:12px;background:#f9f9f9;}
button{padding:8px 12px;border:none;border-radius:6px;background:#007BFF;color:#fff;cursor:pointer;}
input,select{width:100%;padding:8px;margin:5px 0;border:1px solid #ccc;border-radius:6px;}
.nav{display:flex;justify-content:space-around;margin-top:20px;}
.nav div{cursor:pointer;color:#007BFF;}
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
<div id="plansList"></div>

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
const KEY_USER_PLANS='verbose_plans_';
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
afterLoginUI();
}

function afterLoginUI(){
document.getElementById('loginCard').style.display='none';
document.getElementById('dashboardCard').style.display='block';
document.getElementById('welcomeText').innerText=currentUser;
document.getElementById('balanceText').innerText=fmt(Number(localStorage.getItem(KEY_BAL+currentUser)));
document.getElementById('dailyText').innerText=fmt(Number(localStorage.getItem(KEY_DAILY+currentUser)));
renderPlans();
}

// PLANS
function renderPlans(){
const container=document.getElementById('plansList');container.innerHTML='';
plans.forEach(p=>{
const div=document.createElement('div');div.className='card';
div.innerHTML=`<strong>${p.name}</strong><br>Invest: Rs ${fmt(p.invest)} · Total: Rs ${fmt(p.total)} · Days: ${p.days}`;
container.appendChild(div);
});
}

// DEPOSIT
function submitDeposit(){
const amount=Number(document.getElementById('depositAmount').value);
const tx=document.getElementById('depositTx').value.trim();
if(!amount||!tx){alert('Enter amount & TX ID');return;}
alert('Deposit submitted! Admin will approve manually.');
document.getElementById('depositAmount').value='';
document.getElementById('depositTx').value='';
}

// WITHDRAW
function submitWithdraw(){
const account=document.getElementById('withdrawAccount').value.trim();
const amount=Number(document.getElementById('withdrawAmount').value);
if(!account||!amount){alert('Enter account & amount');return;}
alert('Withdrawal request submitted! Admin will approve manually.');
document.getElementById('withdrawAccount').value='';
document.getElementById('withdrawAmount').value='';
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
